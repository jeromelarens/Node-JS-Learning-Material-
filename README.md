<div align="center">

# 🚀 Day 2 — Node.js Modules
### CommonJS vs ES Modules

![Node.js](https://img.shields.io/badge/Node.js-20.x-339933?style=for-the-badge&logo=node.js&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Series](https://img.shields.io/badge/20%20Days-20%20Concepts-blueviolet?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)

**A 20-Day Node.js & Express.js Learning Series**

</div>

---

## 👋 Welcome to Day 2

Today we crack open one of the **most important concepts** in Node.js backend development:

> 🧠 **How Node.js divides code into modules — and how different files talk to each other.**

Real backends are never a single giant file. They're split into:

`Routes` · `Controllers` · `Services` · `Repositories` · `Middleware` · `Utilities` · `Config` · `Database logic`

To make that architecture possible → we need **Modules**. Let's dive in. 👇

---

## 📚 Table of Contents

<details>
<summary><strong>Click to expand full index (27 sections)</strong></summary>

1. [What is a Module?](#1--what-is-a-module)
2. [Why Do We Need Modules?](#2--why-do-we-need-modules)
3. [Node.js Module Systems](#3--nodejs-module-systems)
4. [CommonJS](#4--commonjs)
5. [module.exports](#5--moduleexports)
6. [Exporting Multiple Values](#6--exporting-multiple-values)
7. [The exports Object](#7--the-exports-object)
8. [CommonJS require()](#8--commonjs-require)
9. [ES Modules](#9--es-modules)
10. [Named Exports](#10--named-exports)
11. [Default Export](#11--default-export)
12. [Enabling ES Modules](#12--enabling-es-modules)
13. [.js vs .cjs vs .mjs](#13--js-vs-cjs-vs-mjs)
14. [Built-in Modules](#14--built-in-modules)
15. [Local Modules](#15--local-modules)
16. [Third-Party Modules](#16--third-party-modules)
17. [How Module Resolution Works](#17--how-module-resolution-works)
18. [Module Caching](#18--module-caching)
19. [Circular Dependencies](#19--circular-dependencies)
20. [CommonJS vs ESM](#20--commonjs-vs-esm)
21. [Real Backend Architecture](#21--real-backend-architecture)
22. [Practical Project](#22--practical-project)
23. [Common Mistakes](#23--common-mistakes)
24. [Interview Questions](#24--interview-questions)
25. [Practice Tasks](#25--practice-tasks)
26. [Key Takeaways](#26--key-takeaways)
27. [Day 3 Preview](#27--day-3-preview)

</details>

---

## 1. 📦 What is a Module?

A **module** is simply a reusable, isolated piece of code.

```text
project/
│
├── server.js
├── user.js
├── auth.js
└── database.js
```

**`user.js`**
```js
function getUser() {
    return { id: 1, name: "Jerome" };
}
```

We want to use `getUser()` in another file — that's where modules step in:

```text
Export
   ↓
Function / Variable / Class
   ↓
Import
   ↓
Another File
```

Modules = how different files communicate with each other.

---

## 2. 🤔 Why Do We Need Modules?

Imagine a backend with **50,000 lines** crammed into one `server.js`. It becomes hard to:

`understand` · `maintain` · `debug` · `test` · `reuse` · `collaborate on`

**Instead, organize it:**

```text
src/
│
├── controllers/
│   ├── auth.controller.js
│   └── user.controller.js
│
├── services/
│   ├── auth.service.js
│   └── user.service.js
│
├── routes/
│   ├── auth.routes.js
│   └── user.routes.js
│
├── middleware/
│   └── auth.middleware.js
│
├── utils/
│   └── generateToken.js
│
└── server.js
```

> 💡 Every module has **one responsibility**. This is a foundation of clean backend architecture.

---

## 3. 🧩 Node.js Module Systems

```text
Node.js Modules
      │
      ├── CommonJS
      │
      └── ES Modules
```

| System | Uses |
|---|---|
| **CommonJS** | `require()` + `module.exports` |
| **ES Modules** | `import` + `export` |

```js
// CommonJS
const math = require("./math");

// ES Modules
import { add } from "./math.js";
```

---

## 4. 🟦 CommonJS

The traditional module system widely used in Node.js apps.

```text
module.exports  →  Export
require()       →  Import
```

**`math.js`**
```js
function add(a, b) {
    return a + b;
}

module.exports = add;
```

**`app.js`**
```js
const add = require("./math");

console.log(add(10, 20));
```

**Output**
```text
30
```

**Flow**
```text
math.js → module.exports → add() → require() → app.js
```

---

## 5. 📤 module.exports

Determines what another module receives via `require()`.

```js
function greet(name) {
    return `Hello ${name}`;
}

module.exports = greet;
```

```js
const greet = require("./greet");
console.log(greet("Jerome"));
// Hello Jerome
```

### Exporting an object

```js
function add(a, b) { return a + b; }
function subtract(a, b) { return a - b; }

module.exports = { add, subtract };
```

```js
const math = require("./math");

console.log(math.add(10, 5));      // 15
console.log(math.subtract(10, 5)); // 5
```

---

## 6. 🎁 Exporting Multiple Values

```js
module.exports = {
    name: "Node.js",
    version: "20",
    type: "runtime"
};
```

```js
const nodeInfo = require("./nodeInfo");

console.log(nodeInfo.name);    // Node.js
console.log(nodeInfo.version); // 20
```

---

## 7. ⚠️ The exports Object

You'll often see:

```js
exports.add = add;
```

instead of `module.exports.add = add;` — because initially `exports` points to `module.exports`.

```js
exports.add = add;
exports.subtract = subtract;   // ✅ works fine
```

> 🚫 **Careful:** `exports = add;` does **NOT** replace the module's exported value.

### The Rule

| Goal | Use |
|---|---|
| Replace the entire export | `module.exports = ...` |
| Add a property | `exports.something = ...` |

---

## 8. 📥 CommonJS require()

```js
const userService = require("./user.service");
```

| Path | Meaning |
|---|---|
| `require("./math")` | Same directory |
| `require("../utils/logger")` | Parent directory |
| `require("./services/user.service")` | Nested directory |

---

## 9. 🟩 ES Modules

The standardized JavaScript module system.

**`math.js`**
```js
export function add(a, b) {
    return a + b;
}
```

**`app.js`**
```js
import { add } from "./math.js";

console.log(add(10, 20));
// 30
```

---

## 10. 🏷️ Named Exports

```js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; }
```

```js
import { add, subtract, multiply } from "./math.js";
```

**Rename an import**
```js
import { add as sum } from "./math.js";
console.log(sum(10, 20));
```

**Import everything**
```js
import * as math from "./math.js";

console.log(math.add(10, 20));
console.log(math.subtract(10, 5));
```

---

## 11. ⭐ Default Export

A module can have **one** default export.

**`logger.js`**
```js
export default function logger(message) {
    console.log(`[LOG]: ${message}`);
}
```

```js
import logger from "./logger.js";
logger("Server started");
```

> No `{ }` needed for default imports.

| Type | Export | Import |
|---|---|---|
| **Named** | `export function add() {}` | `import { add } from "./math.js"` |
| **Default** | `export default function add() {}` | `import add from "./math.js"` |

---

## 12. ⚙️ Enabling ES Modules

Add to `package.json`:

```json
{
    "type": "module"
}
```

```text
project/
│
├── package.json   → { "type": "module" }
├── app.js
└── math.js
```

---

## 13. 🗂️ .js vs .cjs vs .mjs

| Extension | Meaning |
|---|---|
| `.js` | Depends on package configuration |
| `.cjs` | Always CommonJS |
| `.mjs` | Always ES Module |

**CommonJS (`math.cjs`)**
```js
module.exports = { add };
```

**ES Module (`math.mjs`)**
```js
export { add };
```

---

## 14. 🔧 Built-in Modules

```text
fs · path · http · events · crypto · os · url · stream · util
```

Modern Node.js code commonly uses the `node:` prefix:

```js
// CommonJS
const fs = require("node:fs");

// ESM
import fs from "node:fs";
```

**Example — `path`**
```js
const path = require("node:path");
console.log(path.join("users", "profile", "data.json"));
```

---

## 15. 🏠 Local Modules

```text
project/
│
├── app.js
├── calculator.js
└── user.js
```

```js
// calculator.js
function add(a, b) { return a + b; }
module.exports = add;
```

```js
// app.js
const add = require("./calculator");
console.log(add(20, 30));
```

> `./` tells Node this is a **local module** path.

---

## 16. 📦 Third-Party Modules

Installed via npm:

```bash
npm install express
```

```js
const express = require("express");
// or
import express from "express";
```

**Popular packages**
```text
express · cors · jsonwebtoken · bcrypt · zod · prisma · redis · dotenv
```

Stored inside `node_modules/`.

---

## 17. 🔍 How Module Resolution Works

```text
require("./utils/logger")
            │
            ↓
      Resolve the path
            │
            ↓
     Load the module
            │
            ↓
       Execute it
            │
            ↓
    Return module.exports
```

For package imports, Node.js follows its resolution rules — including package metadata and `node_modules`.

---

## 18. 💾 Module Caching

```js
// counter.js
let count = 0;
count++;

console.log("Counter module executed");
module.exports = count;
```

```js
// app.js
const first = require("./counter");
const second = require("./counter");

console.log(first);
console.log(second);
```

> ✅ `"Counter module executed"` only prints **once** — the module is cached after first load.

```text
First require() → Load → Execute → Cache
Second require() → Return cached module
```

Important for: `database connections` · `configuration` · `singletons` · `expensive initialization`

---

## 19. 🔄 Circular Dependencies

```text
A → B
↑   ↓
└───┘
```

```js
// user.js
require("./auth");

// auth.js
require("./user");
```

This can create confusing initialization behavior.

**✅ Better approach**

```text
A ↔ B          →         A → Shared Module ← B
(tightly coupled)         (decoupled)
```

```text
user.service
      ↓
shared utility
      ↑
auth.service
```

---

## 20. ⚖️ CommonJS vs ESM

| Feature | CommonJS | ES Modules |
|---|---|---|
| Import | `require()` | `import` |
| Export | `module.exports` | `export` |
| Default export | Manual/object pattern | `export default` |
| Standard JS module system | ❌ No | ✅ Yes |
| Node.js support | ✅ Yes | ✅ Yes |
| Explicit extension | `.cjs` | `.mjs` |
| `.js` behavior | Package-dependent | Package-dependent |
| Common usage | Older Node projects | Modern JS projects |

> ⚠️ This does **NOT** mean `CommonJS = bad` and `ESM = good`. Both are valid — know what your project uses and why.

---

## 21. 🏗️ Real Backend Architecture

```text
src/
│
├── server.js
├── routes/user.routes.js
├── controllers/user.controller.js
├── services/user.service.js
├── repositories/user.repository.js
├── middleware/auth.middleware.js
└── utils/response.js
```

**Request Flow**

```text
Client → Route → Middleware → Controller → Service → Repository → Database
```

**Route**
```js
const express = require("express");
const { getUsers } = require("../controllers/user.controller");

const router = express.Router();
router.get("/users", getUsers);

module.exports = router;
```

**Controller**
```js
const userService = require("../services/user.service");

async function getUsers(req, res) {
    const users = await userService.getUsers();
    res.json({ success: true, data: users });
}

module.exports = { getUsers };
```

**Service**
```js
const userRepository = require("../repositories/user.repository");

async function getUsers() {
    return userRepository.findAll();
}

module.exports = { getUsers };
```

---

## 22. 🛠️ Practical Project

```text
day-2-modules/
│
├── package.json
├── commonjs/
│   ├── math.js
│   └── app.js
└── esm/
    ├── math.js
    └── app.js
```

### Part A — CommonJS

```js
// commonjs/math.js
function add(a, b) { return a + b; }
function subtract(a, b) { return a - b; }
function multiply(a, b) { return a * b; }

module.exports = { add, subtract, multiply };
```

```js
// commonjs/app.js
const math = require("./math");

console.log("Addition:", math.add(10, 5));
console.log("Subtraction:", math.subtract(10, 5));
console.log("Multiplication:", math.multiply(10, 5));
```

```text
Addition: 15
Subtraction: 5
Multiplication: 50
```

### Part B — ES Modules

```json
{ "type": "module" }
```

```js
// esm/math.js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; }
```

```js
// esm/app.js
import { add, subtract, multiply } from "./math.js";

console.log("Addition:", add(10, 5));
console.log("Subtraction:", subtract(10, 5));
console.log("Multiplication:", multiply(10, 5));
```

---

## 23. 🐛 Common Mistakes

| # | Mistake | Fix |
|---|---|---|
| 1 | `require("math")` for a local file | `require("./math")` |
| 2 | `exports = { add };` | `module.exports = { add };` |
| 3 | Mixing CJS/ESM randomly | Know which system the project uses |
| 4 | `import { add } from "./math"` in ESM | Include the extension → `"./math.js"` |
| 5 | One giant `everything.js` | Split responsibilities into modules |

---

## 24. 🎤 Interview Questions

<details>
<summary><strong>🟢 Beginner (1–5)</strong></summary>

1. **What is a module in Node.js?**
   An isolated, reusable piece of code that exposes functionality to other parts of an app.

2. **What is CommonJS?**
   Node.js's traditional module system, using `require()` and `module.exports`.

3. **What are ES Modules?**
   JavaScript's standardized module system, using `import` / `export`.

4. **What is `module.exports`?**
   It defines the value exposed by a CommonJS module.

5. **What is `require()`?**
   It loads a CommonJS module and returns its exported value.

</details>

<details>
<summary><strong>🟡 Intermediate (6–10)</strong></summary>

6. **Difference between `exports` and `module.exports`?**
   `exports` initially points to `module.exports`. Adding properties (`exports.add = add`) works; reassigning (`exports = add`) does not replace the export.

7. **Can a module export multiple functions?**
   Yes — `module.exports = { add, subtract };`

8. **What is module caching?**
   After a CommonJS module loads once, Node caches it — later `require()` calls reuse the cached module.

9. **What is a circular dependency?**
   When modules directly or indirectly depend on each other, e.g. `A → B → A`.

10. **Difference between `.cjs` and `.mjs`?**
    `.cjs` → always CommonJS. `.mjs` → always ES Module.

</details>

<details>
<summary><strong>🔴 Advanced (11–14)</strong></summary>

11. **Why can circular dependencies cause problems?**
    Modules may be accessed before initialization completes, causing partially initialized exports.

12. **How does Node decide if a `.js` file is CJS or ESM?**
    It checks the nearest `package.json`'s `"type"` field, plus explicit `.cjs`/`.mjs` extensions.

13. **Is CommonJS asynchronous?**
    No — `require()` is synchronous, separate from Node's async I/O model.

14. **Are CommonJS and ESM fully compatible?**
    Not completely — Node provides interoperability, but the two have different loading/export semantics.

</details>

---

## 25. ✍️ Practice Tasks

> Don't just read — **write the code yourself.**

- [ ] **Task 1 — Calculator Module:** Export `add`, `subtract`, `multiply`, `divide` from `calculator.js`, import into `app.js`.
- [ ] **Task 2 — User Module:** Export `createUser()`, `getUser()`, `deleteUser()` from `user.js`.
- [ ] **Task 3 — Logger Module:** Export `info()`, `error()`, `warning()` from `logger.js`.
- [ ] **Task 4 — Convert CJS → ESM:**
  ```js
  const math = require("./math");
  console.log(math.add(10, 20));
  ```
  Rewrite in ES Module syntax.
- [ ] **Task 5 — Mini Backend Structure:** Build `routes/ → controllers/ → services/ → utils/ → server.js`, each layer importing/exporting.

---

## 🧠 Mental Model

```text
             MODULE
                │
       ┌────────┴────────┐
       │                 │
     EXPORT            IMPORT
       │                 │
       ↓                 ↓
module.exports        require()
       │
       └────── OR ──────┐
                        │
                     ESM
                        │
                 export / import
```

```text
Route Module → Controller Module → Service Module → Repository Module → Database Module
```

---

## 🔥 Real-World Example

Instead of a 2,000-line `auth.js`:

```text
auth/
│
├── auth.routes.js
├── auth.controller.js
├── auth.service.js
├── auth.repository.js
├── auth.validation.js
└── auth.utils.js
```

```text
auth.routes.js → auth.controller.js → auth.service.js → auth.repository.js
```

---

## ⚡ Quick Reference

<table>
<tr>
<td valign="top" width="50%">

**CommonJS**
```js
// Export one value
module.exports = function () {};

// Export multiple
module.exports = { add, subtract };

// Import
const math = require("./math");

// Property export
exports.add = add;
```

</td>
<td valign="top" width="50%">

**ES Modules**
```js
// Named export
export function add() {}

// Default export
export default function add() {}

// Named import
import { add } from "./math.js";

// Default import
import add from "./math.js";

// Import everything
import * as math from "./math.js";
```

</td>
</tr>
</table>

---

## 🎯 26. Key Takeaways

After Day 2, you should understand:

✅ What a module is · ✅ Why modules matter · ✅ CommonJS · ✅ `require()` · ✅ `module.exports`
✅ `exports` vs `module.exports` · ✅ ES Modules · ✅ `import`/`export` · ✅ Named vs default exports
✅ `"type": "module"` · ✅ `.js` / `.cjs` / `.mjs` · ✅ Built-in / local / third-party modules
✅ Module resolution · ✅ Caching · ✅ Circular dependencies · ✅ CJS vs ESM · ✅ Backend architecture

---

## 💡 The Most Important Lesson

Don't memorize `require()` and `import` as just syntax. Understand the bigger picture:

```text
Large Application
       ↓
Separate Responsibilities
       ↓
Create Modules
       ↓
Export Functionality
       ↓
Import Where Needed
       ↓
Build Maintainable Architecture
```

That's the real purpose of Node.js modules.

---

## 🧪 Day 2 Challenge

Build a **User Management Module System**:

```text
user-system/
│
├── app.js
├── modules/
│   ├── user.js      → createUser(), getUser(), updateUser(), deleteUser()
│   ├── auth.js       → login(), logout()
│   └── logger.js     → info(), error()
└── utils/
    └── response.js   → successResponse(), errorResponse()
```

Connect everything together through `app.js`. Goal: understand how multiple modules communicate in a real application.

---

## 📌 Day 2 Checklist

- [ ] Understand modules
- [ ] Understand CommonJS
- [ ] Understand `require()`
- [ ] Understand `module.exports`
- [ ] Understand `exports`
- [ ] Understand ESM
- [ ] Understand `import` / `export`
- [ ] Understand named & default exports
- [ ] Understand `.cjs` / `.mjs` / `"type": "module"`
- [ ] Practice local, built-in & third-party modules
- [ ] Complete the module challenge
- [ ] Answer interview questions

---

## 🚀 27. Day 3 Preview

### Node.js File System & File Handling

```text
File System → fs module → Read Files → Write Files → Append Files
    → Delete Files → Directories → Sync vs Async → Promises API
    → Real-world File Handling
```

We'll build practical examples, not just theory.

---

<div align="center">

### ⭐ Learning Philosophy

**Don't just memorize Node.js APIs. Understand what happens when your code runs.**

```text
Learn → Understand → Code → Break → Debug → Build → Repeat
```

**Day 2 — Modules: Complete. 🚀**

---

*Part of the 20 Days • 20 Concepts — Node.js & Express.js Learning Series*

</div>
