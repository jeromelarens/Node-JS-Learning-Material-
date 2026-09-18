<div align="center">

# 🚀 Day 5 — Node.js Streams & Buffers

![Node.js](https://img.shields.io/badge/Node.js-20.x-339933?style=for-the-badge&logo=node.js&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Series](https://img.shields.io/badge/20%20Days-20%20Concepts-blueviolet?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)

**A 20-Day Node.js & Express.js Learning Series**

### 🌊 Learn how Node.js handles large amounts of data efficiently

**Buffer → Stream → Chunk → Process → Output**

**Learn → Understand → Code → Debug → Build**

</div>

---

## 👋 Welcome to Day 5

So far in this series:

| Day | Focus | Key Concepts |
|---|---|---|
| **Day 1** | Runtime & Event Loop | `Node.js Runtime` `Event Loop` `Call Stack` `libuv` `Non-blocking I/O` |
| **Day 2** | Modules | `CommonJS` `ES Modules` `Import/Export` `Module Architecture` |
| **Day 3** | File System | `fs` `fs/promises` `Read/Write/Append` `Directories` `Paths` `Metadata` |
| **Day 4** | EventEmitter | `Events` `emit()` `on()` `once()` `Listeners` `Event-driven architecture` |

Now we're going deeper.

# 🌊 Streams & Buffers

```text
5 KB file       →  No big deal.
500 MB video    →  Would you load it ALL into memory first?
5 GB backup     →  Usually, no.
20 GB db dump   →  Instead, process piece by piece.
```

That is where **Streams** become extremely useful.

---

## 📚 Table of Contents

<details>
<summary><strong>Click to expand full index (39 sections)</strong></summary>

1. [What is a Buffer?](#1--what-is-a-buffer)
2. [Why Do We Need Buffers?](#2--why-do-we-need-buffers)
3. [Creating Buffers](#3--creating-buffers)
4. [Buffer from String](#4--buffer-from-string)
5. [Buffer Size](#5--buffer-size)
6. [Buffer to String](#6--buffer-to-string)
7. [Binary Data](#7--binary-data)
8. [What is a Stream?](#8--what-is-a-stream)
9. [Why Do We Need Streams?](#9--why-do-we-need-streams)
10. [Chunk-Based Processing](#10--chunk-based-processing)
11. [Readable Streams](#11--readable-streams)
12. [Writable Streams](#12--writable-streams)
13. [Duplex Streams](#13--duplex-streams)
14. [Transform Streams](#14--transform-streams)
15. [Stream Events](#15--stream-events)
16. [Reading Files with Streams](#16--reading-files-with-streams)
17. [Writing Files with Streams](#17--writing-files-with-streams)
18. [pipe()](#18--pipe)
19. [pipeline()](#19--pipeline)
20. [Backpressure](#20--backpressure)
21. [highWaterMark](#21--highwatermark)
22. [Readable Stream Modes](#22--readable-stream-modes)
23. [Object Mode](#23--object-mode)
24. [HTTP and Streams](#24--http-and-streams)
25. [File Uploads](#25--file-uploads)
26. [File Downloads](#26--file-downloads)
27. [Compression with Streams](#27--compression-with-streams)
28. [Transform Stream Example](#28--transform-stream-example)
29. [Streams vs readFile](#29--streams-vs-readfile)
30. [Memory Comparison](#30--memory-comparison)
31. [Real Backend Architecture](#31--real-backend-architecture)
32. [Common Mistakes](#32--common-mistakes)
33. [Production Considerations](#33--production-considerations)
34. [Practical Project](#34--practical-project)
35. [Interview Questions](#35--interview-questions)
36. [Practice Tasks](#36--practice-tasks)
37. [Day 5 Challenge](#37--day-5-challenge--build-a-streaming-file-processor)
38. [Key Takeaways](#-key-takeaways)
39. [Day 6 Preview](#-day-6-preview)

</details>

---

## 1. 🧱 What is a Buffer?

A **Buffer** is a Node.js object used to represent a sequence of bytes.

> A Buffer is a way for Node.js to work with **raw binary data**.

```text
Images · Videos · PDFs · Audio · ZIP files · Network packets · Encrypted data · File chunks
```

---

## 2. 🤔 Why Do We Need Buffers?

JavaScript traditionally works heavily with strings and objects, but computers store and transfer data as bytes.

```text
File → Bytes → Buffer → Application
```

---

## 3. 🛠️ Creating Buffers

```js
const buffer = Buffer.from("Hello Node.js");

console.log(buffer);
// <Buffer 48 65 6c 6c 6f 20 4e 6f 64 65 2e 6a 73>
```

> Those hexadecimal values represent bytes.

---

## 4. 🔤 Buffer from String

```js
const buffer = Buffer.from("Hello");
console.log(buffer);

const text = buffer.toString();
console.log(text);
// Hello
```

```text
"Hello" → Buffer.from() → Bytes → buffer → toString() → "Hello"
```

---

## 5. 📏 Buffer Size

```js
const buffer = Buffer.from("Hello");

console.log(buffer.length);
// 5
```

> ⚠️ `buffer.length` represents **bytes**, not necessarily characters — important for Unicode:

```js
const buffer = Buffer.from("வணக்கம்");

console.log(buffer.length);
// byte count can be greater than the number of visible characters
```

---

## 6. 🔄 Buffer to String

```js
const buffer = Buffer.from("Node.js Backend");

console.log(buffer.toString("utf8"));
// Node.js Backend
```

> UTF-8 is a common encoding used for text.

---

## 7. 🖼️ Binary Data

Buffers are especially useful for binary data like `image.jpg`.

```js
import fs from "node:fs/promises";

const image = await fs.readFile("profile.jpg");

console.log(Buffer.isBuffer(image));
// true
```

> When no encoding is supplied, `readFile()` returns a Buffer.

---

## 8. 🌊 What is a Stream?

A **Stream** is an abstraction for handling data **incrementally over time**.

```text
Instead of:            A stream allows:
Entire Data             Chunk 1 → Process
    ↓                   Chunk 2 → Process
  Memory                Chunk 3 → Process
    ↓                   Chunk 4 → Process
 Process                ...
```

---

## 9. 🤔 Why Do We Need Streams?

```text
video.mp4 = 5 GB
```

```js
const data = await fs.readFile("video.mp4");
```

> ⚠️ For large data, this can create significant memory pressure.

With a stream, only manageable portions flow through the pipeline:

```text
5 GB File
    ├── Chunk 1
    ├── Chunk 2
    ├── Chunk 3
    ├── Chunk 4
    └── ...
```

---

## 10. 🧩 Chunk-Based Processing

```text
Large File
     ↓
┌───────────┐
│ Chunk 1   │
└───────────┘
     ↓
┌───────────┐
│ Chunk 2   │
└───────────┘
     ↓
┌───────────┐
│ Chunk 3   │
└───────────┘
     ↓
    ...
```

> A chunk is commonly represented as a **Buffer** for byte-oriented streams.

---

## 11. 📖 Readable Streams

Produces data. Examples: `File Read Stream` · `HTTP Request` · `Readable data source`

```js
import fs from "node:fs";

const stream = fs.createReadStream("large-file.txt");

stream.on("data", (chunk) => {
    console.log("Received chunk:", chunk.length);
});

stream.on("end", () => {
    console.log("Finished reading");
});
```

---

## 12. ✍️ Writable Streams

Consumes data. Examples: `File Write Stream` · `HTTP Response` · `Output stream`

```js
import fs from "node:fs";

const stream = fs.createWriteStream("output.txt");

stream.write("Hello Node.js\n");
stream.write("Learning Streams\n");
stream.end();
```

---

## 13. 🔄 Duplex Streams

Can both **read** and **write**.

```text
Input → Duplex Stream → Output
```

A common real-world example is a **network socket**:

```text
Client ↕ Socket ↕ Server
```

---

## 14. 🔧 Transform Streams

A special Duplex Stream where the output is **derived** from the input.

```text
Input → Transform → Modified Output
```

`Compression` · `Decompression` · `Encryption` · `Decryption` · `Data conversion` · `Parsing`

```text
Text → Uppercase Transform → UPPERCASE TEXT
```

---

## 15. 🎯 Stream Events

| Event | Meaning |
|---|---|
| `data` | A chunk is available |
| `end` | Readable stream has no more data |
| `error` | Something went wrong |
| `finish` | Writable stream finished processing writes |
| `close` | Stream closed |
| `drain` | Writable buffer is ready to accept more data |

```js
stream.on("data", (chunk) => console.log(chunk));
stream.on("end", () => console.log("Reading completed"));
stream.on("error", (error) => console.error(error));
writeStream.on("finish", () => console.log("Writing completed"));
```

---

## 16. 📖 Reading Files with Streams

Instead of:

```js
const data = await fs.readFile("large-file.txt", "utf8");
```

use:

```js
import fs from "node:fs";

const stream = fs.createReadStream("large-file.txt", { encoding: "utf8" });

stream.on("data", (chunk) => console.log(chunk));
stream.on("end", () => console.log("File reading completed"));
```

---

## 17. ✍️ Writing Files with Streams

```js
import fs from "node:fs";

const writeStream = fs.createWriteStream("output.txt");

writeStream.write("First line\n");
writeStream.write("Second line\n");
writeStream.end();

writeStream.on("finish", () => {
    console.log("File written successfully");
});
```

---

## 18. 🔗 pipe()

Connects a readable stream to a writable stream.

```js
import fs from "node:fs";

const readable = fs.createReadStream("input.txt");
const writable = fs.createWriteStream("output.txt");

readable.pipe(writable);
```

```text
input.txt → Readable Stream → pipe() → Writable Stream → output.txt
```

> ✨ Much cleaner than manually reading chunks and writing them yourself.

---

## 19. 🛡️ pipeline()

For production-grade stream composition:

```js
import fs from "node:fs";
import { pipeline } from "node:stream/promises";

const source = fs.createReadStream("input.txt");
const destination = fs.createWriteStream("output.txt");

await pipeline(source, destination);

console.log("Pipeline completed");
```

> ✅ Manages **streams, errors, completion, and cleanup** across a chain of streams.

---

## 20. 🚦 Backpressure

> 🚨 One of the **most important stream concepts**.

```text
Readable → Produces data VERY FAST
Writable → Consumes data SLOWLY
```

If the readable outpaces the writable, data can pile up in memory. This is **Backpressure**.

**Without flow control**
```text
Producer
  ↓↓↓↓↓↓↓↓↓
Data Queue
  ↓↓↓↓↓↓↓↓↓
Consumer (slow)
```

**With backpressure**
```text
Producer → Stream Buffer → Consumer → Flow controlled
```

---

## 21. 📊 highWaterMark

An internal buffering threshold:

```js
const stream = fs.createReadStream("large-file.txt", {
    highWaterMark: 64 * 1024
});
```

> ⚠️ **Not a strict maximum memory limit** — it's a buffering threshold the stream implementation uses to manage flow.

---

## 22. 🔀 Readable Stream Modes

```text
Paused mode · Flowing mode
```

Attaching a `'data'` listener can push the stream into **flowing mode**:

```js
stream.on("data", (chunk) => console.log(chunk));
```

```text
Readable Stream → Data becomes available → Application consumes chunks
```

---

## 23. 🧩 Object Mode

Normally streams work with `Buffer`/`String`. In **object mode**, they can work with JS objects.

```js
import { Readable } from "node:stream";

const stream = Readable.from(
    [{ id: 1, name: "Jerome" }, { id: 2, name: "Alex" }],
    { objectMode: true }
);

stream.on("data", (user) => console.log(user));
// { id: 1, name: 'Jerome' }
// { id: 2, name: 'Alex' }
```

---

## 24. 🌐 HTTP and Streams

HTTP request/response are **stream-based interfaces** in Node.js.

```text
Client → Request Data → Readable Request → Node.js Server
       → Writable Response → Client
```

> For large responses, stream data instead of constructing the whole thing in memory.

---

## 25. 📤 File Uploads

```text
Client → HTTP Request → Incoming Data → Stream → File Storage
```

> Streaming avoids loading the entire file into memory. Exact upload architecture depends on your framework/storage.

---

## 26. 📥 File Downloads

```text
report.pdf → Read Stream → HTTP Response → Client
```

```js
import http from "node:http";
import fs from "node:fs";

const server = http.createServer((req, res) => {
    if (req.url === "/download") {
        res.writeHead(200, { "Content-Type": "application/octet-stream" });

        const fileStream = fs.createReadStream("report.pdf");
        fileStream.pipe(res);
    }
});

server.listen(3000);
```

---

## 27. 🗜️ Compression with Streams

```text
Large File → Read Stream → Gzip Transform → Write Stream
```

```js
import fs from "node:fs";
import zlib from "node:zlib";

const input = fs.createReadStream("large-file.txt");
const output = fs.createWriteStream("large-file.txt.gz");

const gzip = zlib.createGzip();

input.pipe(gzip).pipe(output);
```

---

## 28. 🔧 Transform Stream Example

```js
import { Transform } from "node:stream";

const upperCase = new Transform({
    transform(chunk, encoding, callback) {
        const text = chunk.toString();
        callback(null, text.toUpperCase());
    }
});

process.stdin.pipe(upperCase).pipe(process.stdout);
```

```text
Keyboard Input → stdin → Transform → Uppercase → stdout
```

Type `hello node` → get `HELLO NODE`.

---

## 29. ⚖️ Streams vs readFile()

| Feature | `readFile()` | Stream |
|---|---|---|
| Loads whole result | Usually yes | No |
| Good for small files | ✅ | ✅ |
| Good for huge files | Can be memory-heavy | ✅ |
| Chunk processing | ❌ | ✅ |
| Backpressure support | ❌ | ✅ |
| Pipeline composition | Limited | ✅ |
| Complexity | Low | Higher |

---

## 30. 🧠 Memory Comparison

```text
readFile()

2 GB File
████████████████████
        ↓
    Memory
████████████████████


Stream

2 GB File
████████████████████
        ↓
  Chunk → Process
        ↓
  Chunk → Process
        ↓
  Chunk → Process
```

> That's why streams matter for large data.

---

## 31. 🏗️ Real Backend Architecture

**Upload**
```text
Client → Upload File → HTTP Request Stream → Validation
       → Transform/Processing → Storage
```

**Download**
```text
Storage → Read Stream → HTTP Response → Client
```

**Compressed download**
```text
Storage → Read Stream → Gzip Transform → HTTP Response → Client
```

---

## 32. ❌ Common Mistakes

| # | Mistake | Fix |
|---|---|---|
| 1 | `readFile()` on huge files | Use streams instead |
| 2 | Ignoring errors in `.pipe()` chains | Use `pipeline()` for structured error handling |
| 3 | Confusing Buffer and Stream | Buffer = bytes/data; Stream = data flow over time |
| 4 | Assuming streams automatically make things faster | Choose them for the data-flow problem, not blindly |
| 5 | Ignoring backpressure | Design proper flow control |
| 6 | Manually reinventing stream plumbing | Use `pipeline()` when appropriate |
| 7 | Treating `highWaterMark` as a hard memory limit | It's just a buffering threshold |

---

## 33. 🏭 Production Considerations

1. **Use streams for large data:** files, videos, backups, exports, downloads, uploads, compression, encryption.
2. **Use `pipeline()` for complex chains** instead of manually wiring `Readable → Transform → Transform → Writable`.
3. **Handle errors** at every stage: source, transform, destination, connection closure.
4. **Don't buffer unbounded data** — consider memory, backpressure, max file size, timeouts, cleanup.
5. **Validate uploaded files** — don't blindly trust filename, extension, content type, size.
6. **Use dedicated object storage** for large files in production — Amazon S3, Cloudflare R2, Google Cloud Storage, Azure Blob Storage — rather than keeping them in app memory.

---

## 34. 🛠️ Practical Project 1 — Large File Copy Utility

```text
day-5-stream-copy/
│
├── package.json
├── app.js
├── input/
│   └── large-file.txt
└── output/
    └── copied-file.txt
```

```js
import fs from "node:fs";
import path from "node:path";
import { pipeline } from "node:stream/promises";

const source = path.resolve("input", "large-file.txt");
const destination = path.resolve("output", "copied-file.txt");

const readable = fs.createReadStream(source);
const writable = fs.createWriteStream(destination);

try {
    await pipeline(readable, writable);
    console.log("File copied successfully");
} catch (error) {
    console.error("Copy failed:", error.message);
}
```

### 🔥 Practical Project 2 — Gzip Compressor

```js
import fs from "node:fs";
import zlib from "node:zlib";
import { pipeline } from "node:stream/promises";

await pipeline(
    fs.createReadStream("input.txt"),
    zlib.createGzip(),
    fs.createWriteStream("input.txt.gz")
);

console.log("Compression completed");
```

```text
input.txt → Read Stream → Gzip Transform → Write Stream → input.txt.gz
```

---

## 35. 🎤 Interview Questions

<details>
<summary><strong>🟢 Beginner (1–5)</strong></summary>

1. **What is a Buffer?**
   A Node.js object representing a sequence of bytes, used to work with raw binary data.

2. **What is a Stream?**
   An abstraction for handling data incrementally over time instead of loading it all before processing.

3. **Why are streams useful?**
   They process large/continuous data efficiently and control memory usage through incremental processing and backpressure.

4. **What is a chunk?**
   A piece of data processed by a stream — commonly a Buffer for byte-oriented streams.

5. **What is `pipe()`?**
   Connects a readable stream to a writable stream: `readable.pipe(writable);`

</details>

<details>
<summary><strong>🟡 Intermediate (6–10)</strong></summary>

6. **What are the four major stream types?**
   `Readable` · `Writable` · `Duplex` · `Transform`

7. **What is a Readable Stream?**
   A stream from which data can be consumed, e.g. `fs.createReadStream()`.

8. **What is a Writable Stream?**
   A stream to which data can be written, e.g. `fs.createWriteStream()`.

9. **What is a Duplex Stream?**
   A stream that supports both reading and writing.

10. **What is a Transform Stream?**
    A Duplex Stream where the output can be transformed from the input.

</details>

<details>
<summary><strong>🔴 Advanced (11–18)</strong></summary>

11. **What is backpressure?**
    A flow-control mechanism used when a producer generates data faster than a consumer can process it.

12. **What is `highWaterMark`?**
    A threshold controlling internal buffering behavior — not a strict maximum for total application memory.

13. **Difference between Buffer and Stream?**
    Buffer represents bytes/data; Stream handles data flow over time.

14. **Why use streams instead of `readFile()` for huge files?**
    Streams allow incremental processing, avoiding holding the whole file in memory at once.

15. **What is `pipeline()`?**
    Connects multiple streams with structured handling of completion, errors, and cleanup.

16. **Are HTTP requests streams in Node.js?**
    Yes — Node's HTTP request and response objects are stream-based interfaces.

17. **What is object mode?**
    Lets streams work with JS values/objects rather than only strings and Buffers.

18. **Does using streams guarantee lower memory usage?**
    Not automatically — a poorly designed pipeline can still accumulate data and create memory pressure.

</details>

---

## 36. 🧪 Practice Tasks

- [ ] **Task 1 — Buffer:** Create a Buffer from `"Hello Node.js"`. Print the Buffer, byte length, and original string.
- [ ] **Task 2 — Buffer Encoding:** Create a Buffer from `"Hello 🚀"`. Compare byte length vs visible character count.
- [ ] **Task 3 — File Read Stream:** Read `large-file.txt` with `fs.createReadStream()`, print each chunk size.
- [ ] **Task 4 — File Write Stream:** Create `output.txt` and write 100 lines using a Writable Stream.
- [ ] **Task 5 — File Copy:** Copy `input.txt` → `backup.txt` using `Readable → pipe() → Writable`.
- [ ] **Task 6 — File Metadata:** Compare source size vs destination size before/after copying.
- [ ] **Task 7 — Transform Stream:** Convert `hello / node / backend` → `HELLO / NODE / BACKEND`.
- [ ] **Task 8 — Gzip:** Compress `large-file.txt` → `large-file.txt.gz` via `Read Stream → Gzip Transform → Write Stream`.

---

## 37. 🔥 Day 5 Challenge — Build a Streaming File Processor

```text
stream-file-processor/
│
├── package.json
├── app.js
├── input/
│   └── large.txt
├── output/
│   ├── processed.txt
│   └── compressed.gz
└── services/
    └── fileProcessor.js
```

**Requirements**

| # | Requirement | Detail |
|---|---|---|
| 1 | Read File as Stream | `large.txt → Readable Stream` |
| 2 | Transform Content | `hello node.js` → `HELLO NODE.JS` |
| 3 | Save Processed Output | `processed.txt` |
| 4 | Compress Output | `processed.txt → Gzip → compressed.gz` |
| 5 | Use `pipeline()` | Chain everything cleanly |

**Architecture**
```text
large.txt → Read Stream → Transform Stream → Write Stream → processed.txt
processed.txt → Read Stream → Gzip Transform → Write Stream → compressed.gz
```

**Bonus:** Track `Total chunks processed`, `Total bytes processed`, `Processing time`:

```text
--------------------------------
STREAM PROCESSOR
--------------------------------
File: large.txt
Chunks: 142
Bytes: 9,432,112
Processing Time: 248 ms

Status: SUCCESS
--------------------------------
```

---

## 🧠 Mental Model

```text
                 STREAM
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
    Readable     Writable    Transform
        ↓           ↓           ↓
     Produces    Consumes    Modifies
      Data        Data        Data
```

**Common pipeline**
```text
SOURCE → Readable → Transform → Transform → Writable → DESTINATION
```

---

## 🔥 Buffer vs Stream

```text
┌──────────────────────────────┐
│            BUFFER            │
│  Represents bytes/data       │
│  Example: image bytes        │
└──────────────────────────────┘

┌──────────────────────────────┐
│            STREAM             │
│  Handles data over time       │
│  Example: large file reading  │
└──────────────────────────────┘
```

> A stream can deliver data in chunks represented by Buffers.

---

## ⚡ readFile vs Stream

```text
Small File → readFile() → Simple

Large File → Stream → Chunk by Chunk → Lower Memory Pressure
```

---

## 🎯 Key Takeaways

✅ Buffers — creation, encoding, byte length, binary data
✅ Streams — Readable, Writable, Duplex, Transform
✅ Stream events — `data` `end` `error` `finish`
✅ `pipe()` & `pipeline()`
✅ Backpressure & `highWaterMark`
✅ Object mode · HTTP streams · uploads · downloads · compression
✅ `readFile()` vs streams · Buffer vs Stream
✅ Production considerations & memory-efficient processing

---

## 💡 The Most Important Lesson

Don't memorize `createReadStream()`, `createWriteStream()`, `pipe()`, `pipeline()`, `Buffer.from()` in isolation:

```text
Large Data → Don't Load Everything → Read in Chunks
           → Process Chunks → Respect Backpressure
           → Write/Send Incrementally
```

> That's the real power of Node.js Streams.

---

## 🏭 Production Mental Model

```text
                LARGE FILE
                     ↓
              Readable Stream
                     ↓
               Validation
                     ↓
             Transform Stream
                     ↓
                Compression
                     ↓
              Writable Stream
                     ↓
                 Storage
```

```text
Storage → Read Stream → HTTP Response → Client
```

---

## 📌 Day 5 Checklist

- [ ] Understand Buffer — create, convert, byte length, binary data
- [ ] Understand Streams & chunks
- [ ] Understand Readable / Writable / Duplex / Transform Streams
- [ ] Understand stream events
- [ ] Use `createReadStream()` / `createWriteStream()`
- [ ] Use `pipe()` and `pipeline()`
- [ ] Understand backpressure & `highWaterMark`
- [ ] Understand object mode
- [ ] Understand HTTP streams, uploads & downloads
- [ ] Build a Transform Stream
- [ ] Build a Gzip pipeline
- [ ] Complete the Stream File Processor challenge
- [ ] Practice interview questions

---

## 🎤 Can You Explain This in an Interview?

> **"Why are streams important in Node.js?"**
> Streams let Node.js process data incrementally instead of loading everything into memory at once — great for large files, HTTP data, uploads, downloads, compression, and continuous data flows. They also provide backpressure so a fast producer doesn't overwhelm a slower consumer.

> **"Difference between Buffer and Stream?"**
> A Buffer represents a sequence of bytes; a Stream is an abstraction for handling data incrementally over time. Streams often deliver binary chunks as Buffers.

> **"What is backpressure?"**
> A flow-control mechanism used when a producer is faster than its consumer, letting the pipeline manage buffering instead of continuously accumulating data.

---

## 🚀 Day 6 Preview

### Callbacks, Promises & Async/Await

```text
Callbacks → Callback Hell → Promises → .then() .catch() .finally()
          → async/await → Promise.all() / allSettled() / race() / any()
          → Error Handling → Parallel vs Sequential Execution
          → Real Backend Async Patterns
```

We'll also understand **when asynchronous operations actually run** and how to avoid common async mistakes.

---

<div align="center">

# 🌊 Day 5 — Streams & Buffers

### **Process Data. Don't Just Load Data.**

**Learn → Understand → Code → Debug → Build → Share**

### 🚀 One concept closer to becoming a stronger backend developer.

---

*Part of the 20 Days • 20 Concepts — Node.js & Express.js Learning Series*

</div>
