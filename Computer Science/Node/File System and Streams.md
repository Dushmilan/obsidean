# File System & Streams

Node's `fs` module reads and writes files (promise, callback, and sync APIs). **Streams** are the memory-friendly way to move *chunks* of data instead of whole buffers — essential for large files, HTTP bodies, and pipelines. Understanding **Buffers** (raw binary memory) and **backpressure** (slow consumer, fast producer) separates solid Node code from fragile code.

**The Intuition:** Reading a 2 GB file with `readFile` is like drinking a lake through a fire hose — the whole thing lands in memory at once, and the app can choke. A **stream** drinks through a straw, sip by sip, processing each sip as it arrives — so a 2 GB file only ever occupies a few KB of memory at a time. Backpressure is the tap: if your straw is slow, the stream tells the producer to slow down.

## The fs APIs — three flavors

```js
// 1. Synchronous — blocks the loop (fine for scripts, not servers):
import { readFileSync } from 'node:fs';
const data = readFileSync('config.json', 'utf8');

// 2. Callback — classic async:
import { readFile } from 'node:fs';
readFile('config.json', 'utf8', (err, data) => { ... });

// 3. Promise — the modern default:
import { readFile } from 'node:fs/promises';
const data = await readFile('config.json', 'utf8');
```

**Common operations:**
```js
import { readFile, writeFile, mkdir, readdir, stat, rm, copyFile } from 'node:fs/promises';

await writeFile('out.txt', 'Hello', 'utf8');         // write (creates/overwrites)
await mkdir('build', { recursive: true });           // make dirs
const entries = await readdir('src');                // list files
const info = await stat('big.txt');                  // size, mtime, isFile()
await rm('old.txt', { force: true });                // delete
await copyFile('a.txt', 'b.txt');                    // copy
```

## Buffers — raw bytes

```js
import { Buffer } from 'node:buffer';

const buf = Buffer.from('hello');        // UTF-8 bytes by default
const base64 = buf.toString('base64');   // aGVsbG8=
const back = Buffer.from(base64, 'base64');

// Buffers are fixed-size byte arrays — the raw material of files, sockets, images.
```

**The trap:** `readFile` without an encoding returns a `Buffer` (binary). Specify `'utf8'` for text, or you'll get bytes that print as garbage.

## Streams — process data as it flows

```js
import { createReadStream, createWriteStream } from 'node:fs';

// Read a huge file line-by-line, processing as it arrives:
const read = createReadStream('huge.log', { encoding: 'utf8' });

read.on('data', (chunk) => {        // chunk arrives whenever it's ready
  processLine(chunk);
});
read.on('end', () => console.log('done'));
read.on('error', (err) => console.error(err));
```

**Stream types:**
- **Readable** — source of data (`createReadStream`, HTTP request, `process.stdin`)
- **Writable** — sink (`createWriteStream`, HTTP response, `process.stdout`)
- **Duplex** — both (TCP sockets)
- **Transform** — readable + writable that *modifies* data (zlib gzip, crypto cipher)

## Piping & backpressure

```js
// Copy a file with a stream — memory usage stays tiny:
import { createReadStream, createWriteStream } from 'node:fs';
import { pipeline } from 'node:stream/promises';

await pipeline(
  createReadStream('big.bin'),   // source
  createWriteStream('copy.bin')  // sink
);

// pipeline handles backpressure + error propagation automatically.
```

**Backpressure explained:** if the sink (disk, network) is slower than the source, `pipe`/`pipeline` pauses the source until the sink catches up. Data isn't dropped; the flow slows. Doing it manually:

```js
const read = createReadStream('big.bin');
const write = createWriteStream('copy.bin');

read.on('data', (chunk) => {
  const ok = write.write(chunk);
  if (!ok) read.pause();              // sink is full — stop reading
});
write.on('drain', () => read.resume());  // sink ready — resume
```

**Never use streams without handling backpressure** — naive `data`-handler code can balloon memory if the producer outruns the consumer. `pipeline` gets this right for you.

## HTTP body streaming

```js
// Download a file without buffering it all in memory:
import { pipeline } from 'node:stream/promises';
import { createWriteStream } from 'node:fs';

const res = await fetch('https://example.com/large.zip');
await pipeline(res.body, createWriteStream('large.zip'));
```

**Why it matters:** the response body is a readable stream. Piping it to disk uses constant memory regardless of file size — critical for downloads/uploads and file proxies.

## Reading line-by-line

```js
import { createReadStream } from 'node:fs';
import { createInterface } from 'node:readline';

const rl = createInterface({ input: createReadStream('huge.log') });

for await (const line of rl) {
  if (line.includes('ERROR')) console.log(line);
}
```

`readline` + async iteration is the clean way to process large text files without loading them whole.

---

**Setup:** Read a JSON config, mutate, write back.

**Solution:**
```js
import { readFile, writeFile } from 'node:fs/promises';

async function bumpVersion(path) {
  const config = JSON.parse(await readFile(path, 'utf8'));
  config.version = (config.version || 0) + 1;
  await writeFile(path, JSON.stringify(config, null, 2) + '\n');
}
```

**Key insight:** read text as `'utf8'`, parse, modify, serialize with pretty-print, write. Tiny and predictable. For config files this is the right tool — streams would be overkill.

---

**Setup:** Stream a server log through a filter to a report file, with sane memory use.

**Solution:**
```js
import { createReadStream, createWriteStream } from 'node:fs';
import { createInterface } from 'node:readline';
import { pipeline } from 'node:stream/promises';

const read = createReadStream('server.log');
const rl = createInterface({ input: read });
const out = createWriteStream('errors.txt');

for await (const line of rl) {
  if (line.includes('ERROR') || line.includes('FATAL')) {
    if (!out.write(line + '\n')) {        // backpressure: wait for drain
      await new Promise(r => out.once('drain', r));
    }
  }
}
out.end();
```

**Key insight:** one line in memory at a time, errors written as they're found. The `drain` wait is manual backpressure — the report file never grows faster than the disk can absorb.

---

**Setup:** Why does `readFile` on a 3 GB file crash the process, while a stream doesn't?

**Solution:** `readFile` reads the *entire* file into one Buffer before the callback runs — 3 GB needs 3 GB of heap, and Node's default heap limit (~2–4 GB) gets exhausted. A stream reads a few KB at a time, so peak memory stays tiny no matter the file size.

**Key insight:** "Read whole file" is a convenience for *small* files. For anything big, or when you can process incrementally, streams are the memory-safe answer — which is also why HTTP responses and stdin are streams by nature.

---

## Practice (try before peeking)

1. When is `readFile` (whole file) right, and when must you stream?
2. What is backpressure, and what happens without it?
3. Why does `pipeline` beat manual `.on('data')`?

<details><summary>Answers</summary>

1. `readFile` is right for small configs/JSON/docs you genuinely need entirely. Stream when the file is large, memory is constrained, or you can process incrementally (logs, CSV, uploads) — it keeps memory constant.
2. Backpressure is flow control: when the consumer is slower than the producer, the stream pauses the source so memory doesn't balloon. Without it, fast producers keep pushing data the consumer can't absorb — memory grows until the process dies or data is lost.
3. `pipeline` handles backpressure automatically, forwards errors from any stage, and cleans up streams on failure — the manual `data`/`drain` dance is exactly the bug-prone code it replaces.

</details>

---

**Common traps:**
- `readFile` on huge files → heap exhaustion; stream instead
- Forgetting the encoding → Buffer garbage instead of text
- Manual stream wiring without backpressure → memory spikes
- Not handling `error` events on streams — they crash silently otherwise
- `out.write()` returning `false` and ignoring it (dropping data or ballooning memory)

---
