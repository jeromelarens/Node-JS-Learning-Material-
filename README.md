<!-- HEADER BANNER -->
<div align="center">

# 🚀 Day 6 — Callbacks, Promises & Async/Await

### ⚡ 20 Days • 20 Concepts — Node.js & Express.js Learning Series

<img src="https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js"/>
<img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript"/>
<img src="https://img.shields.io/badge/Async-Await-blueviolet?style=for-the-badge" alt="Async"/>
<img src="https://img.shields.io/badge/Status-✅_Completed-brightgreen?style=for-the-badge" alt="Status"/>

> 🧠 **"Node.js won't wait for anyone. Learn how it never blocks — and how you shouldn't either."**

[![Progress](https://img.shields.io/badge/Progress-6%2F20-0e75b6?style=for-the-badge)](https://github.com/)
[![Topics](https://img.shields.io/badge/Topics_Covered-75-orange?style=for-the-badge)](https://github.com/)
[![Difficulty](https://img.shields.io/badge/Level-🔥🔥🔥🔥-red?style=for-the-badge)](https://github.com/)

</div>

---

## 🗺️ Roadmap Navigator

```text
Day 0 → Day 1 → Day 2 → Day 3 → Day 4 → Day 5 → ⭐ DAY 6 ⭐ → Day 7 → ... → Day 20
🔜      ✅      ✅      ✅      ✅      ✅      🚀       🔜           🔜
```

---

## 🎯 What I Learned Today

Node.js is built around **asynchronous, non-blocking** operations. Instead of waiting for a file read, DB query, or API call to finish, Node.js starts the operation and keeps doing other work. Today I conquered the three patterns that make this possible:

```text
        Callback
            ↓
         Promise
            ↓
      Async / Await
```

These aren't three random concepts — they're the **evolution** of handling async operations in JavaScript. 💪

---

## 📑 Table of Contents

- [🧠 What is Asynchronous Programming?](#-what-is-asynchronous-programming)
- [📞 Callbacks](#-callbacks)
- [😵 Callback Hell](#-callback-hell)
- [💎 Promises](#-promises)
- [⚡ Async/Await](#-asyncawait)
- [🔥 Promise Combinators](#-promise-combinators)
- [❌ Common Mistakes](#-common-mistakes)
- [🏗️ Real Backend Patterns](#️-real-backend-patterns)
- [🧪 Practical Exercises](#-practical-exercises)
- [🎯 Interview Questions](#-interview-questions)
- [✅ Day 6 Checklist](#-day-6-checklist)

---

## 🧠 What is Asynchronous Programming?

**Synchronous** = one thing at a time, everyone waits 😴

```text
Task A → Wait → Done → Task B → Wait → Done
```

**Asynchronous** = start it, move on, handle the result when it arrives 🏃💨

```text
Start Task A ──┐
               ├─→ Continue other work
Start Task B ──┘
   ↓ Result A arrives → handle it
   ↓ Result B arrives → handle it
```

> 🔑 **Key idea:** Node.js doesn't block the entire app while waiting for I/O (file, DB, network, timer).

```js
// ❌ Blocking — whole app waits
const data = fs.readFileSync("users.json");
console.log(data);

// ✅ Non-blocking — app keeps working
fs.readFile("users.json", (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
console.log("Doing other work meanwhile...");
```

---

## 📞 Callbacks

> A **callback** is a function passed to another function, to be executed *later*.

```js
function greet(name, callback) {
  console.log(`Hello ${name}`);
  callback();
}

greet("Jerome", () => console.log("Callback executed!"));
// Hello Jerome
// Callback executed!
```

### ❗ Error-First Callback Pattern

Node.js tradition: `callback(error, result)`

```js
fs.readFile("users.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Failed:", err);
    return;        // ⛔ always guard against errors
  }
  console.log(data);
});
```

```text
     error?
    /      \
  yes       no
   ↓         ↓
handle     use result
error
```

<details>
<summary>📁 Real Node.js example — fs module</summary>

```js
const fs = require("node:fs");

fs.readFile("users.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Failed to read file:", err);
    return;
  }
  console.log(data);
});
```

</details>

---

## 😵 Callback Hell

Callbacks are fine... until they multiply. 🕳️

```js
getUser(userId, (err, user) => {
  getOrders(user.id, (err, orders) => {
    getProducts(orders, (err, products) => {
      sendEmail(user, products, (err) => {
        logActivity((err) => {
          // ...where am I even? 😵
        });
      });
    });
  });
});
```

```text
Operation A
   ↓
Operation B
   ↓
Operation C
   ↓
Operation D      →  code drifts RIGHT → hard to read/debug/test
```

**Symptoms:** unreadable, undebuggable, unmaintainable. The solution? ⬇️

---

## 💎 Promises

> A **Promise** represents a value that may be available **now, later, or never**.

### 🧠 Three States

```text
              ┌──────────┐
              │ Pending  │   still running…
              └────┬─────┘
            ┌──────┴──────┐
            ↓             ↓
        Fulfilled      Rejected    ✅ success   ❌ failure
```

> A *settled* Promise is either **Fulfilled** or **Rejected**.

### 🏗️ Creating a Promise

```js
const promise = new Promise((resolve, reject) => {
  const success = true;

  if (success) resolve("Operation successful ✅");
  else reject(new Error("Operation failed ❌"));
});
```

| Method | Meaning |
| --- | --- |
| `resolve(value)` | ✅ Operation succeeded |
| `reject(error)` | ❌ Operation failed |

### 🔗 Handling & Chaining

```js
getUser()
  .then((user) => getOrders(user.id))    // ⬅️ RETURN the promise!
  .then((orders) => getProducts(orders))
  .then((products) => console.log(products))
  .catch((error) => console.error("Chain failed:", error))
  .finally(() => console.log("Done (always runs)"));
```

> ⚠️ **GOLDEN RULE:** Always `return` the Promise inside `.then()` — otherwise the next `.then()` won't wait for it!

### 🌊 Error Propagation

```text
getUser()
   ↓
getOrders()   ← any rejection skips to .catch()
   ↓
processOrders()
   ↓
ERROR ──────→ .catch() handles it
```

<details>
<summary>💡 Complete Promise lifecycle example</summary>

```js
Promise.resolve("Hello")
  .then((result) => {
    console.log(result);      // Hello
    return result + " World";
  })
  .then((result) => {
    console.log(result);      // Hello World
  })
  .catch(console.error)
  .finally(() => console.log("Cleanup done"));
```

</details>

---

## ⚡ Async/Await

Promise chains are good. Async/await makes them **beautiful** — sequential-looking code that's still non-blocking. ✨

```js
// Before (promise chain)
getUser()
  .then((user) => getOrders(user.id))
  .then((orders) => processOrders(orders))
  .then((result) => console.log(result));

// After (async/await) 😍
async function processUser() {
  const user = await getUser();
  const orders = await getOrders(user.id);
  const result = await processOrders(orders);
  console.log(result);
}
```

### 🧠 The Two Keywords

| Keyword | What it does |
| --- | --- |
| `async` | Function **always returns a Promise** |
| `await` | Pauses **that function's continuation** until the Promise settles |

```js
async function greet() {
  return "Hello";   // conceptually → Promise.resolve("Hello")
}

greet().then((msg) => console.log(msg));   // Hello
```

### 🚫 Does `await` block Node.js?

> **NO!** — *classic interview question* 🎯

```text
await → pauses THIS async function only
      → Node.js keeps handling other work
      → Promise settles → function resumes
```

### 🧯 Error Handling

```js
async function getUserData() {
  try {
    const user = await getUser();
    console.log(user);
  } catch (error) {
    console.error("Failed:", error);
  } finally {
    console.log("Cleanup completed");
  }
}
```

### 🔬 Bonus: Microtasks & the Event Loop (Day 1 connects!)

```js
async function main() {
  console.log("A");
  await Promise.resolve();
  console.log("B");
}
main();
console.log("C");
```

Output: `A → C → B` — the await continuation waits in the **microtask queue**, after synchronous code finishes.

---

## 🔥 Promise Combinators

When you have multiple async operations, choosing the right combinator matters:

### ⚡ `Promise.all()` — All must succeed

```js
const [users, products, orders] = await Promise.all([
  getUsers(),
  getProducts(),
  getOrders()
]);
```

```text
getUsers    ────────────>
getProducts ────────────>     concurrent! total time = slowest task
getOrders   ────────────>
```

> ⚠️ If **one rejects → whole thing rejects.** Already-started ops aren't cancelled.

### 📊 `Promise.allSettled()` — Get every result

```js
const results = await Promise.allSettled([taskA(), taskB(), taskC()]);
// [{status:"fulfilled", value:...}, {status:"rejected", reason:...}, ...]
```

Never rejects due to input failures — perfect for **batch operations**.

### 🏎️ `Promise.race()` — First to *settle* (win or lose)

```js
const result = await Promise.race([taskA(), taskB()]);
```

### 🥇 `Promise.any()` — First to *succeed*

```js
const result = await Promise.any([serverA(), serverB(), serverC()]);
// rejects with AggregateError only if ALL reject
```

### 📋 Quick Comparison

| Method | Completes when | Failure behavior |
| --- | --- | --- |
| `Promise.all()` | All fulfill | Rejects if **one** rejects |
| `Promise.allSettled()` | All settle | **Never** rejects on input failure |
| `Promise.race()` | First settles | First rejection rejects |
| `Promise.any()` | First fulfills | Rejects only if **all** reject |

### 🧠 Sequential vs Concurrent — The Big Decision

```js
// ❌ Independent ops run sequentially — wasted time
const profile = await getProfile();
const notifs = await getNotifications();

// ✅ Independent → concurrent
const [profile, notifs] = await Promise.all([
  getProfile(),
  getNotifications()
]);

// ✅ Dependent → sequential is CORRECT
const user = await createUser();
const profile = await createProfile(user.id);   // needs user.id!
```

> 🚨 **Don't use `Promise.all()` blindly** — dependent tasks must be sequential!

---

## ❌ Common Mistakes

| # | Mistake | Fix |
| --- | --- | --- |
| 1 | Forgetting `await` → `user` is a Promise | `const user = await getUser();` |
| 2 | Forgetting `return` in `.then()` | `return getData();` |
| 3 | `users.forEach(async ...)` — forEach doesn't wait | Use `for...of` or `Promise.all(users.map(...))` |
| 4 | Sequential awaits for independent ops | Use `Promise.all()` |
| 5 | `Promise.all()` for dependent ops | Sequential await |
| 6 | Ignoring rejected Promises | Always `try/catch` or `.catch()` |
| 7 | Thinking async = parallel CPU | Async ≠ threads; CPU-heavy work needs Worker Threads |
| 8 | Unlimited concurrent requests | Add concurrency limits |
| 9 | No timeout on external APIs | Use `AbortController` |
| 10 | Mixing callbacks + promises + async randomly | Pick async/await consistently |

### 🔥 The `forEach` trap

```js
// ❌ Broken — forEach ignores the Promises
users.forEach(async (user) => {
  await sendEmail(user);
});
console.log("Done");   // prints BEFORE emails finish!

// ✅ Sequential
for (const user of users) await sendEmail(user);

// ✅ Concurrent
await Promise.all(users.map((user) => sendEmail(user)));
```

### ⏱️ Timeout Pattern

```js
async function fetchWithTimeout(url, timeout = 5000) {
  const controller = new AbortController();
  const timer = setTimeout(() => controller.abort(), timeout);

  try {
    return await fetch(url, { signal: controller.signal });
  } finally {
    clearTimeout(timer);
  }
}
```

---

## 🏗️ Real Backend Patterns

### 🔐 Login Flow (dependent → sequential)

```text
Find user → Verify password → Generate token → Respond
```

```js
async function loginUser(email, password) {
  const user = await findUserByEmail(email);
  if (!user) throw new Error("Invalid credentials");

  const valid = await comparePassword(password, user.password);
  if (!valid) throw new Error("Invalid credentials");

  const token = await generateToken(user.id);
  return { user, token };
}
```

### 🖥️ Express Route with Async/Await

```js
router.get("/users/:id", async (req, res) => {
  try {
    const user = await userService.getUserById(req.params.id);
    res.json({ success: true, data: user });
  } catch (error) {
    res.status(500).json({ success: false, message: "Failed to fetch user" });
  }
});
```

### 🧱 Layered Architecture

```text
Route → Controller → Service → Repository → Database
                          ↕
                    Promise / await
                          ↕
          Prisma  •  Redis  •  External API
```

Every layer returns a Promise. Clean, testable, production-grade. 🏭

---

## 🧪 Practical Exercises I Did

<details>
<summary><b>📝 Exercise 1 — Promise Basics (delay function)</b></summary>

```js
function delay(ms) {
  return new Promise((resolve) =>
    setTimeout(() => resolve(`Completed after ${ms}ms`), ms)
  );
}

const result = await delay(2000);
console.log(result);
```

</details>

<details>
<summary><b>📝 Exercise 2 — Sequential Tasks</b></summary>

```js
async function task(name, delayTime) {
  await delay(delayTime);
  console.log(`${name} completed`);
}

await task("Task A", 1000);
await task("Task B", 1000);
await task("Task C", 1000);
// Total: ~3000ms
```

</details>

<details>
<summary><b>📝 Exercise 3 — Concurrent Tasks</b></summary>

```js
await Promise.all([
  task("Task A", 1000),
  task("Task B", 1000),
  task("Task C", 1000)
]);
// Total: ~1000ms 🚀 3x faster!
```

</details>

<details>
<summary><b>📝 Exercise 4 — Error Handling</b></summary>

```js
function riskyOperation() {
  return new Promise((resolve, reject) => {
    Math.random() > 0.5
      ? resolve("Success ✅")
      : reject(new Error("Operation failed ❌"));
  });
}

try {
  console.log(await riskyOperation());
} catch (error) {
  console.error(error.message);
}
```

</details>

<details>
<summary><b>📝 Exercise 5 — allSettled()</b></summary>

```js
const results = await Promise.allSettled([
  riskyOperation(), riskyOperation(), riskyOperation(), riskyOperation()
]);
console.log(results);   // every outcome, success or failure
```

</details>

### 🏆 Challenge Built — Async Data Processing System

```text
users.json + products.json + orders.json
        ↓ read asynchronously (Promise.all)
        ↓ parse JSON
        ↓ find user → find orders → match products
        ↓ calculate total value
        ↓ return result with proper error handling
```

**Bonus:** Exposed as `GET /users/:id/orders` Express endpoint with service + repository layers. 🎉

---

## 🎯 Interview Questions

<details>
<summary><b>🟢 Beginner</b></summary>

1. **What is a callback?** — A function passed to another function, executed later (usually after an async op completes).
2. **What is callback hell?** — Deeply nested callback code that's hard to read, debug and maintain.
3. **What is a Promise?** — An object representing the eventual completion/failure of an async operation and its value.
4. **Promise states?** — `Pending → Fulfilled` or `Pending → Rejected`.
5. **`resolve()`?** — Fulfills the Promise with a value. **`reject()`?** — Rejects it with a reason (usually an Error).

</details>

<details>
<summary><b>🟡 Intermediate</b></summary>

6. **Promise vs async/await?** — Async/await is syntax **built on Promises** for sequential-looking code.
7. **What does an async function return?** — **Always a Promise.**
8. **What does `await` do?** — Pauses the async function until the Promise settles; yields value or throws.
9. **Does await block Node.js?** — **No!** Only that function's continuation pauses; the event loop keeps spinning.
10. **`Promise.all()`?** — Waits for all to fulfill; rejects if one rejects.
11. **all() vs allSettled()?** — all rejects on first failure; allSettled reports every outcome.

</details>

<details>
<summary><b>🔴 Advanced</b></summary>

12. **When to use Promise.all()?** — Independent operations that can run concurrently.
13. **When sequential await?** — When each step depends on the previous result.
14. **Why doesn't async forEach() wait?** — forEach ignores the Promises its callback returns.
15. **Promise.race()?** — Settles with the first Promise to settle (fulfilled *or* rejected).
16. **Promise.any()?** — Fulfills with the first to succeed; rejects only if ALL reject.
17. **Is async/await synchronous?** — No, just synchronous-*looking* syntax over Promises.
18. **Does async/await create a thread?** — **No.**
19. **How to handle async errors?** — `try/catch` around `await`, or `.catch()` on the chain.

</details>

---

## 🧠 Day 6 Mental Model

```text
                 ASYNC OPERATION
                       │
                       ▼
                  Callback        "Call me when done"
                       │
                       ▼
                   Promise         "Here's the future result"
                       │
                       ▼
                 Async / Await      "Let me write it sequentially"
```

```text
async function
      ↓ returns Promise
await Promise
      ↓ pause function continuation
Node.js continues other work
      ↓ Promise settles
function resumes 🏁
```

---

## ✅ Day 6 Checklist

- [x] Asynchronous programming meaning
- [x] Callbacks & error-first pattern
- [x] Callback hell
- [x] Promise states (pending / fulfilled / rejected)
- [x] `resolve()` & `reject()`
- [x] `.then()` / `.catch()` / `.finally()`
- [x] Promise chaining + returning values
- [x] Promise error propagation
- [x] `async` & `await`
- [x] `try/catch` with async/await
- [x] `Promise.all()` / `allSettled()` / `race()` / `any()`
- [x] Sequential vs concurrent execution
- [x] Async `forEach()` problem
- [x] Async DB & API operations
- [x] Timeout handling (AbortController)
- [x] Async/await & the Event Loop
- [x] Concurrency vs parallelism

---

## 💬 The Quote I'm Carrying Forward

> The goal isn't *"use async/await everywhere."*
> The real goal is: **understand which operations are independent, which are dependent, how errors propagate, and how to control concurrency safely.**

---

<div align="center">

### 🎓 Day 6 — Complete! ✅

| Day | Topic | Status |
| --- | --- | --- |
| Day 0 | Getting Started & Roadmap | ✅ |
| Day 1 | Node.js Runtime & Event Loop | ✅ |
| Day 2 | CommonJS vs ES Modules | ✅ |
| Day 3 | File System & File Handling | ✅ |
| Day 4 | EventEmitter | ✅ |
| Day 5 | Streams & Buffers | ✅ |
| **Day 6** | **Callbacks, Promises & Async/Await** | **🚀 Today** |
| Day 7 | NPM & Package Management | 🔜 Next |
| ... | ... | ... |
| Day 20 | Production-Ready Express Architecture | 🔜 |

### 🔮 Coming Up — Day 7: NPM & Package Management

`package.json` • `package-lock.json` • dependencies vs devDependencies • semantic versioning • `npm scripts` • `npx` • lockfiles • security auditing

**Learn → Practice → Build → Understand** 💪

</div>

---

<div align="center">
<sub>📅 Series started: Day 0 &nbsp;|&nbsp; 🏗️ Built with Node.js &nbsp;|&nbsp; 📖 Part of 20 Days • 20 Concepts</sub>
</div>
