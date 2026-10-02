<div align="center">

# 🚀 Day 1 — Node.js Runtime & Event Loop

### *"Before you build APIs, understand how Node.js thinks."*

![Node.js](https://img.shields.io/badge/Node.js-20.x-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner%20Friendly-22c55e?style=for-the-badge)
![Time](https://img.shields.io/badge/Time-3--4%20Hours-orange?style=for-the-badge)
![Tasks](https://img.shields.io/badge/Hands--On%20Tasks-12-blueviolet?style=for-the-badge)
![Series](https://img.shields.io/badge/20%20Days-20%20Concepts-ec4899?style=for-the-badge)

**A 20-Day Node.js & Express.js Learning Series**

</div>

---

## 🗺️ How to Use This Guide

This is **not a document to just read**. It is a **workbook**. Follow this loop for every section:

```text
 📖 Read  →  🧠 Predict  →  💻 Run  →  🔍 Compare  →  ✍️ Explain in your own words
```

| Icon | Meaning |
|---|---|
| 💡 | Key idea — remember this |
| 🧠 | Stop and think / predict before scrolling |
| 💻 | Type the code yourself (don't copy-paste!) |
| ⚠️ | Common trap / mistake |
| 🎯 | Task for you |
| ✅ | Solution (try first, then peek) |

> 🔥 **Golden rule:** If you can't predict the output of a code snippet *before* running it, you haven't understood it yet. That is completely normal on Day 1. Keep practicing.

---

## 📑 Table of Contents

- [🎒 Prerequisites & Setup](#-prerequisites--setup)
- [🎯 Learning Goals](#-learning-goals)
- [Part 1 — The Big Picture](#part-1--the-big-picture)
  - [1. What is Node.js?](#1--what-is-nodejs)
  - [2. Node.js ≠ Language](#2--nodejs-is-not-a-language)
  - [3. V8 Engine](#3--the-v8-engine)
  - [4. Node.js vs Browser](#4--nodejs-vs-browser)
- [Part 2 — How Code Runs](#part-2--how-code-runs)
  - [5. The Call Stack](#5--the-call-stack)
  - [6. Sync vs Async](#6--synchronous-vs-asynchronous)
  - [7. Blocking vs Non-Blocking](#7--blocking-vs-non-blocking)
- [Part 3 — The Event Loop](#part-3--the-event-loop)
  - [8. The Restaurant Analogy](#8--the-restaurant-analogy-)
  - [9. libuv & The Thread Pool](#9--libuv--the-thread-pool)
  - [10. Event Loop Phases](#10--event-loop-phases)
  - [11. Microtasks & `process.nextTick`](#11--microtasks--processnexttick)
  - [12. `setTimeout` vs `setImmediate`](#12--settimeout-vs-setimmediate)
- [Part 4 — Real World](#part-4--real-world)
  - [13. Is Node.js Single-Threaded?](#13--is-nodejs-really-single-threaded)
  - [14. CPU-Bound vs I/O-Bound](#14--cpu-bound-vs-io-bound)
  - [15. Worker Threads (Intro)](#15--worker-threads-intro)
  - [16. Your First Server](#16--your-first-server)
- [📝 Practice Zone](#-practice-zone)
- [🏗️ Mini Project](#️-mini-project-event-loop-lab-server)
- [🐞 Common Mistakes](#-common-mistakes--debugging-tips)
- [🧠 Interview Questions](#-interview-questions)
- [📋 Cheat Sheet](#-cheat-sheet)
- [📖 Glossary](#-glossary)
- [✅ Progress Tracker](#-progress-tracker)
- [📚 Further Reading](#-further-reading)

---

## 🎒 Prerequisites & Setup

**You should know:** basic JavaScript (variables, functions, arrays, objects, arrow functions). Callbacks and Promises will be explained here as we go.

### 💻 Setup (5 minutes)

```bash
# 1. Check Node.js is installed (use v20 or newer)
node -v

# 2. Check npm
npm -v

# 3. Create a workspace for the whole series
mkdir node-20-days && cd node-20-days
mkdir day-01 && cd day-01
```

Don't have Node? Download the **LTS** version from [nodejs.org](https://nodejs.org).

### 📂 Suggested Folder Structure

```text
node-20-days/
└── day-01/
    ├── 01-hello.js
    ├── 02-blocking.js
    ├── 03-event-loop-order.js
    ├── tasks/
    │   ├── task-01.js
    │   └── ...
    └── mini-project/
        ├── server.js
        └── worker.js
```

### ▶️ How to run any file

```bash
node filename.js
```

---

## 🎯 Learning Goals

By the end of Day 1, you will be able to:

- [ ] Explain what Node.js is **in your own words** (and what it isn't)
- [ ] Describe what **V8** and **libuv** each do
- [ ] Draw the **Call Stack** for a piece of code
- [ ] Explain **blocking vs non-blocking** with an example
- [ ] Describe the **Event Loop** using a real-life analogy
- [ ] Name the **6 phases** of the event loop
- [ ] Predict the output order of `sync`, `process.nextTick`, `Promise`, `setTimeout`, and `setImmediate`
- [ ] Explain why Node.js is *"single-threaded but not really"*
- [ ] Identify **CPU-bound** code that can freeze a server
- [ ] Build a small server that proves all of this with your own eyes

---

# Part 1 — The Big Picture

## 1. 🌱 What is Node.js?

**Node.js** is a **runtime environment** that lets you run JavaScript **outside the browser** — on your laptop, on a server, in the cloud.

```text
Before Node.js (2009)              After Node.js
─────────────────────              ─────────────────────
JavaScript → Browser only          JavaScript → Browser ✅
                                   JavaScript → Server  ✅
                                   JavaScript → CLI     ✅
                                   JavaScript → Desktop ✅ (Electron)
```

### 🤔 What does "runtime" mean?

A **runtime** is everything needed to *run* your code:

```text
┌──────────────────── Node.js Runtime ────────────────────┐
│                                                          │
│   ⚙️  V8 Engine      → understands & executes JS         │
│   🔄 libuv           → event loop + async I/O            │
│   📦 Node APIs       → fs, http, path, os, crypto ...    │
│   🔗 Bindings        → connect JS ↔ C/C++ internals      │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### 🏗️ What can you build?

| | | |
|---|---|---|
| 🌐 REST APIs | 💬 Chat apps | 🔐 Auth systems |
| 🛠️ CLI tools | 📺 Streaming apps | 🧩 Microservices |
| 💳 Payment backends | 🔌 WebSocket servers | 🤖 Bots & automation |

### 💻 Try it — your first Node program

Create `01-hello.js`:

```js
console.log("Hello from Node.js 👋");
console.log("Node version:", process.version);
console.log("Platform:", process.platform);
console.log("Current folder:", process.cwd());
```

Run: `node 01-hello.js`

> 🎯 **Check yourself:** `process` is not something you defined. Where did it come from? *(Answer: Node.js provides it as a global object. Browsers don't have it.)*

---

## 2. 🚫 Node.js Is Not a Language

This is the #1 beginner confusion.

| Term | What it is | Real-life comparison |
|---|---|---|
| **JavaScript** | The *language* you write | English |
| **V8** | The *engine* that executes JS | A translator who understands English |
| **Node.js** | The *runtime* that gives JS superpowers on a server | The whole office (translator + phone + files + internet) |

```text
Your Code (JavaScript)
        │
        ▼
   Node.js Runtime
        │
        ▼
     V8 Engine
        │
        ▼
 Machine Code (CPU runs it)
```

> 💡 When someone says *"I know Node.js"*, they usually mean: *"I know JavaScript and the Node.js runtime & its APIs."*

---

## 3. ⚙️ The V8 Engine

**V8** is Google's open-source JavaScript engine, written in C++. It's also what powers **Google Chrome**.

### What V8 does

| Job | Meaning |
|---|---|
| **Parse** | Reads your JS text and turns it into a structure it understands |
| **Compile** | Converts JS into fast machine code (JIT — Just-In-Time compilation) |
| **Execute** | Runs the code |
| **Memory management** | Allocates memory & cleans unused memory (**Garbage Collection**) |

### ⚠️ What V8 does NOT do

V8 knows pure JavaScript only. It has **no idea** about:

- ❌ Reading files (`fs`)
- ❌ Creating servers (`http`)
- ❌ Timers like `setTimeout` (that's provided by the runtime)
- ❌ Networking, databases, OS access

**Node.js adds all of that around V8.**

```js
// V8 understands this (pure JS):
const sum = [1, 2, 3].reduce((a, b) => a + b, 0);

// V8 alone can't do this. Node.js APIs make it possible:
const fs = require("fs");
fs.writeFileSync("hello.txt", "Hi!");
```

---

## 4. 🆚 Node.js vs Browser

Same language, **different environments**:

| Feature | 🌐 Browser | 🟢 Node.js |
|---|---|---|
| Global object | `window` | `global` / `globalThis` |
| DOM (`document`) | ✅ Yes | ❌ No |
| File system (`fs`) | ❌ No | ✅ Yes |
| Create HTTP server | ❌ No | ✅ Yes |
| Modules | ES Modules | CommonJS (`require`) + ES Modules |
| Purpose | Build UI | Build servers, tools, backends |

### 💻 Try it

```js
console.log(typeof window);    // "undefined" in Node
console.log(typeof document);  // "undefined" in Node
console.log(typeof process);   // "object"
console.log(typeof require);   // "function" (in CommonJS files)
```

---

# Part 2 — How Code Runs

## 5. 📚 The Call Stack

The **Call Stack** is how JavaScript keeps track of *"what function am I running right now?"*

> Think of a **stack of plates** 🍽️ — you add on top, and remove from top. **Last In, First Out (LIFO).**

### Example

```js
function first() {
  console.log("first: start");
  second();
  console.log("first: end");
}

function second() {
  console.log("second: running");
}

first();
```

### Step-by-step trace

```text
Step 1: Program starts          Step 2: first() called
┌──────────────┐                ┌──────────────┐
│    global    │                │   first()    │
└──────────────┘                ├──────────────┤
                                │    global    │
                                └──────────────┘

Step 3: second() called         Step 4: second() finishes → popped
┌──────────────┐                ┌──────────────┐
│   second()   │                │   first()    │
├──────────────┤                ├──────────────┤
│   first()    │                │    global    │
├──────────────┤                └──────────────┘
│    global    │
└──────────────┘                Step 5: first() finishes → popped
                                ┌──────────────┐
                                │    global    │
                                └──────────────┘
```

**Output**
```text
first: start
second: running
first: end
```

> 💡 **Rule:** *Push → Execute → Pop.* A function can only finish when everything it called has finished.

### ⚠️ What happens if the stack gets too deep? (Stack Overflow)

```js
function forever() {
  forever(); // calls itself endlessly
}
forever();
// RangeError: Maximum call stack size exceeded
```

### 🎯 Mini Exercise

Draw the call stack (like above) for this code. What's the **maximum** number of frames on the stack at once (including `global`)?

```js
function a() { b(); }
function b() { c(); }
function c() { console.log("C!"); }
a();
```

<details>
<summary>✅ Answer</summary>

Maximum = **4 frames**: `c()` on top, then `b()`, `a()`, and `global` at the bottom.
Output: `C!`

</details>

---

## 6. ⏱️ Synchronous vs Asynchronous

### 🔵 Synchronous = one thing at a time, in order

```js
console.log("A");
console.log("B");
console.log("C");
// A → B → C
```

Real life: standing in a queue at a bank. Nobody is served until the person ahead is done.

### 🟠 Asynchronous = start something, move on, handle the result later

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer finished");
}, 2000);

console.log("End");
```

**Output**
```text
Start
End
Timer finished     ← appears after ~2 seconds
```

Real life: you put clothes in a washing machine 🧺 and go cook. You don't stand there staring at it.

### 🧠 Predict before you scroll!

```js
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
```

<details>
<summary>✅ Answer</summary>

```text
1
3
2
```

Even with `0` ms, the timer callback **never** jumps ahead of the currently running synchronous code. It waits until the call stack is empty.

</details>

### 🔔 Three ways to handle async results

| Style | Example | Notes |
|---|---|---|
| **Callback** | `fs.readFile(path, cb)` | Oldest style, can get nested ("callback hell") |
| **Promise** | `fs.promises.readFile(path).then(...)` | Cleaner chaining |
| **async/await** | `await fs.promises.readFile(path)` | Looks synchronous, still async |

```js
const fs = require("fs");
const fsp = require("fs/promises");

// 1) Callback
fs.readFile("data.txt", "utf8", (err, data) => {
  if (err) return console.error(err);
  console.log("callback:", data);
});

// 2) Promise
fsp.readFile("data.txt", "utf8")
  .then((data) => console.log("promise:", data))
  .catch(console.error);

// 3) async/await
async function main() {
  try {
    const data = await fsp.readFile("data.txt", "utf8");
    console.log("await:", data);
  } catch (err) {
    console.error(err);
  }
}
main();
```

> 💡 All three are **non-blocking**. `await` only pauses *that function*, not the whole program.

---

## 7. 🧱 Blocking vs Non-Blocking

| | 🔴 Blocking | 🟢 Non-Blocking |
|---|---|---|
| Behavior | Everything waits | Work continues; result handled later |
| Naming in Node | Usually ends with `Sync` (`readFileSync`) | Callback / Promise based |
| Effect on server | One slow task delays **all** users | Other users keep getting served |

### 💻 Try it — blocking

Create `data.txt` with any text, then `02-blocking.js`:

```js
const fs = require("fs");

console.log("1. Start");

const data = fs.readFileSync("data.txt", "utf8"); // ⛔ waits here
console.log("2. File content:", data);

console.log("3. Done");
```

Output order: `1 → 2 → 3` (always).

### 💻 Try it — non-blocking

```js
const fs = require("fs");

console.log("1. Start");

fs.readFile("data.txt", "utf8", (err, data) => {
  console.log("2. File content:", data);
});

console.log("3. Done");
```

Output order: `1 → 3 → 2`.

### 🔥 Why it matters on a server

Imagine 100 users hit your server at the same time and each needs a file read of 100 ms.

```text
BLOCKING:      User1 ▓▓▓▓ → User2 ▓▓▓▓ → User3 ▓▓▓▓ → ... (last user waits ~10 seconds!)
NON-BLOCKING:  User1 ▓▓▓▓
               User2 ▓▓▓▓      ← all handled in an overlapping way
               User3 ▓▓▓▓
```

> ⚠️ **Rule of thumb:** On a server, avoid `*Sync` functions inside request handlers. It's fine to use them at **startup** (e.g., reading config once).

---

# Part 3 — The Event Loop

## 8. 🍽️ The Restaurant Analogy

Forget diagrams for a minute. Understand with a story.

```text
 👨‍🍳 KITCHEN (libuv / OS / thread pool)     🧑‍💼 WAITER (Main JS Thread)
 ──────────────────────────────────         ──────────────────────────
 Cooks dishes in the background             Takes orders, serves food
 Can cook many dishes at once               Only ONE waiter, but very fast
```

**What a smart waiter does:**

1. Takes **Table 1's** order → gives the slip to the kitchen → *doesn't wait there*.
2. Takes **Table 2's** order → gives to the kitchen.
3. Takes **Table 3's** order → gives to the kitchen.
4. Kitchen shouts: *"Table 1 is ready!"* → waiter serves it.
5. Repeats forever. 🔁

**A bad waiter (blocking):** takes Table 1's order, then **stands in the kitchen watching** until the dish is done. Tables 2 and 3 starve. 😡

### 🧩 Mapping the analogy to Node.js

| Restaurant | Node.js |
|---|---|
| Waiter | **Main JS thread** (runs your JavaScript) |
| Order slip | **Async request** (read a file, DB query, HTTP call) |
| Kitchen | **libuv + OS + thread pool** |
| "Table 1 ready!" bell | **Callback queued** when operation completes |
| Waiter checking for bells & new orders continuously | **The Event Loop** |
| Waiter stuck cooking a huge cake himself | **CPU-heavy code blocking the loop** 🚨 |

### 🔄 The Flow

```text
 Your JS code runs on the Call Stack
              │
              ▼
   Async operation encountered
   (fs.readFile, setTimeout, http request...)
              │
              ▼
   Handed to Node APIs → libuv → OS / thread pool
              │                    (JS keeps running meanwhile!)
              ▼
   Operation completes → callback is queued
              │
              ▼
   ♻️  EVENT LOOP: "Is the Call Stack empty?
        If yes, move the next callback onto it."
              │
              ▼
   Callback runs on the Call Stack
```

> 💡 **One-line definition:** The Event Loop is the mechanism that **takes finished async work's callbacks and runs them on the main thread when the call stack is free.**

---

## 9. 🧵 libuv & The Thread Pool

**libuv** is a C library that gives Node.js its async superpowers.

```text
                  Node.js
                     │
        ┌────────────┴────────────┐
        ▼                         ▼
       V8                       libuv
 (runs your JS)         (event loop + async I/O)
                                  │
                  ┌───────────────┴───────────────┐
                  ▼                               ▼
          OS async mechanisms                Thread Pool
     (epoll / kqueue / IOCP)            (default 4 threads)
      Network I/O (sockets, HTTP)       File system, DNS lookup,
                                        crypto, zlib compression
```

### 🔑 Two ways async work gets done

| Work type | Handled by |
|---|---|
| **Network** (HTTP, TCP sockets) | OS async mechanisms (very efficient, no extra thread per connection) |
| **fs, crypto (pbkdf2, scrypt), zlib, `dns.lookup`** | libuv **Thread Pool** (default size **4**) |

> 💡 You can change the pool size: `UV_THREADPOOL_SIZE=8 node app.js` (we'll do an experiment in the Practice Zone).

### 🧠 Mental model

> **Node.js = V8 + libuv + Node APIs + C++ bindings.** Not just V8!

---

## 10. 🌀 Event Loop Phases

The event loop runs in a **cycle**. Each trip around is called a **tick/iteration**, and has these phases:

```text
        ┌───────────────────────────┐
   ┌───►│  1. ⏰ Timers              │  setTimeout / setInterval callbacks
   │    └─────────────┬─────────────┘
   │    ┌─────────────▼─────────────┐
   │    │  2. ⏳ Pending Callbacks   │  some deferred system callbacks
   │    └─────────────┬─────────────┘
   │    ┌─────────────▼─────────────┐
   │    │  3. 🔧 Idle, Prepare       │  internal use only
   │    └─────────────┬─────────────┘
   │    ┌─────────────▼─────────────┐
   │    │  4. 📥 Poll                │  I/O callbacks; waits for new I/O
   │    └─────────────┬─────────────┘
   │    ┌─────────────▼─────────────┐
   │    │  5. ✅ Check               │  setImmediate callbacks
   │    └─────────────┬─────────────┘
   │    ┌─────────────▼─────────────┐
   │    │  6. 🚪 Close Callbacks     │  e.g. socket.on("close")
   │    └─────────────┬─────────────┘
   └──────────────────┘   (repeat until nothing is left to do)
```

### 📋 Phase summary

| # | Phase | What runs here | Example |
|---|---|---|---|
| 1 | **Timers** | Expired timer callbacks | `setTimeout`, `setInterval` |
| 2 | **Pending** | Certain deferred system-level callbacks | e.g. some TCP errors |
| 3 | **Idle/Prepare** | Internal use | — |
| 4 | **Poll** | Completed I/O callbacks; can wait for new I/O | `fs.readFile` callback, incoming request |
| 5 | **Check** | `setImmediate` callbacks | `setImmediate(fn)` |
| 6 | **Close** | Cleanup events | `socket.on("close")` |

> ⚠️ **Important:** `setTimeout(fn, 1000)` means *"run no sooner than ~1000 ms"*, **not** *"exactly at 1000 ms"*. If the main thread is busy, it runs later.

### 🧠 Between phases: the microtask queues

After **each** callback (and between phases), Node drains:

1. `process.nextTick` queue (highest priority)
2. Promise microtask queue (`.then`, `await`, `queueMicrotask`)

That's our next topic ⬇️

---

## 11. ⚡ Microtasks & `process.nextTick`

### Priority order (memorize!)

```text
  1️⃣  Synchronous code (current call stack)
  2️⃣  process.nextTick() callbacks
  3️⃣  Promise microtasks  (.then / await / queueMicrotask)
  4️⃣  Event-loop phases    (timers, I/O, setImmediate ...)
```

### 💻 Example 1

```js
console.log("Start");

Promise.resolve().then(() => console.log("Promise"));

console.log("End");
```

```text
Start
End
Promise
```

### 💻 Example 2 — the classic

```js
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

console.log("4");
```

```text
1
4
3
2
```

**Why?**

```text
Step 1: Sync code      → prints 1, schedules timer, schedules promise, prints 4
Step 2: Stack empty    → run microtasks → prints 3
Step 3: Event loop     → timers phase → prints 2
```

### 💻 Example 3 — with `process.nextTick`

```js
console.log("A");

setTimeout(() => console.log("B (timeout)"), 0);

Promise.resolve().then(() => console.log("C (promise)"));

process.nextTick(() => console.log("D (nextTick)"));

console.log("E");
```

```text
A
E
D (nextTick)
C (promise)
B (timeout)
```

> 💡 `nextTick` runs **before** Promise callbacks. Name tip: it's misleading — it runs *before* the next event-loop phase continues, not "on the next tick".

### ⚠️ Danger: starving the event loop

```js
function loop() {
  process.nextTick(loop); // schedules itself forever
}
loop();
setTimeout(() => console.log("I will never run 😢"), 0);
```

Microtask queues are drained **completely** before the loop moves on. An endless chain of them blocks everything else. (Don't run this unless you're ready to press `Ctrl + C`.)

---

## 12. ⏲️ `setTimeout` vs `setImmediate`

```js
setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
```

### From the main module → order is **not guaranteed**

It depends on how fast your process starts. Run it multiple times — you may see either order.

### Inside an I/O callback → `setImmediate` always wins

```js
const fs = require("fs");

fs.readFile(__filename, () => {
  setTimeout(() => console.log("timeout"), 0);
  setImmediate(() => console.log("immediate"));
});
```

```text
immediate
timeout
```

**Why?** We're currently in the **Poll** phase. The next phase is **Check** (`setImmediate`), and only *after* the loop wraps around do we reach **Timers**.

```text
 Poll (we are here) ──► Check (setImmediate ✅ runs) ──► Close ──► Timers (timeout runs)
```

> 🧠 **Lesson:** Don't memorize one fixed output. Ask: *"In which phase am I scheduling this?"*

---

# Part 4 — Real World

## 13. 🤔 Is Node.js Really Single-Threaded?

**Short answer: Your JavaScript runs on one main thread. But Node.js, as a whole, uses more threads.**

```text
             ┌──────────── Node.js Process ────────────┐
             │                                          │
             │   🧵 Main Thread  → runs YOUR JS code     │
             │                     + the event loop      │
             │                                          │
             │   🧵🧵🧵🧵 libuv Thread Pool (default 4)   │
             │        → fs, crypto, zlib, dns.lookup     │
             │                                          │
             │   🧵 V8 helper threads (GC, compile)      │
             │                                          │
             │   🧵 Worker Threads (only if YOU create)  │
             └──────────────────────────────────────────┘
```

| Statement | True? |
|---|---|
| "My JS code runs in parallel on many threads by default." | ❌ No |
| "Node.js process uses only 1 thread total." | ❌ No |
| "Your JS runs on 1 main thread by default; Node uses other threads internally." | ✅ Yes |

> 💡 **Benefit:** No race conditions between your JS functions by default, because only one piece of your JS runs at a time. **Cost:** a heavy computation stops everything else.

---

## 14. ⚔️ CPU-Bound vs I/O-Bound

| 🌐 I/O-Bound (waiting) | 🧮 CPU-Bound (calculating) |
|---|---|
| Database queries | Complex math loops |
| HTTP/API calls | Image/video processing |
| Reading files | Large JSON transformation |
| Network communication | Compression / heavy encryption |
| *Time is spent **waiting*** | *Time is spent **computing*** |

### ✅ Node.js is great at I/O-bound

While waiting for a DB response, the main thread serves other requests.

### 🚨 Node.js struggles with CPU-bound on the main thread

```js
// Blocks everything for several seconds
function heavyTask() {
  let sum = 0;
  for (let i = 0; i < 5_000_000_000; i++) {
    sum += i;
  }
  return sum;
}
```

```text
 CPU-heavy task running
        │
        ▼
 Main thread is BUSY (stack never empties)
        │
        ▼
 Event loop can't pick up callbacks
        │
        ▼
 ❌ Timers delayed, ❌ requests wait, ❌ server "freezes"
```

### 🛠️ Solutions

| Solution | When to use |
|---|---|
| **Worker Threads** | CPU-heavy JS inside the same app |
| **Child processes / cluster** | Use multiple CPU cores |
| **Job queues** (BullMQ, RabbitMQ...) | Background tasks like emails, reports |
| **Separate microservice** | Heavy processing as its own service |
| **Chunking** | Break the work into small pieces (`setImmediate`) |

---

## 15. 🧰 Worker Threads (Intro)

A **Worker Thread** is a separate JS thread with its own event loop and memory, used for CPU-heavy work.

### 💻 Minimal example

**`worker-demo.js`**

```js
const { Worker, isMainThread, parentPort, workerData } = require("worker_threads");

if (isMainThread) {
  // 🧵 Main thread
  console.log("Main: starting worker...");

  const worker = new Worker(__filename, { workerData: { n: 1_000_000_000 } });

  worker.on("message", (result) => console.log("Main: got result =", result));
  worker.on("error", (err) => console.error("Worker error:", err));
  worker.on("exit", (code) => console.log("Worker exited with code", code));

  // Main thread stays FREE while the worker calculates
  setInterval(() => console.log("Main: still responsive ✅"), 500).unref();
} else {
  // 🧵 Worker thread
  let sum = 0;
  for (let i = 0; i < workerData.n; i++) sum += i;
  parentPort.postMessage(sum);
}
```

> 💡 While the worker calculates, the main thread keeps printing *"still responsive"*. That's the whole point!

---

## 16. 🛠️ Your First Server

```js
// server.js
const http = require("http");

const server = http.createServer((req, res) => {
  console.log("Request received:", req.method, req.url);

  setTimeout(() => {
    res.end("Response from Node.js");
  }, 2000);
});

server.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

```bash
node server.js
```

Open `http://localhost:3000` in **three browser tabs quickly**. All three respond after ~2 seconds each — **not** 2 + 4 + 6 seconds — because the timer wait does not block the main thread.

### 🔍 What happened internally?

```text
Request arrives → Poll phase → your callback runs
   → setTimeout registered (Node/libuv tracks the timer)
   → callback finishes, stack empties
   → event loop free to accept another request
   → 2s later: Timers phase → res.end() runs
```

---

# 📝 Practice Zone

> 🎯 **Do these in order.** Try each one *before* opening the solution. Write your **prediction first** as a comment, then run it.

## 🟢 Level 1 — Warm-up (Predict the output)

### 🎯 Task 1 — Execution Order

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

<details>
<summary>✅ Solution</summary>

```text
A
D
C
B
```
Sync (`A`, `D`) → microtask (`C`) → timer (`B`).

</details>

---

### 🎯 Task 2 — Timers

```js
setTimeout(() => console.log("Timer 1"), 1000);
setTimeout(() => console.log("Timer 2"), 0);
setTimeout(() => console.log("Timer 3"), 500);
```

<details>
<summary>✅ Solution</summary>

```text
Timer 2
Timer 3
Timer 1
```
Timers run in order of **expiry time**, not the order they were written.

</details>

---

### 🎯 Task 3 — The Big Mix (hard!)

```js
console.log("1");

setTimeout(() => console.log("2"), 0);

setImmediate(() => console.log("3"));

Promise.resolve().then(() => console.log("4"));

process.nextTick(() => console.log("5"));

console.log("6");
```

<details>
<summary>✅ Solution</summary>

```text
1
6
5      ← nextTick
4      ← promise
2 / 3  ← these two can appear in either order from the main module
```
Reasoning: sync → nextTick → promise → event loop. `setTimeout 0` vs `setImmediate` from the main module is **not guaranteed**.

</details>

---

### 🎯 Task 4 — Draw the stack

Draw the call stack at the moment `console.log("Hi")` runs:

```js
function one()   { two(); }
function two()   { three(); }
function three() { console.log("Hi"); }
one();
```

<details>
<summary>✅ Solution</summary>

```text
┌────────────┐
│  three()   │   ← top (running)
│  two()     │
│  one()     │
│  global    │
└────────────┘
```

</details>

---

## 🟡 Level 2 — Hands-on Experiments

### 🎯 Task 5 — Blocking Experiment 🔥

Goal: *see* the event loop being blocked.

```js
// task-05.js
const start = Date.now();

setTimeout(() => {
  console.log(`Timer fired after ${Date.now() - start} ms (expected ~1000)`);
}, 1000);

// Block the main thread for 3 seconds
while (Date.now() - start < 3000) {
  // busy waiting...
}

console.log("Loop finished");
```

**Questions:**
1. After how many ms does the timer actually fire?
2. Why isn't it 1000 ms?
3. What does this tell you about `setTimeout` accuracy?

<details>
<summary>✅ Solution</summary>

1. Around **3000+ ms**.
2. The timer expired at 1000 ms, but the callback can only run when the call stack is empty. The `while` loop kept the stack busy for 3 seconds.
3. `setTimeout` guarantees a **minimum** delay, never an exact time.

</details>

---

### 🎯 Task 6 — Sync vs Async File Read

Create `big.txt` (e.g. copy-paste lots of text) and compare:

```js
const fs = require("fs");

console.time("sync");
fs.readFileSync("big.txt");
console.timeEnd("sync");

console.time("async");
fs.readFile("big.txt", () => {
  console.timeEnd("async");
});
console.log("Line after readFile — runs BEFORE async finishes");
```

**Observe:** Which log appears first? What does it tell you about the order of execution?

<details>
<summary>✅ Solution</summary>

`sync` timing appears first (the program waited). The line *"Line after readFile…"* prints **before** the `async` timing, proving the async read didn't block the code that follows.

</details>

---

### 🎯 Task 7 — Thread Pool Experiment 🧵

```js
// task-07.js
const crypto = require("crypto");

const start = Date.now();

for (let i = 1; i <= 6; i++) {
  crypto.pbkdf2("password", "salt", 500_000, 64, "sha512", () => {
    console.log(`Hash ${i} done in ${Date.now() - start} ms`);
  });
}
```

Run it normally, then with a bigger pool:

```bash
# Mac / Linux
UV_THREADPOOL_SIZE=8 node task-07.js

# Windows PowerShell
$env:UV_THREADPOOL_SIZE=8; node task-07.js
```

**Questions:**
1. With the default pool (4), how do the 6 hashes finish? All together or in groups?
2. What changes with a pool size of 8?

<details>
<summary>✅ Solution</summary>

1. With 4 threads, the first **4** finish at roughly the same time; hashes 5 and 6 wait for a free thread and finish later (in a second "batch").
2. With 8 threads (and enough CPU cores), all 6 run at the same time and finish around the same time. Exact timings depend on your machine's CPU.

</details>

---

### 🎯 Task 8 — `setTimeout` vs `setImmediate`

Run this **10 times** from the terminal:

```js
setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
```

Then wrap them inside an `fs.readFile` callback and run again.

**Questions:** Is the order consistent in both cases? Why / why not?

<details>
<summary>✅ Solution</summary>

- Main module: order **may vary** between runs (depends on process performance).
- Inside an I/O callback: `immediate` always prints first, because after the Poll phase comes the Check phase before looping back to Timers.

</details>

---

## 🔴 Level 3 — Challenge Tasks

### 🎯 Task 9 — Fix the Blocking Server

This server freezes for **everyone** when someone visits `/heavy`. **Run it, prove it freezes, then fix it with a Worker Thread.**

```js
const http = require("http");

const server = http.createServer((req, res) => {
  if (req.url === "/heavy") {
    let sum = 0;
    for (let i = 0; i < 3_000_000_000; i++) sum += i;
    return res.end("Heavy result: " + sum);
  }
  res.end("Hello! I am fast ⚡");
});

server.listen(3000, () => console.log("http://localhost:3000"));
```

**How to test the freeze:**
1. Open `/heavy` in one tab.
2. Immediately open `/` in another tab.
3. Notice `/` hangs until `/heavy` finishes.

<details>
<summary>✅ Solution (hint first, then full code)</summary>

**Hint:** Move the loop into a `Worker`, and return a Promise that resolves when the worker posts its result.

```js
// server.js
const http = require("http");
const { Worker } = require("worker_threads");

function runHeavyTask() {
  return new Promise((resolve, reject) => {
    const worker = new Worker("./worker.js");
    worker.on("message", resolve);
    worker.on("error", reject);
  });
}

const server = http.createServer(async (req, res) => {
  if (req.url === "/heavy") {
    try {
      const result = await runHeavyTask();
      return res.end("Heavy result: " + result);
    } catch (err) {
      res.statusCode = 500;
      return res.end("Worker failed");
    }
  }
  res.end("Hello! I am fast ⚡");
});

server.listen(3000, () => console.log("http://localhost:3000"));
```

```js
// worker.js
const { parentPort } = require("worker_threads");

let sum = 0;
for (let i = 0; i < 3_000_000_000; i++) sum += i;

parentPort.postMessage(sum);
```

Now `/` responds instantly even while `/heavy` is calculating. ✅

</details>

---

### 🎯 Task 10 — Build Your Own Logger of the Event Loop

Write a script that logs these 8 lines in the correct order **by only using** `console.log`, `setTimeout`, `setImmediate`, `process.nextTick`, and `Promise`:

```text
1. sync start
2. sync end
3. nextTick
4. promise
5. timeout
6. immediate
```

Which order is *guaranteed* and which is *not*? Explain.

<details>
<summary>✅ Solution</summary>

```js
console.log("sync start");

setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
Promise.resolve().then(() => console.log("promise"));
process.nextTick(() => console.log("nextTick"));

console.log("sync end");
```

Guaranteed: `sync start → sync end → nextTick → promise`.
Not guaranteed (main module): `timeout` vs `immediate` order.

To make `immediate` reliably come before `timeout`, schedule both from inside an I/O callback.

</details>

---

### 🎯 Task 11 — Async/Await Order Puzzle 🧩

```js
async function foo() {
  console.log("foo start");
  await bar();
  console.log("foo end");
}

async function bar() {
  console.log("bar");
}

console.log("script start");
foo();
console.log("script end");
```

<details>
<summary>✅ Solution</summary>

```text
script start
foo start
bar
script end
foo end
```
`foo()` runs synchronously until the first `await`. The code after `await` is scheduled as a **microtask**, so it runs after `script end`.

</details>

---

### 🎯 Task 12 — Explain It Like I'm 5 (Writing Task) ✍️

In **5–8 lines** of your own words, write an explanation of the Event Loop for someone who has never coded. Use **your own analogy** (not the restaurant!). Post it in your notes/blog/LinkedIn with `#20DaysNodeJS`.

> 💡 If you can explain it simply, you understand it deeply.

---

# 🏗️ Mini Project: Event Loop Lab Server

Build a small server that **proves** how Node.js behaves. This project ties all the concepts together.

### 📌 Requirements

| Route | Behavior | What it demonstrates |
|---|---|---|
| `GET /` | Returns `"Hello ⚡"` instantly | Normal fast request |
| `GET /delay` | Responds after 2 seconds using `setTimeout` | **Non-blocking** wait |
| `GET /read` | Reads a file with `fs.readFile` and returns its content | **Async I/O** |
| `GET /block` | Runs a 5-second busy loop | **Blocking** the event loop 🚨 |
| `GET /worker` | Runs the same heavy task in a Worker Thread | **Fixing** the block ✅ |
| `GET /time` | Returns the current time as JSON | Used to **test responsiveness** |

### 🧪 Experiments to run

1. Open `/delay` in 3 tabs at once → all finish in ~2 s.
2. Open `/block`, then immediately `/time` → `/time` **hangs** ❌
3. Open `/worker`, then immediately `/time` → `/time` **responds instantly** ✅
4. Write 4–5 lines in `NOTES.md` explaining what you saw.

### 🧱 Starter Code

```js
// mini-project/server.js
const http = require("http");
const fs = require("fs");
const { Worker } = require("worker_threads");

function sendJSON(res, data, status = 200) {
  res.writeHead(status, { "Content-Type": "application/json" });
  res.end(JSON.stringify(data));
}

function runInWorker(ms) {
  return new Promise((resolve, reject) => {
    const worker = new Worker("./worker.js", { workerData: { ms } });
    worker.on("message", resolve);
    worker.on("error", reject);
  });
}

const server = http.createServer(async (req, res) => {
  const { url } = req;

  if (url === "/") {
    return res.end("Hello ⚡");
  }

  if (url === "/time") {
    return sendJSON(res, { time: new Date().toISOString() });
  }

  if (url === "/delay") {
    return setTimeout(() => res.end("Delayed 2s response"), 2000);
  }

  if (url === "/read") {
    return fs.readFile(__filename, "utf8", (err, data) => {
      if (err) return sendJSON(res, { error: err.message }, 500);
      res.end(data.slice(0, 200));
    });
  }

  if (url === "/block") {
    const start = Date.now();
    while (Date.now() - start < 5000) {} // ⛔ blocks everyone
    return res.end("Blocked for 5 seconds 😵");
  }

  if (url === "/worker") {
    try {
      const msg = await runInWorker(5000);
      return res.end(msg);
    } catch (err) {
      return sendJSON(res, { error: err.message }, 500);
    }
  }

  sendJSON(res, { error: "Not found" }, 404);
});

server.listen(3000, () => console.log("🚀 Lab running on http://localhost:3000"));
```

```js
// mini-project/worker.js
const { parentPort, workerData } = require("worker_threads");

const start = Date.now();
while (Date.now() - start < workerData.ms) {} // busy loop inside the worker only

parentPort.postMessage(`Worker finished after ${workerData.ms} ms ✅`);
```

### 🌟 Bonus Challenges

- [ ] Add a `GET /tick` route that logs the order of `nextTick`, promise, timeout, and immediate to the server console
- [ ] Add a **request counter** and show it in `/time`
- [ ] Log how long each request took (use `console.time`)
- [ ] Use `autocannon` or `ab` to load-test `/delay` vs `/block` and compare results

---

# 🐞 Common Mistakes & Debugging Tips

| ❌ Mistake | ✅ Reality / Fix |
|---|---|
| "Node.js runs everything asynchronously." | Your JS runs **synchronously by default**. Only specific operations are async. |
| "`setTimeout(fn, 0)` runs immediately." | It runs **after** the current sync code and microtasks, in the Timers phase. |
| "`async` makes a function run in another thread." | No. `async` doesn't create threads. A CPU-heavy `async` function still blocks. |
| "`await` blocks the whole program." | It only pauses **that** async function. Other code continues. |
| Using `readFileSync` inside a request handler | Use `fs.promises.readFile` instead. |
| Writing a heavy `for` loop in an API route | Move it to a Worker Thread or background job. |
| Forgetting error handling in callbacks | Always check `err` first: `if (err) return ...` |
| "Node.js is always faster than other backends." | It depends on workload. Node shines at I/O-heavy apps. |

### 🕵️ Debugging cheat tips

```js
// 1. Measure how long something takes
console.time("task");
// ... code ...
console.timeEnd("task");

// 2. See the call stack at any point
console.trace("Where am I?");

// 3. Measure event loop delay (is something blocking?)
let last = Date.now();
setInterval(() => {
  const now = Date.now();
  const lag = now - last - 100;
  if (lag > 50) console.log(`⚠️ Event loop lag: ${lag} ms`);
  last = now;
}, 100);
```

---

# 🧠 Interview Questions

<details>
<summary><strong>🟢 Beginner</strong></summary>

**1. What is Node.js?**
A JavaScript runtime environment that lets JS run outside the browser.

**2. Which engine does Node.js use?**
Google's V8.

**3. Is Node.js a language or framework?**
Neither. It's a runtime. JavaScript is the language; Express is a framework built on top of Node.

**4. What is the Event Loop?**
The mechanism that picks completed async callbacks and runs them on the main thread when the call stack is free.

**5. What is the Call Stack?**
A LIFO structure tracking which functions are currently executing.

**6. Is Node.js single-threaded?**
Your JS runs on one main thread, but Node uses other threads internally (libuv pool, Worker Threads).

</details>

<details>
<summary><strong>🟡 Intermediate</strong></summary>

**7. What is non-blocking I/O?**
Starting an I/O operation without making the main thread wait for it to finish.

**8. What is libuv?**
A C library providing the event loop, async I/O, timers and thread pool for Node.js.

**9. Name the event loop phases.**
Timers → Pending Callbacks → Idle/Prepare → Poll → Check → Close Callbacks.

**10. `process.nextTick` vs `Promise.then`?**
Both are microtask-style queues drained between phases. `nextTick` has higher priority and runs first.

**11. `setTimeout(fn, 0)` vs `setImmediate(fn)`?**
Timer runs in the Timers phase, immediate in the Check phase. From the main module, order isn't guaranteed; inside an I/O callback, `setImmediate` runs first.

**12. What happens when the event loop is blocked?**
No callbacks, timers, or requests are processed — latency spikes and the server appears frozen.

</details>

<details>
<summary><strong>🔴 Advanced</strong></summary>

**13. Which operations use the libuv thread pool?**
File system operations, `dns.lookup`, some crypto functions (e.g. `pbkdf2`, `scrypt`), and zlib. Network I/O mostly uses OS-level async mechanisms instead.

**14. What is the default thread pool size and how do you change it?**
4. Use the `UV_THREADPOOL_SIZE` environment variable.

**15. Why can't `async/await` fix CPU-heavy code?**
It only changes how results are *handled*, not where the computation runs. The CPU work still happens on the main thread.

**16. How would you handle CPU-intensive tasks?**
Worker Threads, child processes/cluster, job queues, or dedicated services.

**17. Can microtasks starve the event loop?**
Yes. If microtasks keep scheduling new microtasks forever, the loop can never move to the next phase.

**18. Explain the relationship between V8 and libuv.**
V8 executes JavaScript; libuv provides the event loop and async I/O. Node.js bindings connect them.

</details>

---

# 📋 Cheat Sheet

```text
┌──────────────────────── DAY 1 CHEAT SHEET ────────────────────────┐
│                                                                    │
│  Node.js  = V8 + libuv + Node APIs + bindings                      │
│  V8       = executes JS            libuv = event loop + async I/O  │
│                                                                    │
│  Execution priority:                                               │
│     1. Sync code                                                   │
│     2. process.nextTick                                            │
│     3. Promise / queueMicrotask                                    │
│     4. Event loop phases                                           │
│                                                                    │
│  Phases: Timers → Pending → Idle → Poll → Check → Close            │
│                                                                    │
│  Thread pool: default 4  (fs, crypto, zlib, dns.lookup)            │
│  Network I/O: handled by OS async mechanisms                       │
│                                                                    │
│  I/O-bound → Node is great ✅                                      │
│  CPU-bound → Worker Threads / queues / services 🛠️                 │
│                                                                    │
│  setTimeout(fn, ms) = MINIMUM delay, not exact                     │
│  async/await does NOT create threads                               │
└────────────────────────────────────────────────────────────────────┘
```

---

# 📖 Glossary

| Term | Simple meaning |
|---|---|
| **Runtime** | Environment that provides everything needed to run code |
| **Engine** | Software that reads and executes code (V8) |
| **Call Stack** | Where JS tracks currently running functions |
| **Callback** | A function passed to be called later |
| **Promise** | An object representing a future result |
| **Event Loop** | Loop that moves ready callbacks to the call stack |
| **libuv** | C library behind Node's async I/O and event loop |
| **Thread Pool** | A set of background threads for some heavy operations |
| **Blocking** | Code that makes everything else wait |
| **Non-blocking** | Code that lets other work continue while waiting |
| **I/O** | Input/Output: files, network, database |
| **CPU-bound** | Work limited by processing power |
| **Microtask** | High-priority task queue (Promises, nextTick) |
| **Worker Thread** | Separate JS thread for CPU-heavy work |
| **Latency** | Time a user waits for a response |
| **Concurrency** | Handling many tasks in overlapping time |

---

# ✅ Progress Tracker

Tick these off as you complete them:

**📖 Concepts**
- [ ] Read Parts 1–4
- [ ] Ran every code example myself
- [ ] Can explain the event loop without looking at notes

**💻 Tasks**
- [ ] Task 1–4 (Level 1)
- [ ] Task 5–8 (Level 2)
- [ ] Task 9–12 (Level 3)

**🏗️ Project**
- [ ] Built the Event Loop Lab server
- [ ] Tested `/block` vs `/worker`
- [ ] Wrote `NOTES.md` with my observations

**🎤 Interview Ready**
- [ ] Answered all Beginner questions aloud
- [ ] Answered all Intermediate questions aloud
- [ ] Attempted the Advanced questions

> 🏆 **Score yourself:** 10+ items ticked = ready for Day 2. Less than that? Revisit the tasks you skipped. Repetition is how it sticks.

---

# 📚 Further Reading

- 📘 [Node.js Official Docs — The Event Loop, Timers, and `process.nextTick()`](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- 📘 [Node.js Docs — Overview of Blocking vs Non-Blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
- 📘 [Node.js Docs — Worker Threads](https://nodejs.org/api/worker_threads.html)
- 📘 [libuv Documentation](https://docs.libuv.org/)
- 📘 [MDN — Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)

---

<div align="center">

## 🎉 Day 1 Complete!

You now understand **how Node.js actually thinks**. Everything in the next 19 days (Express, middleware, authentication, databases, WebSockets) is built on this foundation.

### ➡️ Next up: **Day 2 — Modules, `require` vs `import`, and `npm`**

---

*Part of the **20 Days • 20 Concepts** — Node.js & Express.js Learning Series*

⭐ *If this helped you, star the repo and share your progress with* `#20DaysNodeJS`

</div>
