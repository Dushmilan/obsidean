# Build Tools & Debugging

Real C projects are multi-file and built with `gcc`/`clang`, organized by `make` (or CMake), and debugged with `gdb` and `valgrind`. The compiler's warnings are your first debugger — `-Wall -Wextra -Werror` catches most bugs before they run.

**The Intuition:** Compilation is a pipeline: preprocess → compile → assemble → link. Each `.c` becomes an object file; the linker merges objects and libraries into the executable. The toolchain's warnings are a free static analyzer — treat them as errors. When a program misbehaves, `gdb` lets you pause time (breakpoints), inspect memory, and step through your code; `valgrind` finds memory errors you can't see.

## The build pipeline

```bash
# 1. Preprocess: expand #include/#define (text transformation)
gcc -E main.c -o main.i

# 2. Compile to assembly
gcc -S main.c -o main.s

# 3. Assemble to object file (machine code, unresolved references)
gcc -c main.c -o main.o

# 4. Link with other objects + libraries → executable
gcc main.o point.o -o prog -lm

# One command does all four:
gcc main.c point.c -o prog
```

## Compiler flags you should always use

```bash
gcc -Wall -Wextra -Werror main.c -o prog
# -Wall    common warnings (unused vars, printf mismatches)
# -Wextra  more (sign comparisons, missing-field-initializers)
# -Werror  treat warnings as errors — forces you to fix them
# -g       debug symbols (required for gdb, valgrind)
# -O2      optimization (debug builds: -O0 for predictable stepping)
# -std=c11  language standard (or c17/c23)
# -pedantic  stricter standards conformance
# -fsanitize=address  runtime memory checker (fast, catches overflows)

# A strong baseline:
gcc -std=c11 -Wall -Wextra -Werror -g -fsanitize=address,undefined main.c -o prog
```

## Makefiles — build automation

```makefile
# Makefile
CC      = gcc
CFLAGS  = -std=c11 -Wall -Wextra -Werror -g
TARGET  = prog
OBJS    = main.o point.o

$(TARGET): $(OBJS)              # rule: target: prerequisites
	$(CC) $(OBJS) -o $(TARGET) -lm

%.o: %.c                        # pattern rule: how to build any .o
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(TARGET)

.PHONY: clean
```

```bash
make          # builds prog — only recompiles CHANGED files (by timestamp)
make clean    # removes objects
```

**Why make:** it tracks which files changed and recompiles only those — a 100-file project rebuilds in seconds instead of minutes.

## gdb — the debugger

```bash
gcc -g -Wall main.c -o prog
gdb ./prog
```

```gdb
(gdb) break main.c:10        # breakpoint at line 10
(gdb) break function_name    # breakpoint at function entry
(gdb) run                    # start
(gdb) next                   # step over (n)
(gdb) step                   # step into (s)
(gdb) continue               # run to next breakpoint (c)
(gdb) print x                # inspect variable (p)
(gdb) print &x               # address
(gdb) list                   # show source around current line
(gdb) bt                     # backtrace — the call stack (when you crash)
(gdb) info locals            # all local variables
(gdb) watch y                # break when y changes
(gdb) quit
```

**Debugging a segfault — the essential flow:**
```bash
gcc -g -Wall main.c -o prog
gdb ./prog
(gdb) run
# Program received signal SIGSEGV, Segmentation fault.
# main.c:12 ... arr[i] = x;   ← the crashing line
(gdb) bt                      # how did we get here?
# The backtrace shows the call chain — the bug is usually in a caller
(gdb) print i                 # is i out of bounds? print arr, i, n
```

## The classic debugging workflow

1. **Reproduce** the bug reliably — smallest input that triggers it
2. **Find the crash line** — gdb backtrace, or run with sanitizers
3. **Check the state** — print variables at the crash: index, bounds, pointers
4. **Fix the cause**, not the symptom — the crash line is often a *victim* (heap corruption elsewhere surfaces here)
5. **Add regression tests** — ensure the bug never returns

## Sanitizers — automated memory checking

```bash
# AddressSanitizer — use-after-free, buffer overflow, leaks
gcc -fsanitize=address -g main.c -o prog && ./prog
# → "ERROR: AddressSanitizer: heap-buffer-overflow on address ..."

# UndefinedBehaviorSanitizer — overflow, misaligned access, null deref
gcc -fsanitize=undefined -g main.c -o prog && ./prog

# Combine with -g for stack traces pointing at the exact line
```

## valgrind — memory error detection (slower but thorough)

```bash
gcc -g -Wall main.c -o prog
valgrind --leak-check=full --track-origins=yes ./prog
```

```text
==12345== Invalid write of size 4          ← writing past an array
==12345==    at 0x...: main (main.c:12)
==12345== 40 bytes in 1 blocks are definitely lost   ← a leak
==12345==    at 0x...: malloc (main.c:9)
```

| Tool | Catches | Speed |
|------|---------|-------|
| `-fsanitize=address` | overflows, use-after-free, leaks | ~2× slower |
| `-fsanitize=undefined` | overflow, misalign, null | ~1.2× slower |
| `valgrind` | everything above + uninitialized reads | ~20× slower |
| gdb | inspect live state, backtraces | n/a |

## Defensive coding practices

```c
// Assert — document invariants, catch impossible states early
#include <assert.h>
assert(n > 0);
assert(idx < capacity);      // removed under NDEBUG

// Check all system calls
FILE *f = fopen(...);
if (!f) { perror("fopen"); return; }

// Initialize everything
int x = 0;                    // never read uninitialized
char buf[100] = {0};          // zero-fill

// Bounds-check arrays
if (i >= 0 && i < n) arr[i] = 5;
```

---

**Setup:** A program segfaults. Walk through the debug flow.

**Solution:**
```c
// main.c — the bug
#include <string.h>
int main(void) {
    char *s = "hello";       // string literal — READ-ONLY
    strcpy(s, "world!");     // writes to read-only memory + overflow → segfault
    return 0;
}
```
```bash
gcc -g -Wall main.c -o prog && gdb ./prog
(gdb) run
# SIGSEGV at strcpy (inlined into main.c:6)
(gdb) p s            # $1 = 0x... "hello" — points at a literal
(gdb) bt             # main → __strcpy_ssse3 → ...
```
**Fix:** `char s[32]; strcpy(s, "world!");`

**Key insight:** The crash site (`strcpy`) is a library function — the *cause* is the caller passing read-only or undersized memory. The backtrace + inspecting `s` reveals it. This is why you debug the *state*, not just the line.

---

**Setup:** Find the memory error in a program that "works but acts weird."

**Solution:**
```c
int main(void) {
    int *arr = malloc(5 * sizeof(int));
    for (int i = 0; i <= 5; i++)    // writes arr[5] — ONE PAST the end
        arr[i] = i * i;
    free(arr);                       // corrupts allocator bookkeeping
    return 0;
}
```
```bash
gcc -g -fsanitize=address main.c -o prog && ./prog
# ERROR: AddressSanitizer: heap-buffer-overflow on address 0x... at pc 0x... 
#     #0 ... in main main.c:7      ← the exact write
```

**Key insight:** The off-by-one (`<= 5`) silently corrupts memory that "happens to work" until something else breaks — the *classic* heisenbug. ASan pinpoints the exact write. This is why `-fsanitize=address` belongs in every dev build.

---

**Setup:** A Makefile that rebuilds only what changed.

**Solution:**
```makefile
CC = gcc
CFLAGS = -std=c11 -Wall -Wextra -Werror -g
TARGET = prog
OBJS = main.o point.o

$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $(TARGET) -lm

main.o: main.c point.h
point.o: point.c point.h

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(TARGET)

.PHONY: clean
```
```bash
make          # first build: compiles both .c files
make          # second build: "make: Nothing to be done" — no changes
# edit point.c
make          # recompiles ONLY point.o, then relinks
```

**Key insight:** The dependency lines (`main.o: main.c point.h`) tell make when to rebuild — change a header, every file that includes it recompiles. Correct dependencies prevent "it works after make clean but not after incremental build" bugs.

---

## Practice (try before peeking)

1. Why does a build work with `-O0` but break with `-O2`?
2. `gdb` prints the crash line, but the fix isn't there — where do you look?
3. What's the first flag to add when a bug appears only intermittently?

<details><summary>Answers</summary>

1. Optimization often *exposes* latent UB — the optimizer assumed it couldn't happen and transformed the code. `-fsanitize=undefined` finds it.
2. The crash line is where the corruption *surfaces*, not where it started. Check the backtrace for who called, and inspect the state at the crash.
3. `-fsanitize=address,undefined` — it catches the root cause (overflow/use-after-free) with a precise stack trace. Or compile with `-g` and run under valgrind.

</details>

---

**Common traps:**
- Building without `-Wall -Wextra -Werror` — you're ignoring free bug reports
- Debugging optimized builds (`-O2` without `-g`) — line numbers and variables lie
- Fixing the crash line instead of the cause — heap corruption surfaces far from the writer
- Not using sanitizers on test runs — most memory bugs are invisible until they're not
- `make clean && make` "fixes" a problem — it usually hides a missing dependency in your Makefile

---
