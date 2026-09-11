# File I/O

C's file model is a **stream**: a sequence of bytes with a cursor. You `fopen` a file, read/write through the cursor, and `fclose` it. C gives you three levels — character, formatted, and raw blocks — plus the three standard streams already open for you.

**The Intuition:** A file is a tape: you read from where the cursor is, the cursor advances. `FILE *` is a handle to the stream (with its internal buffer and position). Text mode translates newlines (`\n` ↔ `\r\n` on Windows); binary mode is raw bytes. Every `fopen` must be matched by an `fclose` — and you *must check the return of fopen* (it's NULL on failure).

## Opening & closing

```c
#include <stdio.h>

FILE *f = fopen("data.txt", "r");    // "r" read, "w" write(truncate!),
if (f == NULL) {                     // "a" append, "r+" read+write,
    perror("open");                  // "rb"/"wb" binary
    return 1;
}
// ... read/write ...
fclose(f);                           // flush + release — ALWAYS

// The three standard streams — already open:
FILE *stdin, *stdout, *stderr;
```

## Reading

```c
// Character by character
int c = fgetc(f);                    // returns int (not char!) — can hold EOF
while ((c = fgetc(f)) != EOF) {
    putchar(c);
}

// Line by line
char line[256];
while (fgets(line, sizeof(line), f)) {   // reads a line INCLUDING \n, null-terminated
    // fgets stops at newline, at sizeof-1 chars, or EOF
    printf("Line: %s", line);
}

// Formatted
int x; double y;
fscanf(f, "%d %lf", &x, &y);         // like scanf, but from a file

// Raw blocks
unsigned char buf[1024];
size_t n = fread(buf, 1, sizeof(buf), f);   // returns items actually read
```

## Writing

```c
fputc('A', f);
fputs("hello\n", f);
fprintf(f, "x = %d, y = %.2f\n", x, y);    // formatted — the workhorse
fwrite(data, sizeof(int), 10, f);           // raw block write

// Flush & seek
fflush(f);              // push buffered data to the OS now
fseek(f, 0, SEEK_SET);  // rewind to start
fseek(f, -10, SEEK_END);// 10 bytes before end
long pos = ftell(f);    // current position
rewind(f);              // fseek(f, 0, SEEK_SET)
```

## Reading the whole file — the classic patterns

```c
// 1. Line by line (memory-friendly, the common case)
FILE *f = fopen("big.txt", "r");
if (!f) { perror("open"); return; }
char line[1024];
while (fgets(line, sizeof(line), f)) {
    // process line — line has \n; strip with line[strcspn(line, "\n")] = 0;
}
fclose(f);

// 2. Whole file into a dynamically sized buffer
FILE *f = fopen("data.bin", "rb");
fseek(f, 0, SEEK_END);
long size = ftell(f);          // file size in bytes
rewind(f);
unsigned char *buf = malloc(size + 1);
if (buf) {
    fread(buf, 1, size, f);    // read it all
    buf[size] = '\0';          // if text, null-terminate
}
fclose(f);
```

## The `feof` trap

```c
// WRONG — feof doesn't become true until AFTER a read hits EOF:
while (!feof(f)) {
    fgets(line, sizeof(line), f);
    // process line...   ← processes one too many (the last read returned NULL)
}

// RIGHT — check the READ's return value:
while (fgets(line, sizeof(line), f)) {
    // process line — only runs for successfully read lines
}
```

## Binary I/O — structs to disk

```c
typedef struct { int id; double score; } Record;

// Write an array of records
Record records[10];
FILE *f = fopen("recs.dat", "wb");
fwrite(records, sizeof(Record), 10, f);
fclose(f);

// Read them back
Record loaded[10];
FILE *f = fopen("recs.dat", "rb");
fread(loaded, sizeof(Record), 10, f);
fclose(f);
```

**Caveat:** Writing structs raw is fast but **not portable** — padding differs across compilers/architectures, and int/double sizes differ. For portable formats: use packing, fixed-width types (`int32_t`), and explicit serialization.

## Errors — the return values

```c
FILE *f = fopen("nope.txt", "r");
if (f == NULL) {
    perror("fopen");       // prints "fopen: No such file or directory"
    fprintf(stderr, "error code: %d\n", errno);   // errno from <errno.h>
    return 1;
}
// fopen/fclose/fread/fwrite return NULL/0 on failure — always check
// the standard idiom: 
if (fclose(f) != 0) { /* write failed on flush */ }
```

---

**Setup:** Copy a file, byte by byte.

**Solution:**
```c
void copy_file(const char *src, const char *dst) {
    FILE *in = fopen(src, "rb");
    if (!in) { perror(src); return; }
    FILE *out = fopen(dst, "wb");
    if (!out) { perror(dst); fclose(in); return; }

    int c;
    while ((c = fgetc(in)) != EOF)
        fputc(c, out);

    fclose(in);
    fclose(out);
}
```

**Key insight:** Binary mode (`"rb"`/`"wb"`) prevents newline translation — essential for exact copies. `fgetc` returns `int` specifically so it can represent `EOF` (-1) separately from every possible byte. Always close both files on every path.

---

**Setup:** Count lines, words, and characters in a file (like `wc`).

**Solution:**
```c
void wc(const char *path) {
    FILE *f = fopen(path, "r");
    if (!f) { perror(path); return; }
    long lines = 0, words = 0, chars = 0;
    int in_word = 0, c;
    while ((c = fgetc(f)) != EOF) {
        chars++;
        if (c == '\n') lines++;
        if (c == ' ' || c == '\n' || c == '\t') in_word = 0;
        else if (!in_word) { in_word = 1; words++; }
    }
    fclose(f);
    printf("%ld %ld %ld\n", lines, words, chars);
}
```

**Key insight:** The `in_word` state flag — the same DFA-style logic as the word counter in the Arrays & Strings note. Reading char-by-char keeps memory O(1) regardless of file size.

---

**Setup:** Read numbers from a file and print their sum.

**Solution:**
```c
FILE *f = fopen("numbers.txt", "r");
if (!f) { perror("numbers.txt"); return; }
double x, sum = 0;
while (fscanf(f, "%lf", &x) == 1)    // returns # of items matched
    sum += x;
fclose(f);
printf("Sum: %.2f\n", sum);
```

**Key insight:** `fscanf` returns the number of items successfully assigned — comparing against `== 1` makes the loop stop at EOF *or* at malformed input. This is the robust "read until done" pattern.

---

**Setup:** Write a struct array to a file, then read it back.

**Solution:**
```c
typedef struct { char name[32]; int score; } Player;

Player team[3] = {{"Ada", 92}, {"Bob", 85}, {"Cy", 97}};
FILE *f = fopen("team.dat", "wb");
fwrite(team, sizeof(Player), 3, f);
fclose(f);

Player loaded[3];
f = fopen("team.dat", "rb");
fread(loaded, sizeof(Player), 3, f);
fclose(f);
// loaded now mirrors team exactly (on the same platform)
```

**Key insight:** `fwrite`/`fread` with `sizeof(Player)` move whole records in one call. Fast and simple for local persistence — but remember the portability caveat (padding, endianness).

---

## Practice (try before peeking)

1. Why does `fgetc` return `int` and not `char`?
2. What does `fopen("x.txt", "w")` do to an existing file?
3. `while (!feof(f))` — what's the bug?

<details><summary>Answers</summary>

1. `char` can't represent `EOF` (which is -1) distinctly from a valid byte (0–255). `int` holds both.
2. It **truncates** it to zero bytes immediately — existing content is gone. Use `"a"` or `"r+"` to preserve.
3. `feof` is only set *after* a read attempt crosses the end — the loop processes one extra (garbage or uninitialized) line.

</details>

---

**Common traps:**
- Forgetting to check `fopen` for NULL → segfault on `fgetc(NULL)`
- `feof` misuse (see above) — check read return values, not `feof`
- Text vs binary mode — newline translation corrupts binary data without `"b"`
- `fgets` leaves the `\n` in the buffer; `scanf` leaves it in the stream — mixed `fgets`/`fscanf` causes skipped reads
- Unclosed files → resource leak (and on Windows, locked files)

---
