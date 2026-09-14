# 🚀 Day 1 — Node.js Runtime & Event Loop

> Part of the **20 Days • 20 Concepts — Node.js & Express.js** learning series

Understanding the Node.js runtime and Event Loop is one of the most important foundations for becoming a good backend developer. Before building APIs, authentication systems, payment integrations, or WebSocket servers, you need to understand *how Node.js actually executes your JavaScript code*.

---

## 📌 Table of Contents

- [What You Will Learn](#-what-you-will-learn)
- [1. What is Node.js?](#1-what-is-nodejs)
- [2. Node.js Is Not a Programming Language](#2-nodejs-is-not-a-programming-language)
- [3. The V8 Engine](#3-the-v8-engine)
- [4. Event-Driven, Non-Blocking I/O](#4-event-driven-non-blocking-io)
- [5. Blocking vs Non-Blocking Code](#5-blocking-vs-non-blocking-code)
- [6. The Call Stack](#6-the-call-stack)
- [7. Synchronous vs Asynchronous Execution](#7-synchronous-vs-asynchronous-execution)
- [8. The Event Loop](#8-the-event-loop)
- [9. libuv](#9-libuv)
- [10. Event Loop Phases](#10-event-loop-phases)
- [11. setTimeout vs setImmediate](#11-settimeout-vs-setimmediate)
- [12. Microtasks](#12-microtasks)
- [13. Is Node.js Really Single-Threaded?](#13-is-nodejs-really-single-threaded)
- [14. CPU-Bound vs I/O-Bound Work](#14-cpu-bound-vs-io-bound-work)
- [15. Practical Example](#15-practical-example)
- [16. Common Misconceptions](#16-common-misconceptions)
- [17. Key Takeaways](#17-key-takeaways)
- [Interview Questions](#-interview-questions)
- [Practice Tasks](#️-practice-tasks)

---

## 📌 What You Will Learn

By the end of this concept, you should understand:

- [ ] What Node.js actually is
- [ ] How Node.js executes JavaScript
- [ ] What the V8 engine does
- [ ] What the Call Stack is
- [ ] What asynchronous operations are
- [ ] What libuv does
- [ ] What the Event Loop does
- [ ] How Node.js handles non-blocking I/O
- [ ] Why Node.js can handle many concurrent requests
- [ ] The difference between sync and async execution
- [ ] Common misconceptions about Node.js being "single-threaded"

---

## 1. What is Node.js?

**Node.js** is a JavaScript runtime environment that allows JavaScript to run **outside the browser**.

```
Browser              Node.js
  │                    │
  ▼                    ▼
JavaScript          JavaScript
  │                    │
  ▼                    ▼
V8 Engine           V8 Engine
                       │
                       ▼
              Server-side Applications
```

With Node.js, you can build:

- REST APIs
- Web servers
- Authentication systems
- Real-time applications
- CLI tools
- Microservices
- Streaming applications
- Backend services

---

## 2. Node.js Is Not a Programming Language

A common beginner mistake is confusing Node.js with a language.

| Term | What it actually is |
|------|----------------------|
| **JavaScript** | The programming language |
| **Node.js** | The runtime environment that runs JS outside the browser |
| **V8** | The engine that executes the JavaScript |

```
JavaScript → Node.js Runtime → V8 Engine → Machine Execution
```

---

## 3. The V8 Engine

Node.js uses Google's **V8** engine to execute JavaScript.

V8 handles:

- Parsing JavaScript
- Compiling JavaScript
- Executing JavaScript
- Memory management & garbage collection

But V8 alone can't build a backend server — it doesn't know about `fs.readFile()` or `http.createServer()`. **Node.js provides those APIs around V8.**

---

## 4. Event-Driven, Non-Blocking I/O

The defining trait of Node.js is its **event-driven, non-blocking I/O model**.

A blocking system waits for one operation before starting the next. Node.js starts an I/O operation and keeps working instead of waiting idly:

```
Request → Start I/O → Continue other work → I/O completes
        → Callback / Promise ready → JS handles the result
```

This is a big part of why Node.js excels at I/O-heavy applications.

---

## 5. Blocking vs Non-Blocking Code

**Synchronous (blocking) read:**

```js
const fs = require("fs");

const data = fs.readFileSync("data.txt", "utf8");
console.log(data);
console.log("Done");
```

Execution waits for the file read to finish before continuing.

**Asynchronous (non-blocking) read:**

```js
const fs = require("fs");

fs.readFile("data.txt", "utf8", (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});

console.log("Done");
```

Output:

```
Done
<file contents>
```

Node.js starts the file operation and doesn't block JS execution while waiting.

---

## 6. The Call Stack

The **Call Stack** tracks which functions are currently executing.

```js
function first() {
  second();
}
function second() {
  console.log("Hello");
}
first();
```

```
┌─────────────┐
│  second()   │
├─────────────┤
│  first()    │
├─────────────┤
│   global    │
└─────────────┘
```

Rule: **Push → Execute → Pop.** The most recently added function runs first, then is removed once it finishes.

---

## 7. Synchronous vs Asynchronous Execution

**Synchronous** — runs top to bottom, each line waits for the previous:

```js
console.log("A");
console.log("B");
console.log("C");
// A, B, C
```

**Asynchronous** — some operations are started without blocking the rest of the code:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer finished");
}, 2000);

console.log("End");
// Start, End, Timer finished
```

---

## 8. The Event Loop

The **Event Loop** coordinates when callbacks and other scheduled JS work get executed.

```
Call Stack → JS executes → Async operation
   → Node/OS/libuv handles I/O → Operation completes
   → Callback ready → Event Loop → Call Stack
```

> The Event Loop lets Node.js coordinate async work **without blocking** the main JavaScript execution path.

---

## 9. libuv

**libuv** is the library providing the infrastructure behind Node's async I/O model:

- Event loop
- Async I/O
- Networking
- File-system operations
- Timers
- Thread pool

```
            Node.js
               │
     ┌─────────┴─────────┐
     │                   │
    V8                 libuv
     │                   │
JS Execution        Async I/O / Event Loop
                          │
                     Thread Pool
```

Mental model: **Node.js = V8 + libuv + Node APIs + other runtime components** — not just V8.

---

## 10. Event Loop Phases

```
Timers → Pending Callbacks → Poll → Check → Close Callbacks → (next iteration)
```

| Phase | Handles |
|-------|---------|
| **Timers** | `setTimeout()`, `setInterval()` callbacks |
| **Pending Callbacks** | Certain deferred callbacks from previous operations |
| **Poll** | I/O-related callbacks; decides whether to wait for more I/O |
| **Check** | `setImmediate()` callbacks |
| **Close Callbacks** | Close-related events (e.g. `socket.on("close", ...)`) |

> A timer sets a **minimum** delay before it's eligible to run — not a guaranteed exact time.

---

## 11. setTimeout vs setImmediate

```js
setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
```

From the main module, the order between these two is **not guaranteed**.

But inside an I/O callback, `setImmediate()` is designed to run **before** a timer scheduled in that same callback:

```js
const fs = require("fs");

fs.readFile(__filename, () => {
  setTimeout(() => console.log("timeout"), 0);
  setImmediate(() => console.log("immediate"));
});

// Typically:
// immediate
// timeout
```

**Lesson:** don't memorize one universal output — understand the *context* in which callbacks are scheduled.

---

## 12. Microtasks

Node.js processes microtasks such as `Promise.then()` and `queueMicrotask()`. There's also `process.nextTick()`, which uses its own queue with special priority.

```js
console.log("Start");

Promise.resolve().then(() => console.log("Promise"));

console.log("End");

// Start
// End
// Promise
```

### Full example

```js
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

console.log("4");

// 1
// 4
// 3
// 2
```

**Why:** synchronous code (`1`, `4`) runs first, then microtasks (`3`), then timers (`2`).

---

## 13. Is Node.js Really Single-Threaded?

**Partially true, often misunderstood.**

- JavaScript execution runs on **one main thread**.
- But Node.js as a whole isn't limited to one thread — it uses **libuv's thread pool** for certain operations, plus **Worker Threads** for CPU-intensive JS.

```
             Node.js Process
                    │
             Main JS Thread
                    │
             ┌──────┴──────┐
             │   libuv     │
             │ Thread Pool │
             └──────┬──────┘
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      Worker      Worker      Worker
```

> Node.js uses a single main JS execution thread by default, but the overall runtime can use multiple threads and OS facilities.

---

## 14. CPU-Bound vs I/O-Bound Work

| I/O-Bound | CPU-Bound |
|-----------|-----------|
| Database queries | Heavy math calculations |
| HTTP requests | Image processing |
| File operations | Large data transformations |
| Network communication | Encryption/compression |

Node.js handles I/O-bound work efficiently since it avoids blocking the main thread while waiting.

CPU-heavy code on the main thread, however, **blocks the event loop**:

```
CPU-heavy task → Main thread blocked
              → Event loop can't process other callbacks
              → Requests get delayed
```

**Solutions:** Worker Threads, separate services, job queues, background workers, process-based scaling.

---

## 15. Practical Example

```js
// server.js
const http = require("http");

const server = http.createServer((req, res) => {
  console.log("Request received");

  setTimeout(() => {
    res.end("Response from Node.js");
  }, 2000);
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

```bash
node server.js
# then open http://localhost:3000
```

While a request waits on the timer, Node.js isn't frozen for those two seconds — the event loop keeps handling other eligible work.

---

## 16. Common Misconceptions

| ❌ Misconception | ✅ Reality |
|------------------|-----------|
| "Node.js executes everything asynchronously." | Normal JS execution is synchronous by default. |
| "Node.js has no threads." | It has a main JS thread, plus libuv's thread pool and Worker Threads. |
| "`setTimeout(fn, 0)` runs immediately." | It becomes *eligible* after the delay, subject to event-loop scheduling. |
| "The Event Loop makes CPU-heavy code async." | CPU-bound work on the main thread still blocks the loop. |
| "Node.js is always faster than other backends." | Performance depends on workload, architecture, DB, network, and more. |

---

## 17. Key Takeaways

**Node.js**
- JavaScript runtime environment
- Runs JS outside the browser
- Uses V8 to execute JavaScript
- Provides server-side APIs
- Event-driven architecture

**Event Loop**
- Coordinates async callback execution
- Enables non-blocking I/O
- Works with libuv and Node.js internals
- Does **not** make CPU-heavy JS non-blocking

**Mental model**

```
JS Code → Call Stack → Node APIs / libuv / OS
        → Async Operation → Callback/Promise ready
        → Event Loop → Call Stack → JS executes
```

---

## 🧠 Interview Questions

<details>
<summary><strong>Beginner</strong></summary>

1. **What is Node.js?**
   A JavaScript runtime environment that allows JS to run outside the browser.
2. **What engine does Node.js use?**
   Google's V8 JavaScript engine.
3. **What is the Event Loop?**
   The core mechanism that coordinates when eligible callbacks and other JS work are executed.
4. **Is Node.js single-threaded?**
   JS execution runs on a main thread by default, but Node.js can use other threads/OS facilities for certain operations.

</details>

<details>
<summary><strong>Intermediate</strong></summary>

5. **What is non-blocking I/O?**
   An I/O operation can be started without forcing the main JS thread to wait synchronously for it to finish.
6. **What is libuv?**
   The library providing the event loop and async I/O infrastructure, including a thread pool.
7. **What happens when the Event Loop is blocked?**
   Other JS callbacks can't run normally — this increases latency and delays requests.
8. **Difference between `setTimeout()` and `setImmediate()`?**
   They use different event-loop mechanisms; inside an I/O callback, `setImmediate()` generally runs before a timer scheduled at the same time.

</details>

<details>
<summary><strong>Advanced</strong></summary>

9. **Why is Node.js suitable for I/O-heavy apps?**
   Its event-driven, non-blocking model avoids synchronously waiting for I/O.
10. **Why can CPU-intensive code be problematic?**
    It can block the main thread and prevent the event loop from processing other work.
11. **How can CPU-heavy tasks be handled?**
    Worker Threads, background workers, job queues, separate processes, or dedicated services.
12. **Relationship between V8 and libuv?**
    V8 executes JavaScript; libuv provides the async I/O and event-loop infrastructure Node.js relies on.

</details>

---

## 🛠️ Practice Tasks

Try these yourself before moving to Day 2.

### Task 1 — Execution Order

Predict the output, then run it to verify:

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

console.log("D");
```

### Task 2 — Timer Experiment

```js
setTimeout(() => console.log("Timer 1"), 1000);
setTimeout(() => console.log("Timer 2"), 0);
setTimeout(() => console.log("Timer 3"), 500);
```

Observe and explain the execution order.

### Task 3 — Blocking Experiment

Write a CPU-intensive `while` loop and observe what happens to other timers or requests while it runs. Then research how **Worker Threads** solve this problem.

---

## 📚 Mental Model to Remember

Don't just memorize the Event Loop diagram — think of it as a flow:

```
JavaScript
    │
    ▼
Call Stack
    │
 ┌──┴───────────┐
 ▼              ▼
Sync Work    Async Work
                 │
                 ▼
        Node.js / libuv / OS
                 │
                 ▼
         Operation Complete
                 │
                 ▼
      Callback / Microtask
                 │
                 ▼
            Event Loop
                 │
                 ▼
             Call Stack
```

---

### ✅ Next up: Day 2

Continue the series to see how these concepts power **Express.js** routing, middleware, and real backend applications.

