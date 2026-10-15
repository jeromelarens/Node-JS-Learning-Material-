<div align="center">

# 🔐 Day 8 — Environment Variables & Configuration

### *Same code. Different environments. Zero secrets leaked.*

![Node.js](https://img.shields.io/badge/Node.js-20%2B-339933?logo=node.js&logoColor=white)
![dotenv](https://img.shields.io/badge/dotenv-.env%20files-ECD53F?logo=dotenv&logoColor=black)
![Security](https://img.shields.io/badge/Focus-Security%20%26%20Config-red)
![Level](https://img.shields.io/badge/Level-Beginner%20Friendly-brightgreen)
![Time](https://img.shields.io/badge/Time-4--5%20hours-blue)

**20 Days • 20 Concepts — Node.js & Express.js Learning Series**

</div>

---

## 🗺️ How to Use This Material

This is a **workbook**, not just a document. For every section:

```text
📖 Read  →  🧪 Type it yourself  →  💥 Break it on purpose  →  ✅ Check the answer
```

| Icon | Meaning |
|---|---|
| 💡 | Key idea, remember this |
| 🧪 | Hands-on lab |
| 🎯 | Task, solve it without copy-pasting |
| 🤔 | Think question, answer before opening the hint |
| ⚠️ | Common trap |
| 🚨 | Security warning |
| 🏆 | Challenge |

> 💡 **Tip:** Keep **one terminal** for running the server and **one editor** open. Change a value, restart, observe. That loop is how configuration really clicks.

---

## 📑 Table of Contents

1. [What You Will Learn](#-1-what-you-will-learn)
2. [Why Configuration Matters (The Story)](#-2-why-configuration-matters-the-story)
3. [What Are Environment Variables?](#-3-what-are-environment-variables)
4. [Lab 1 — Meet process.env](#-4-lab-1--meet-processenv)
5. [Lab 2 — Setting Variables from the Terminal](#-5-lab-2--setting-variables-from-the-terminal)
6. [.env Files & dotenv](#-6-env-files--dotenv)
7. [Lab 3 — Your First .env File](#-7-lab-3--your-first-env-file)
8. [The Golden Rule: Never Commit Secrets](#-8-the-golden-rule-never-commit-secrets)
9. [.env.example — Documenting Config](#-9-envexample--documenting-config)
10. [Everything Is a String!](#-10-everything-is-a-string)
11. [Lab 4 — The String Trap](#-11-lab-4--the-string-trap)
12. [Defaults: `||` vs `??`](#-12-defaults--vs-)
13. [Required vs Optional Variables](#-13-required-vs-optional-variables)
14. [Environments: dev, test, staging, production](#-14-environments-dev-test-staging-production)
15. [Centralized Configuration Module](#-15-centralized-configuration-module)
16. [Lab 5 — Build config/env.js](#-16-lab-5--build-configenvjs)
17. [Fail Fast: Validation at Startup](#-17-fail-fast-validation-at-startup)
18. [Lab 6 — Break Your Config on Purpose](#-18-lab-6--break-your-config-on-purpose)
19. [Validation with Zod](#-19-validation-with-zod)
20. [Lab 7 — Upgrade to Zod](#-20-lab-7--upgrade-to-zod)
21. [Database, Redis, JWT & CORS Config](#-21-database-redis-jwt--cors-config)
22. [Config vs Secrets](#-22-config-vs-secrets)
23. [Production Secret Management](#-23-production-secret-management)
24. [Public vs Private Variables (Next.js)](#-24-public-vs-private-variables-nextjs)
25. [Testing Configuration](#-25-testing-configuration)
26. [🚨 Leaked Secret Emergency Drill](#-26-leaked-secret-emergency-drill)
27. [Spot the Mistakes (Code Review)](#-27-spot-the-mistakes-code-review)
28. [Debugging Challenges](#-28-debugging-challenges)
29. [Quiz Time](#-29-quiz-time)
30. [Mini Project](#-30-mini-project--day-8-capstone)
31. [Interview Questions](#-31-interview-questions)
32. [Cheat Sheet](#-32-cheat-sheet)
33. [Glossary](#-33-glossary)
34. [Progress Tracker](#-34-progress-tracker)
35. [Day 9 Preview](#-35-day-9-preview)

---

## 🎯 1. What You Will Learn

By the end of today you will be able to:

- ✅ Explain what environment variables are and why we use them
- ✅ Read values with `process.env`
- ✅ Use `.env` files with `dotenv`
- ✅ Keep secrets out of Git
- ✅ Convert strings to numbers and booleans safely
- ✅ Validate configuration when the app starts
- ✅ Build a centralized `config/env.js`
- ✅ Use Zod to validate configuration
- ✅ Handle dev, test and production environments
- ✅ Know what to do if a secret leaks

---

## 📖 2. Why Configuration Matters (The Story)

Meet **Arun**, a fresher. He builds an API and writes this:

```js
const PORT = 5000;
const DATABASE_URL = "postgresql://admin:MyRealPassword123@prod-db.com:5432/shop";
const JWT_SECRET = "super-secret";
```

Then he pushes the code to a **public GitHub repo**. 😱

| What happened | Result |
|---|---|
| Password is in the code | Anyone on the internet can read it |
| Bots scan GitHub for secrets 24/7 | Database is attacked within minutes |
| He must change the code to switch DB | Every environment needs a code edit |
| Teammate's local DB is different | Teammate edits the same file → Git conflicts |

### ✅ The fix

```js
const PORT = process.env.PORT;
const DATABASE_URL = process.env.DATABASE_URL;
const JWT_SECRET = process.env.JWT_SECRET;
```

Now the **code is identical** everywhere. Only the **environment** changes.

```mermaid
flowchart LR
    CODE[📄 Same Code] --> DEV[💻 Development<br/>local DB, debug logs]
    CODE --> TEST[🧪 Test<br/>test DB]
    CODE --> STG[🎭 Staging<br/>staging DB]
    CODE --> PROD[🚀 Production<br/>real DB, real secrets]
```

> 💡 **Principle:** *Configuration belongs to the environment, not to the code.*

---

## 🧠 3. What Are Environment Variables?

**Environment variables are key-value settings given to a running program by the place where it runs.**

```text
PORT=5000
NODE_ENV=development
DATABASE_URL=postgresql://localhost:5432/mydb
```

| Name (key) | Value |
|---|---|
| `PORT` | `5000` |
| `NODE_ENV` | `development` |
| `DATABASE_URL` | a connection string |

### 🏠 Analogy: Room settings

Your **code** is like an AC remote. The **environment variables** are the room settings: temperature, mode, timer. The same remote works in any room, and each room has different settings.

### ⚠️ Not automatically secure!

Environment variables are **not magically secret**. They still need:

- Access control (who can see them)
- Redaction in logs
- A proper secret manager in production

---

## 🧪 4. Lab 1 — Meet process.env

`process` is a global object in Node.js. `process.env` holds all environment variables.

Create `day-8-practice/lab1.js`:

```js
console.log("NODE_ENV:", process.env.NODE_ENV);
console.log("PORT:", process.env.PORT);
console.log("HOME or USERPROFILE:", process.env.HOME ?? process.env.USERPROFILE);
console.log("Unknown:", process.env.THIS_DOES_NOT_EXIST);
```

Run it:

```bash
node lab1.js
```

You'll likely see:

```text
NODE_ENV: undefined
PORT: undefined
HOME or USERPROFILE: /Users/yourname
Unknown: undefined
```

### 🤔 Think

*Why is `PORT` undefined even though the app "works fine" on your friend's machine?*

<details>
<summary>💡 Answer</summary>

Your friend has `PORT` set in **their** environment (or `.env` file). Yours is different. This is exactly why we must **never assume** a variable exists.

</details>

### 🎯 Task 1

Print **all** environment variable names on your machine:

```js
console.log(Object.keys(process.env));
```

How many are there? Find any three that look familiar (PATH, USER, etc.).

> 🚨 **Don't paste this output on the internet.** It can contain private information.

---

## 🧪 5. Lab 2 — Setting Variables from the Terminal

Before `.env` files, understand the raw way.

### macOS / Linux (bash/zsh)

```bash
PORT=4000 node lab1.js
```

### Windows PowerShell

```powershell
$env:PORT = "4000"
node lab1.js
```

### Windows CMD

```cmd
set PORT=4000
node lab1.js
```

Now `PORT` should print `4000`.

### Node.js built-in `.env` loading (Node 20.6+)

```bash
node --env-file=.env lab1.js
```

You'll use this in Lab 3.

### 🎯 Task 2

1. Run `lab1.js` with `NODE_ENV=production` set from your terminal
2. Run it again **without** it
3. Observe the difference in output

<details>
<summary>⚠️ Variable disappears after closing the terminal?</summary>

Yes. Variables set in the terminal usually live **only for that terminal session**. That's one reason we use `.env` files for local development.

</details>

---

## 📄 6. .env Files & dotenv

A **`.env` file** is a simple text file that stores local development settings.

```env
PORT=5000
NODE_ENV=development
APP_NAME=Day 8 Learning API
```

### ❗ Important

Creating a `.env` file does **not** automatically load it into Node.js. You need a **loader**:

| Loader | How |
|---|---|
| `dotenv` package | `import "dotenv/config"` |
| Node.js built-in | `node --env-file=.env app.js` (Node 20.6+) |
| Hosting platform | Injects variables directly (no file needed) |

```mermaid
flowchart LR
    A[📄 .env file] --> B[⚙️ dotenv loads it]
    B --> C[process.env]
    C --> D[🧩 Your config module]
    D --> E[🚀 App services]
```

### `.env` syntax rules

```env
# This is a comment
PORT=5000
APP_NAME=My App
GREETING="Hello World"
EMPTY=
```

| ✅ Do | ❌ Don't |
|---|---|
| `PORT=5000` | `PORT = 5000` (spaces may cause problems in some tools) |
| `# comments on own line` | Put real secrets in example files |
| Quote values with spaces | `export PORT=5000` (not needed in `.env`) |

---

## 🧪 7. Lab 3 — Your First .env File

### Step 1 — Setup

```bash
mkdir day-8-config
cd day-8-config
npm init -y
npm install dotenv
```

Add `"type": "module"` to `package.json`.

### Step 2 — Create `.env`

```env
PORT=5000
NODE_ENV=development
APP_NAME=Day 8 Learning API
```

### Step 3 — Create `index.js`

```js
import "dotenv/config";

console.log("App:", process.env.APP_NAME);
console.log("Port:", process.env.PORT);
console.log("Env:", process.env.NODE_ENV);
```

### Step 4 — Run

```bash
node index.js
```

Expected:

```text
App: Day 8 Learning API
Port: 5000
Env: development
```

> dotenv may also print a short tip or log line. That's normal and depends on the version.

### 💥 Break it (do each one, observe, then fix)

| Experiment | What to do | What you'll learn |
|---|---|---|
| 1 | Rename `.env` to `.envv` | Variables become `undefined` |
| 2 | Remove the `import "dotenv/config"` line | `.env` is not read |
| 3 | Run `PORT=9999 node index.js` | Real environment **wins** over `.env` (dotenv doesn't override existing variables by default) |
| 4 | Use `node --env-file=.env index.js` without dotenv import | Built-in loader works |

### 🎯 Task 3

Add **five** new variables to `.env` (`APP_VERSION`, `AUTHOR`, `LOG_LEVEL`, `MAX_USERS`, `DEBUG_MODE`) and print them all.

<details>
<summary>⚠️ Getting `undefined` for everything?</summary>

Check: (1) `.env` is in the **project root** where you run `node`, (2) filename is exactly `.env`, (3) you imported dotenv, (4) there are no typos in variable names (they are case-sensitive!).

</details>

---

## 🚨 8. The Golden Rule: Never Commit Secrets

> **If a secret reaches a public repo, assume it is already stolen.**

### Create `.gitignore`

```gitignore
node_modules/
.env
.env.*
!.env.example
```

| Line | Meaning |
|---|---|
| `.env` | Ignore the real secrets file |
| `.env.*` | Ignore `.env.local`, `.env.production`, etc. |
| `!.env.example` | **Except** the safe example file, which we DO commit |

### ✅ Commit vs ❌ Don't commit

| ✅ Commit | ❌ Never commit |
|---|---|
| `.env.example` | `.env` |
| `.gitignore` | Real passwords, API keys |
| Source code | JWT secrets, private keys |

### 🧪 Verify it works

```bash
git init
git status
```

`.env` must **not** appear in the list. Then check explicitly:

```bash
git check-ignore -v .env
```

If it prints a rule, you're protected. ✅

### ⚠️ Already committed `.env` by mistake?

```bash
git ls-files .env
```

If this prints `.env`, Git is **tracking** it. Adding it to `.gitignore` now is **not enough**. See the [Emergency Drill](#-26-leaked-secret-emergency-drill).

### 🎯 Task 4

1. Create `.gitignore` as above
2. Run `git status` and confirm `.env` is hidden
3. Run `git check-ignore -v .env` and read the output

---

## 📋 9. .env.example — Documenting Config

New teammates ask: *"Which variables do I need?"* The answer is **`.env.example`**.

```env
# ===== App =====
NODE_ENV=development
PORT=5000
APP_NAME=Day 8 Learning API

# ===== Database =====
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE

# ===== Redis =====
REDIS_URL=redis://localhost:6379

# ===== Auth =====
JWT_SECRET=replace-with-a-strong-random-secret

# ===== CORS =====
CORS_ORIGIN=http://localhost:3000

# ===== Logging =====
LOG_LEVEL=debug
```

### Rules for `.env.example`

- ✅ Include **every** variable the app needs
- ✅ Use **placeholders** (`USER:PASSWORD@HOST`)
- ✅ Add comments explaining each group
- ❌ **Never** put real credentials in it

### 🔑 Generate a strong secret

```bash
node -e "console.log(require('node:crypto').randomBytes(32).toString('hex'))"
```

This prints a random 64-character secret. Put it in your **local `.env`**, not in `.env.example`.

### 🎯 Task 5

Create `.env.example` for your project. Then imagine you just cloned this repo. Write the 3-step README instructions to get the app running (hint: *copy example → fill values → run*).

<details>
<summary>✅ Sample onboarding steps</summary>

```bash
cp .env.example .env     # Windows: copy .env.example .env
# open .env and fill in real local values
npm install
npm run dev
```

</details>

---

## 🔤 10. Everything Is a String!

> 💡 **Environment variables are strings. Always.** Even if they look like numbers or booleans.

```env
PORT=5000
ENABLE_LOGGING=false
MAX_RETRIES=3
```

```js
console.log(typeof process.env.PORT);            // "string"
console.log(typeof process.env.ENABLE_LOGGING);  // "string"
console.log(typeof process.env.MAX_RETRIES);     // "string"
```

### 😈 The famous trap

```js
if (process.env.ENABLE_LOGGING) {
  console.log("Logging is ON");
}
```

With `ENABLE_LOGGING=false`, this **still prints "Logging is ON"**, because `"false"` is a non-empty string and non-empty strings are **truthy** in JavaScript.

### Truthy/falsy quick table

| Value | Truthy? |
|---|---|
| `"false"` | ✅ **true** (surprise!) |
| `"0"` | ✅ **true** (surprise!) |
| `""` (empty string) | ❌ false |
| `undefined` | ❌ false |

### ✅ Correct conversions

```js
// Number
const port = Number(process.env.PORT);

// Boolean
const enableLogging = process.env.ENABLE_LOGGING === "true";
```

### ⚠️ Conversion is not validation

```js
Number("hello");  // NaN
Number("");       // 0   ← dangerous! empty string becomes zero
Number(undefined) // NaN
```

So always **validate** after converting:

```js
const port = Number(process.env.PORT);

if (!Number.isInteger(port) || port < 1 || port > 65535) {
  throw new Error("PORT must be a valid TCP port (1-65535)");
}
```

### One more gotcha 🤯

```js
process.env.MY_VAR = undefined;
console.log(process.env.MY_VAR);        // "undefined" (a STRING!)
console.log(typeof process.env.MY_VAR); // "string"
```

Assigning to `process.env` converts values to strings. To remove a variable, use `delete process.env.MY_VAR`.

---

## 🧪 11. Lab 4 — The String Trap

Create `lab4.js`:

```js
process.env.ENABLE_LOGGING = "false";
process.env.PORT = "5000";
process.env.EMPTY = "";

console.log("1.", typeof process.env.PORT);
console.log("2.", process.env.ENABLE_LOGGING ? "ON" : "OFF");
console.log("3.", process.env.ENABLE_LOGGING === "true" ? "ON" : "OFF");
console.log("4.", Number(process.env.PORT) + 1);
console.log("5.", process.env.PORT + 1);
console.log("6.", Number(process.env.EMPTY));
```

### 🤔 Predict first!

Write down your predicted output for all 6 lines **before running**.

<details>
<summary>✅ Actual output & explanation</summary>

```text
1. string
2. ON         ← "false" is truthy!
3. OFF        ← correct boolean check
4. 5001       ← Number("5000") + 1
5. 50001      ← string concatenation: "5000" + 1 = "50001"
6. 0          ← empty string converts to 0
```

</details>

### 🎯 Task 6 — Write a boolean parser

Write a function `parseBoolean(value, defaultValue)` that:

- returns `true` for `"true"` or `"1"`
- returns `false` for `"false"` or `"0"`
- returns `defaultValue` if the value is `undefined`
- **throws an error** for anything else (like `"yes!!"`)

<details>
<summary>✅ Solution</summary>

```js
function parseBoolean(value, defaultValue = false) {
  if (value === undefined) return defaultValue;

  const normalized = value.trim().toLowerCase();

  if (normalized === "true" || normalized === "1") return true;
  if (normalized === "false" || normalized === "0") return false;

  throw new Error(`Invalid boolean value: "${value}"`);
}

console.log(parseBoolean("true"));       // true
console.log(parseBoolean("0"));          // false
console.log(parseBoolean(undefined));    // false (default)
// parseBoolean("maybe");                // throws
```

</details>

---

## 🔀 12. Defaults: `||` vs `??`

Optional settings can have **default values**.

```js
const PORT_A = process.env.PORT || 5000;
const PORT_B = process.env.PORT ?? "5000";
```

They behave **differently**:

| `process.env.PORT` is... | `\|\|` result | `??` result |
|---|---|---|
| `"3000"` | `"3000"` | `"3000"` |
| `undefined` | `5000` | `"5000"` |
| `""` (empty) | `5000` ⚠️ | `""` ⚠️ |

- `||` falls back on **any falsy** value (including empty string)
- `??` falls back **only** on `null` or `undefined`

### ✅ Safe principle

| Situation | What to do |
|---|---|
| **Optional** setting (like `LOG_LEVEL`) | Safe default is fine |
| **Required** setting (like `DATABASE_URL`) | **Never** silently default. Fail with a clear error |
| **Production** | Never silently fall back to `localhost` infrastructure |

### 🤔 Think

*What happens in production if `DATABASE_URL` is missing and the code silently falls back to `localhost`?*

<details>
<summary>💡 Answer</summary>

The app may start successfully but connect to the wrong place, fail mysteriously, or (worse) use an unintended database. A clear startup error is far safer than a silent wrong default.

</details>

---

## ⚖️ 13. Required vs Optional Variables

```mermaid
flowchart TD
    Q{Can the app work safely<br/>without this value?}
    Q -->|No| R[🔴 REQUIRED<br/>Validate at startup, crash if missing]
    Q -->|Yes, with a safe documented behavior| O[🟢 OPTIONAL<br/>Provide a default]
```

### 🎯 Task 7 — Classify these

| Variable | Required or Optional? |
|---|---|
| `DATABASE_URL` | ? |
| `JWT_SECRET` | ? |
| `LOG_LEVEL` | ? |
| `APP_NAME` | ? |
| `PORT` | ? |
| `SENTRY_DSN` (error tracking) | ? |

<details>
<summary>✅ Answers</summary>

| Variable | Answer | Why |
|---|---|---|
| `DATABASE_URL` | Required | App can't store data |
| `JWT_SECRET` | Required | Auth can't be safe without it |
| `LOG_LEVEL` | Optional | Default `info` is safe |
| `APP_NAME` | Optional | Cosmetic |
| `PORT` | Optional | Common default 5000/3000 (but many platforms require using their provided `PORT`) |
| `SENTRY_DSN` | Optional | Monitoring can be disabled; app still works |

</details>

---

## 🌍 14. Environments: dev, test, staging, production

| Setting | 💻 Development | 🧪 Test | 🎭 Staging | 🚀 Production |
|---|---|---|---|---|
| Database | Local DB | **Isolated** test DB | Staging DB | Production DB |
| Redis | Local | Isolated | Staging | Managed |
| Logging | Verbose (debug) | Quiet | Moderate | Controlled, no secrets |
| Secrets | Local fake/dev values | Test-only | Separate from prod | Managed secrets |
| External APIs | Sandbox/mocks | Mocks | Sandbox | Live |

### 🚨 Never

- ❌ Copy production secrets to your laptop
- ❌ Run tests against the production database
- ❌ Reuse the same JWT secret across environments

### NODE_ENV

```env
NODE_ENV=development
```

```js
if (process.env.NODE_ENV === "production") {
  console.log("Production mode");
}
```

⚠️ **Don't overuse it.** Don't hide ten different behaviors behind `NODE_ENV`. For example, whether email is enabled should be its own setting (`EMAIL_ENABLED=true`), not `if (NODE_ENV === "production")`.

Also remember: not every tool sets `NODE_ENV` for you, so **validate it** (accept only known values).

### 🎯 Task 8

Create three files: `.env.development`, `.env.test`, `.env.production.example` (placeholders only). Run:

```bash
node --env-file=.env.development index.js
node --env-file=.env.test index.js
```

Observe how the same code behaves differently.

---

## 🏗️ 15. Centralized Configuration Module

### ❌ The messy way

```js
// auth.js
const secret = process.env.JWT_SECRET;

// db.js
const url = process.env.DATABASE_URL;

// cache.js
const redis = process.env.REDIS_URL;
```

Problems: scattered reads, no validation, typos like `process.env.JWT_SECERT` silently give `undefined`.

### ✅ The professional way

```mermaid
flowchart TD
    ENV[🌍 Environment / .env] --> LOAD[⚙️ config/env.js<br/>load + convert + validate]
    LOAD --> OBJ[🧊 env object frozen]
    OBJ --> A[auth service]
    OBJ --> B[database]
    OBJ --> C[redis]
    OBJ --> D[server.js]
```

```text
src/
├── config/
│   └── env.js        ← ONLY file that reads process.env
├── routes/
├── services/
└── server.js
```

### Benefits

| Benefit | Why |
|---|---|
| 🔍 One place to look | Instantly see all required settings |
| 🛡️ Consistent validation | Bad config caught at startup |
| 🧪 Easy to test | Test one module |
| ✍️ Typo-safe | Typos fail loudly in one place |
| 🧊 Immutable | `Object.freeze` prevents accidental changes |

---

## 🧪 16. Lab 5 — Build config/env.js

### Step 1 — Project structure

```text
day-8-config/
├── src/
│   ├── config/
│   │   └── env.js
│   └── server.js
├── .env
├── .env.example
├── .gitignore
└── package.json
```

Install:

```bash
npm install express dotenv
```

### Step 2 — `.env`

```env
NODE_ENV=development
PORT=5000
APP_NAME=Day 8 Config API
LOG_LEVEL=debug
```

### Step 3 — `src/config/env.js`

```js
import "dotenv/config";

const allowedEnvironments = ["development", "test", "staging", "production"];

function readPort(rawValue = "5000") {
  const port = Number(rawValue);

  if (!Number.isInteger(port) || port < 1 || port > 65535) {
    throw new Error(`Invalid PORT "${rawValue}". It must be 1-65535.`);
  }

  return port;
}

const nodeEnv = process.env.NODE_ENV ?? "development";

if (!allowedEnvironments.includes(nodeEnv)) {
  throw new Error(
    `Invalid NODE_ENV "${nodeEnv}". Allowed: ${allowedEnvironments.join(", ")}`
  );
}

export const env = Object.freeze({
  NODE_ENV: nodeEnv,
  PORT: readPort(process.env.PORT),
  APP_NAME: process.env.APP_NAME ?? "Node.js API",
  LOG_LEVEL: process.env.LOG_LEVEL ?? "info"
});
```

### Step 4 — `src/server.js`

```js
import express from "express";
import { env } from "./config/env.js";

const app = express();

app.get("/", (req, res) => {
  res.json({
    success: true,
    application: env.APP_NAME,
    environment: env.NODE_ENV
  });
});

app.listen(env.PORT, () => {
  console.log(`${env.APP_NAME} listening on port ${env.PORT}`);
});
```

### Step 5 — Scripts in `package.json`

```json
{
  "type": "module",
  "scripts": {
    "dev": "node --watch src/server.js",
    "start": "node src/server.js"
  }
}
```

### Step 6 — Run

```bash
npm run dev
```

Open `http://localhost:5000`.

### 🎯 Task 9

1. Change `PORT=4000` in `.env`. Does `--watch` restart pick it up? 
2. Change `APP_NAME`. Reload the browser. Did the response change?

<details>
<summary>💡 Observation</summary>

`node --watch` restarts when **watched source files** change. Depending on your Node version, changes to `.env` may **not** trigger a restart. If nothing changes, stop (`Ctrl+C`) and run `npm run dev` again. Environment variables are read **once at startup**. They are not live-reloaded.

</details>

### ⚠️ Import order matters (ESM trap!)

`import "dotenv/config"` must run **before** any module that reads `process.env`. That's why we put it **inside** `env.js`, so any file importing `env.js` is guaranteed to have loaded `.env` first.

```js
// ❌ Risky: another module reads process.env before dotenv loads
import { connectDb } from "./db.js";
import "dotenv/config";

// ✅ Safe: only read configuration through env.js
import { env } from "./config/env.js";
```

---

## ⚡ 17. Fail Fast: Validation at Startup

> 💡 **Fail fast** = if configuration is wrong, **crash immediately with a clear message** instead of failing mysteriously later.

```mermaid
flowchart LR
    S[🚀 App starts] --> V{Config valid?}
    V -->|Yes| OK[✅ Start server]
    V -->|No| ERR[❌ Print clear error<br/>exit immediately]
```

### Good vs bad failure

| ❌ Bad (late, confusing) | ✅ Good (early, clear) |
|---|---|
| `TypeError: Cannot read properties of undefined` at 3 AM during a user request | `Missing required environment variable: DATABASE_URL` at startup |
| `Invalid signature` errors for random users | `JWT_SECRET must be at least 32 characters` |

### A reusable helper

```js
function required(name) {
  const value = process.env[name];

  if (value === undefined || value.trim() === "") {
    throw new Error(`Missing required environment variable: ${name}`);
  }

  return value;
}
```

> 🚨 The error message prints the **variable name**, never its **value**.

---

## 🧪 18. Lab 6 — Break Your Config on Purpose

Add a required variable to `env.js`:

```js
export const env = Object.freeze({
  NODE_ENV: nodeEnv,
  PORT: readPort(process.env.PORT),
  APP_NAME: process.env.APP_NAME ?? "Node.js API",
  LOG_LEVEL: process.env.LOG_LEVEL ?? "info",
  DATABASE_URL: required("DATABASE_URL"),
  JWT_SECRET: required("JWT_SECRET")
});
```

(Don't forget to paste the `required()` helper above it.)

### 💥 Experiments

| # | What to try | Expected result |
|---|---|---|
| 1 | Start without `DATABASE_URL` in `.env` | Clear "Missing required environment variable" error |
| 2 | Set `PORT=hello` | "Invalid PORT" error |
| 3 | Set `PORT=99999` | "Invalid PORT" error (out of range) |
| 4 | Set `PORT=` (empty) | Error. Note: with `??`, empty string is **not** replaced by the default, and `Number("")` is `0`, which fails the range check |
| 5 | Set `NODE_ENV=prod` | "Invalid NODE_ENV" (only `production` is allowed) |
| 6 | Set `JWT_SECRET=   ` (spaces only) | Treated as missing thanks to `.trim()` |

### 🎯 Task 10

Add these validations to `env.js`:

1. `JWT_SECRET` must be at least **32 characters**
2. `LOG_LEVEL` must be one of `debug`, `info`, `warn`, `error`
3. In `production`, `JWT_SECRET` must **not** equal `replace-with-a-strong-random-secret`

<details>
<summary>✅ Solution</summary>

```js
const jwtSecret = required("JWT_SECRET");

if (jwtSecret.length < 32) {
  throw new Error("JWT_SECRET must be at least 32 characters");
}

if (
  nodeEnv === "production" &&
  jwtSecret === "replace-with-a-strong-random-secret"
) {
  throw new Error("JWT_SECRET must not use the example placeholder in production");
}

const allowedLogLevels = ["debug", "info", "warn", "error"];
const logLevel = process.env.LOG_LEVEL ?? "info";

if (!allowedLogLevels.includes(logLevel)) {
  throw new Error(`LOG_LEVEL must be one of: ${allowedLogLevels.join(", ")}`);
}
```

</details>

---

## 🛡️ 19. Validation with Zod

Writing every check by hand gets long. **Zod** is a schema validation library that does it declaratively.

```bash
npm install zod
```

```js
import "dotenv/config";
import { z } from "zod";

const envSchema = z.object({
  NODE_ENV: z
    .enum(["development", "test", "staging", "production"])
    .default("development"),

  PORT: z.coerce.number().int().min(1).max(65535).default(5000),

  APP_NAME: z.string().min(1).default("Node.js API"),

  DATABASE_URL: z.string().min(1),

  JWT_SECRET: z.string().min(32),

  CORS_ORIGIN: z.string().url(),

  LOG_LEVEL: z.enum(["debug", "info", "warn", "error"]).default("info")
});

const result = envSchema.safeParse(process.env);

if (!result.success) {
  console.error(
    "❌ Invalid environment configuration:",
    result.error.issues.map(({ path, message }) => ({
      variable: path.join("."),
      message
    }))
  );
  process.exit(1);
}

export const env = Object.freeze(result.data);
```

### What each part does

| Zod piece | Meaning |
|---|---|
| `z.object({...})` | Describes the shape of your config |
| `z.enum([...])` | Only these values allowed |
| `z.coerce.number()` | Convert string → number, then validate |
| `.int().min().max()` | Extra number rules |
| `.default(5000)` | Used when the value is missing |
| `z.string().url()` | Must look like a valid URL |
| `safeParse` | Returns success/failure **without throwing** |

### ⚠️ Zod boolean trap

```js
z.coerce.boolean().parse("false"); // true 😱
```

`z.coerce.boolean()` uses JavaScript truthiness, so `"false"` becomes `true`. For environment booleans use an explicit transform:

```js
const booleanFromEnv = z
  .enum(["true", "false"])
  .transform((value) => value === "true");

// Usage in schema:
// ENABLE_LOGGING: booleanFromEnv.default("false")
```

### 🚨 Don't print the whole config

When debugging, **never** do `console.log(process.env)` or `console.log(env)` in production. It can leak secrets into logs.

---

## 🧪 20. Lab 7 — Upgrade to Zod

### Step 1

Replace your hand-written `env.js` with the Zod version above.

### Step 2 — Create a complete `.env`

```env
NODE_ENV=development
PORT=5000
APP_NAME=Day 8 Zod API
DATABASE_URL=postgresql://user:password@localhost:5432/devdb
JWT_SECRET=paste-a-64-char-random-hex-string-generated-with-node-crypto-here
CORS_ORIGIN=http://localhost:3000
LOG_LEVEL=debug
```

(Replace `JWT_SECRET` with a real generated value from Section 9.)

### Step 3 — Run and then break

| # | Break it by... | Expected error |
|---|---|---|
| 1 | Delete `DATABASE_URL` | variable `DATABASE_URL` is required |
| 2 | `JWT_SECRET=short` | Must be at least 32 characters |
| 3 | `CORS_ORIGIN=localhost3000` | Invalid URL |
| 4 | `PORT=abc` | Expected number |
| 5 | `NODE_ENV=prod` | Invalid enum value |

### 🎯 Task 11

Add `ENABLE_REQUEST_LOGS` as a boolean setting using the explicit transform. Test it with `true`, `false`, and `yes`. Which one fails?

<details>
<summary>✅ Answer</summary>

`yes` fails, because the enum accepts only `"true"` or `"false"`. That strictness is the point: bad values are caught instead of silently guessed.

</details>

---

## 🧩 21. Database, Redis, JWT & CORS Config

### 🗄️ Database

```env
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE
```

```text
Environment → Validation → Database client → Services
```

- ✅ Read the URL from validated config
- ❌ Never hardcode it
- 🚨 Never log the full URL (it contains the password)

Anatomy of a connection string:

```text
postgresql://  USER : PASSWORD  @  HOST  : 5432 / DATABASE
   │            │       │          │       │        │
 protocol    username  secret   server   port   db name
```

### ⚡ Redis

```env
REDIS_URL=redis://localhost:6379
```

- Managed Redis providers often need **TLS** and authentication (often `rediss://`). Follow your provider's docs.
- Don't remove TLS settings just to make a local test pass in production.
- Use timeouts, handle connection failures, close connections on shutdown.

### 🔑 JWT secret

```js
// ❌ Never
const JWT_SECRET = "super-secret";

// ✅ Always
import { env } from "./config/env.js";
const secret = env.JWT_SECRET;
```

| Rule | Why |
|---|---|
| Generate with a cryptographic random generator | Guessable secrets get cracked |
| Different secret per environment | A dev leak shouldn't compromise production |
| Never send it to the browser | It signs/validates your tokens |
| Rotate if exposed | Old secret = attacker can forge tokens |

> 💡 You'll learn JWT properly on Day 16. Today you only learn how to **store and load** the secret safely.

### 🌐 CORS

```env
CORS_ORIGIN=http://localhost:3000
```

CORS decides which **browser origins** may call your API.

| ✅ Good | ❌ Risky |
|---|---|
| Specific allowed origin(s) from config | `origin: "*"` on APIs that need restrictions or use credentials |

⚠️ CORS is **not** authentication. It does not protect your API from non-browser clients like curl or Postman.

### 🎯 Task 12

Add `REDIS_URL` (optional) and `CORS_ORIGIN` (required) to your schema. Then write a function `maskDatabaseUrl(url)` that hides the password for safe logging.

<details>
<summary>✅ Solution</summary>

```js
export function maskDatabaseUrl(rawUrl) {
  try {
    const url = new URL(rawUrl);
    if (url.password) url.password = "*****";
    return url.toString();
  } catch {
    return "[invalid URL]";
  }
}

console.log(maskDatabaseUrl("postgresql://admin:secret123@localhost:5432/mydb"));
// postgresql://admin:*****@localhost:5432/mydb
```

</details>

---

## 🔍 22. Config vs Secrets

| | ⚙️ Configuration | 🔒 Secrets |
|---|---|---|
| Purpose | Control behavior | Prove identity / grant access |
| Examples | `PORT`, `LOG_LEVEL`, `APP_NAME` | `JWT_SECRET`, API keys, DB password |
| Safe to show? | Usually | **Never** |
| Needs protection? | Low | **High** |

### 🎯 Task 13 — Sort them

Mark each as **Config (C)** or **Secret (S)**:

| Value | C or S? |
|---|---|
| `PORT=5000` | ? |
| `STRIPE_SECRET_KEY=sk_live_...` | ? |
| `LOG_LEVEL=debug` | ? |
| `DATABASE_URL=postgres://user:pass@host/db` | ? |
| `CORS_ORIGIN=https://myapp.com` | ? |
| `SMTP_PASSWORD=...` | ? |

<details>
<summary>✅ Answers</summary>

`PORT` C, `STRIPE_SECRET_KEY` S, `LOG_LEVEL` C, `DATABASE_URL` **S** (contains a password), `CORS_ORIGIN` C, `SMTP_PASSWORD` S.

</details>

---

## 🏭 23. Production Secret Management

| Environment | Where config usually lives |
|---|---|
| 💻 Local dev | Git-ignored `.env` file |
| 🤖 CI/CD | Protected CI variables/secrets |
| 🐳 Containers | Orchestrator/platform secrets |
| ☁️ Cloud hosting | Platform environment settings or a cloud secret manager |

> 💡 In production, **don't rely on a `.env` file** sitting on the server. Inject variables through the platform.

### 🛡️ Principles

1. **Least privilege**: only services that need a secret can access it
2. **Never log** secret values
3. **Rotate** after any suspected exposure
4. **Separate** dev and prod credentials
5. **Keep secrets out of client bundles**
6. **Audit** who can read secrets

### Docker reminder

Don't bake secrets into an image:

```dockerfile
# ❌ BAD: secret is stored forever in the image layers
ENV JWT_SECRET=my-real-secret

# ✅ GOOD: provide at runtime
# docker run -e JWT_SECRET=... my-image
# (or use platform/orchestrator secrets)
```

---

## 🌐 24. Public vs Private Variables (Next.js)

In full-stack frameworks, some variables reach the **browser**.

In Next.js, variables prefixed with `NEXT_PUBLIC_` can be exposed to client-side code.

```env
NEXT_PUBLIC_API_URL=https://api.example.com   # public, OK
JWT_SECRET=...                                # private, server only
DATABASE_URL=...                              # private, server only
```

| ✅ Safe as public | 🚨 Never public |
|---|---|
| API base URL | `JWT_SECRET` |
| Public analytics ID | `DATABASE_URL` |
| Feature flag labels | Private API keys |

> 💡 **A variable name doesn't make it secret. Where it's used and exposed decides the risk.**

---

## 🧪 25. Testing Configuration

Configuration is **behavior**, so test it!

### What to test

- ✅ Valid development config passes
- ❌ Missing `DATABASE_URL` fails
- ❌ Invalid `PORT` fails
- ❌ Unsupported `NODE_ENV` fails
- ❌ Malformed URL fails
- ✅ Optional values fall back to defaults
- ✅ Error messages don't contain secret values

### Make it testable: extract a pure function

Create `src/config/parseEnv.js`:

```js
import { z } from "zod";

const envSchema = z.object({
  NODE_ENV: z.enum(["development", "test", "staging", "production"]).default("development"),
  PORT: z.coerce.number().int().min(1).max(65535).default(5000),
  DATABASE_URL: z.string().min(1),
  JWT_SECRET: z.string().min(32)
});

export function parseEnv(source) {
  return envSchema.safeParse(source);
}
```

Then `src/config/env.js` just loads dotenv, calls `parseEnv(process.env)`, and exits on failure.

### Tests with Node's built-in runner

`tests/env.test.js`:

```js
import test from "node:test";
import assert from "node:assert";
import { parseEnv } from "../src/config/parseEnv.js";

const validEnv = {
  NODE_ENV: "test",
  PORT: "5000",
  DATABASE_URL: "postgresql://user:pass@localhost:5432/testdb",
  JWT_SECRET: "a".repeat(32)
};

test("accepts valid configuration", () => {
  const result = parseEnv(validEnv);
  assert.strictEqual(result.success, true);
  assert.strictEqual(result.data.PORT, 5000); // converted to number
});

test("rejects missing DATABASE_URL", () => {
  const { DATABASE_URL, ...withoutDb } = validEnv;
  const result = parseEnv(withoutDb);
  assert.strictEqual(result.success, false);
});

test("rejects invalid port", () => {
  const result = parseEnv({ ...validEnv, PORT: "99999" });
  assert.strictEqual(result.success, false);
});

test("rejects unsupported NODE_ENV", () => {
  const result = parseEnv({ ...validEnv, NODE_ENV: "prod" });
  assert.strictEqual(result.success, false);
});
```

Run:

```bash
node --test
```

> 💡 Notice we pass a **plain object** to `parseEnv`, so we **never mutate `process.env`** in tests. This avoids tests affecting each other.

### 🎯 Task 14

Add two more tests:

1. `JWT_SECRET` shorter than 32 characters fails
2. When `PORT` is missing, the default `5000` is used

---

## 🚨 26. Leaked Secret Emergency Drill

**Scenario:** You realize you pushed `.env` with a real database password to GitHub 10 minutes ago.

```mermaid
flowchart TD
    A[😱 Secret leaked] --> B[1️⃣ ROTATE the credential NOW<br/>change DB password / regenerate API key]
    B --> C[2️⃣ Update deployed apps with new secret]
    C --> D[3️⃣ Check logs for suspicious access]
    D --> E[4️⃣ Remove file from Git tracking + fix .gitignore]
    E --> F[5️⃣ Clean history if needed<br/>using proper tools]
    F --> G[6️⃣ Write what happened, add safeguards]
```

### ⚠️ The most common wrong answer

> "I'll just delete the file and push again."

**That's not enough.** Git history still contains the old version, and the secret may already be copied or scanned. **Rotating the credential is the real fix.**

### Stop tracking (but keep the local file)

```bash
git rm --cached .env
echo ".env" >> .gitignore
git commit -m "Stop tracking .env"
```

### Prevention

- Use a pre-commit secret scanner (like gitleaks) or your Git platform's secret scanning
- Review `git status` before every commit
- Keep `.env.example` clean

### 🤔 Think

*Why is rotating the secret more important than deleting the commit?*

<details>
<summary>💡 Answer</summary>

Because you can never be sure who already saw or copied the secret. A rotated credential makes the leaked one useless. Deleting history only hides it going forward.

</details>

---

## 🕵️ 27. Spot the Mistakes (Code Review)

Find **all** problems in this code. There are **10**.

```js
// config.js
import dotenv from "dotenv";
dotenv.config();

export const PORT = process.env.PORT || 5000;
export const DB = process.env.DATABASE_URL || "postgresql://admin:admin123@localhost/prod";
export const SECRET = process.env.JWT_SECRET || "secret";
export const LOGGING = process.env.ENABLE_LOGGING;

console.log("Config loaded:", process.env);

if (LOGGING) {
  console.log("Logging enabled");
}

if (process.env.NODE_ENV = "production") {
  console.log("Running in production");
}
```

<details>
<summary>✅ The 10 problems</summary>

1. `PORT` is not converted to a number or validated (and `||` swallows an empty string)
2. `PORT` falls back silently instead of validating
3. Hardcoded **database credentials** in the fallback
4. The fallback URL even looks like **production**
5. `DATABASE_URL` is required but has a silent default
6. `JWT_SECRET` falls back to a weak, guessable `"secret"`
7. `LOGGING` is a raw string, so `"false"` would be truthy
8. `console.log(process.env)` **leaks all secrets** into logs
9. `NODE_ENV = "production"` uses **assignment (`=`)** instead of comparison (`===`). It overwrites the variable and is always truthy!
10. `NODE_ENV` is never validated against allowed values

</details>

---

## 🐞 28. Debugging Challenges

Try each before opening the answer.

### 🐞 Bug 1

```text
Server running on port undefined
```

<details>
<summary>🔍 Cause & fix</summary>

`process.env.PORT` is not set and no default exists, or `.env` isn't loaded. Check `.env` location, dotenv import, and variable spelling. Prefer a validated config module.

</details>

### 🐞 Bug 2

```text
Error: Missing required environment variable: DATABASE_URL
```

(but you **do** have it in `.env`)

<details>
<summary>🔍 Likely causes</summary>

- `.env` is in the wrong folder (not where you run `node`)
- Typo: `DATABASE_URI` vs `DATABASE_URL`
- Spaces around `=`
- dotenv is imported **after** the module that reads the variable
- File is named `.env.txt` (hidden extensions on Windows!)

</details>

### 🐞 Bug 3

You set `ENABLE_LOGGING=false` but logs still print.

<details>
<summary>🔍 Cause & fix</summary>

`"false"` is a truthy string. Compare explicitly: `process.env.ENABLE_LOGGING === "true"`, or use a boolean parser.

</details>

### 🐞 Bug 4

The server listens on port `50001` instead of `5000`.

<details>
<summary>🔍 Cause</summary>

String concatenation: `process.env.PORT + 1` → `"50001"`. Convert first: `Number(process.env.PORT) + 1`.

</details>

### 🐞 Bug 5

It works locally, but in production: `JWT_SECRET is required`.

<details>
<summary>🔍 Cause & fix</summary>

`.env` is git-ignored, so it was never deployed (correct behavior!). Set the variable in your **hosting platform's** environment/secret settings.

</details>

### 🐞 Bug 6

You changed `.env` and saved, but the app still uses the old value.

<details>
<summary>🔍 Cause & fix</summary>

Environment variables are read at **startup**. Restart the process. Also check whether a real environment variable with the same name is set in your terminal. By default dotenv does **not** override existing environment variables.

</details>

### 🐞 Bug 7

```text
SyntaxError: Cannot use import statement outside a module
```

<details>
<summary>🔍 Fix</summary>

Add `"type": "module"` in `package.json`, or use `require("dotenv").config()` in CommonJS.

</details>

### 🐞 Bug 8 🔥

Your teammate says: *"I added `.env` to `.gitignore` but `git status` still shows it modified."*

<details>
<summary>🔍 Fix</summary>

The file was **already tracked** before it was ignored. `.gitignore` only affects untracked files. Run `git rm --cached .env`, commit, and **rotate any real secrets** that were committed.

</details>

---

## 📝 29. Quiz Time

Answer on paper first.

**Q1.** What is `process.env`?

**Q2.** What type are environment variable values?
- A) number  B) boolean  C) string  D) object

**Q3.** With `FLAG=false`, what does `if (process.env.FLAG)` do?
- A) Skips the block  B) Runs the block  C) Throws an error

**Q4.** `Number("")` returns:
- A) NaN  B) 0  C) undefined  D) an error

**Q5.** Which commits to Git?
- A) `.env`  B) `.env.example`  C) both

**Q6.** What does `??` do differently from `||`?

**Q7.** Why centralize config in one module?

**Q8.** What does "fail fast" mean?

**Q9.** If a secret was pushed to GitHub, the most important first step is:
- A) Delete the file  B) Rotate the secret  C) Ignore it  D) Rename the repo

**Q10.** Is `DATABASE_URL=postgres://user:pass@host/db` config or a secret?

**Q11.** Why is `process.env.NODE_ENV = "production"` inside an `if` condition a bug?

**Q12.** Why should you never log `process.env` in production?

<details>
<summary>✅ Answers</summary>

1. The object exposing the process environment variables to Node.js
2. **C**: string
3. **B**: runs the block (`"false"` is truthy)
4. **B**: `0`
5. **B**: `.env.example`
6. `??` falls back only for `null`/`undefined`. `||` falls back for any falsy value, including `""` and `0`
7. Consistent validation, one place to look, easier testing, fewer typos
8. Crash early with a clear error when config is invalid, instead of failing later in confusing ways
9. **B**: rotate the secret
10. **Secret** (it contains a password)
11. `=` assigns instead of comparing. It overwrites the value and is always truthy. Use `===`
12. It can expose secrets (DB passwords, API keys, tokens) in log files that many people/tools can read

</details>

### 🏅 Scoring

| Score | Level |
|---|---|
| 11-12 | 🏆 Config Master |
| 8-10 | 🥈 Almost there |
| 5-7 | 🥉 Redo Labs 4-6 |
| 0-4 | 📚 Re-read and redo the labs |

---

## 🚀 30. Mini Project — Day 8 Capstone

### 📋 Goal

Build a **production-style configuration layer** for an Express API.

### Final structure

```text
day-8-project/
├── src/
│   ├── config/
│   │   ├── parseEnv.js
│   │   └── env.js
│   ├── routes/
│   │   └── health.js
│   └── server.js
├── tests/
│   └── env.test.js
├── .env
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

### ✅ Checklist

#### Part A: Setup

- [ ] `npm init -y`, set `"type": "module"`
- [ ] Install `express`, `dotenv`, `zod`
- [ ] Create `.gitignore` (`.env`, `.env.*`, `!.env.example`, `node_modules/`)
- [ ] Run `git check-ignore -v .env` and confirm it's ignored

#### Part B: Configuration

- [ ] `.env.example` documents **every** variable with placeholders and comments
- [ ] `.env` has working local values (real generated `JWT_SECRET`)
- [ ] `parseEnv.js` validates with Zod:
  - [ ] `NODE_ENV` (enum)
  - [ ] `PORT` (integer 1-65535, default 5000)
  - [ ] `APP_NAME` (default)
  - [ ] `DATABASE_URL` (required)
  - [ ] `JWT_SECRET` (required, min 32 chars)
  - [ ] `CORS_ORIGIN` (valid URL)
  - [ ] `LOG_LEVEL` (enum, default `info`)
  - [ ] `ENABLE_REQUEST_LOGS` (explicit boolean)
- [ ] `env.js` loads dotenv, calls `parseEnv`, prints a clear error and exits on failure
- [ ] `env` is exported with `Object.freeze`

#### Part C: API

- [ ] `GET /` returns app name and environment
- [ ] `GET /health` returns `{ "status": "ok", "environment": "..." }`
- [ ] **No** route returns secret values
- [ ] Server listens on `env.PORT`
- [ ] Only `config/` reads `process.env`. Search your project for `process.env` to verify!

#### Part D: Safe logging

- [ ] On startup log: app name, environment, port
- [ ] Add `maskDatabaseUrl()` and log the **masked** DB URL only
- [ ] Confirm secrets never appear in the console

#### Part E: Tests

- [ ] Valid config passes
- [ ] Missing `DATABASE_URL` fails
- [ ] Invalid port fails
- [ ] Unsupported `NODE_ENV` fails
- [ ] Short `JWT_SECRET` fails
- [ ] Default port is used when `PORT` is missing
- [ ] `npm test` passes

#### Part F: Documentation

- [ ] Write a `README.md` covering: setup steps, a variable table (name, required?, example, description), how to run, how to test

### 🎁 Variable documentation table (template)

| Variable | Required | Example | Description |
|---|---|---|---|
| `NODE_ENV` | No | `development` | App environment |
| `PORT` | No | `5000` | Server port |
| `DATABASE_URL` | **Yes** | `postgresql://USER:PASSWORD@HOST:5432/DB` | Database connection |
| `JWT_SECRET` | **Yes** | *(generate)* | Token signing secret, min 32 chars |
| `CORS_ORIGIN` | **Yes** | `http://localhost:3000` | Allowed frontend origin |
| `LOG_LEVEL` | No | `info` | Logging verbosity |

### 🏆 Bonus Challenges

| Level | Challenge |
|---|---|
| 🥉 Easy | Add `.env.test` and run the app with `node --env-file=.env.test` |
| 🥈 Medium | In production, reject `CORS_ORIGIN` values that start with `http://localhost` |
| 🥇 Hard | Add a `npm run config:check` script that validates config and exits without starting the server |
| 💎 Expert | Write a Dockerfile that reads all config at runtime (no secrets baked into the image) |

<details>
<summary>💡 Hint for the Hard bonus</summary>

Create `scripts/check-config.js` that just imports `../src/config/env.js` and prints `✅ Configuration is valid`. If validation fails, `env.js` already exits with an error. Add `"config:check": "node scripts/check-config.js"` to `package.json`.

</details>

### ✅ Self-evaluation rubric

| Criteria | Done? |
|---|---|
| `.env` is git-ignored and **not** tracked | ☐ |
| `.env.example` complete with placeholders | ☐ |
| Only `env.js` reads `process.env` | ☐ |
| Invalid config stops startup with a clear message | ☐ |
| Types converted (port number, booleans) | ☐ |
| No secrets in logs or API responses | ☐ |
| Config tests pass | ☐ |
| README documents all variables | ☐ |

---

## 💼 31. Interview Questions

### 🟢 Beginner

<details>
<summary><b>1. What are environment variables?</b></summary>

Key-value configuration values supplied to a running process by its environment.

</details>

<details>
<summary><b>2. What is process.env?</b></summary>

A Node.js object for reading the process's environment variables.

</details>

<details>
<summary><b>3. What is a .env file?</b></summary>

A local file for development settings, loaded by a tool like dotenv or Node's `--env-file` option.

</details>

<details>
<summary><b>4. Why not commit secrets to Git?</b></summary>

Anyone with repo access (or access to its history) can read them. Exposed secrets must be rotated.

</details>

<details>
<summary><b>5. What is dotenv?</b></summary>

A package that loads variables from a `.env` file into `process.env`.

</details>

### 🟡 Intermediate

<details>
<summary><b>6. Why are environment variables strings, and why does it matter?</b></summary>

The OS-level environment stores text. You must convert and validate numbers/booleans yourself. For example `"false"` is truthy in JavaScript.

</details>

<details>
<summary><b>7. Why centralize configuration?</b></summary>

One place for loading, converting and validating, which reduces duplication, typos and hidden dependencies, and makes testing easier.

</details>

<details>
<summary><b>8. Required vs optional configuration?</b></summary>

Required: the app can't operate safely without it. Optional: the app has a safe, documented fallback or can disable the feature.

</details>

<details>
<summary><b>9. Why validate at startup?</b></summary>

To catch bad deployments before the app handles traffic, with clear errors instead of runtime surprises.

</details>

<details>
<summary><b>10. `||` vs `??` for defaults?</b></summary>

`||` replaces any falsy value (including `""`/`0`); `??` replaces only `null`/`undefined`. Neither validates the value.

</details>

### 🔴 Advanced

<details>
<summary><b>11. Why use a secret manager in production?</b></summary>

For access control, auditing, rotation and reduced accidental exposure, which plain files on a server don't provide.

</details>

<details>
<summary><b>12. How do you test config code?</b></summary>

Extract a pure `parseEnv(source)` function, pass plain objects, and avoid mutating the global `process.env` across tests.

</details>

<details>
<summary><b>13. Config vs secrets?</b></summary>

Config controls behavior (port, log level). Secrets grant access or prove identity (passwords, keys, tokens) and need stricter handling.

</details>

<details>
<summary><b>14. What does `NEXT_PUBLIC_` do in Next.js?</b></summary>

Marks a variable as exposed to browser code, so it must never hold secrets.

</details>

<details>
<summary><b>15. Production DB URL missing, should the app default to localhost?</b></summary>

No. It should fail fast with a clear error because a silent wrong default is dangerous.

</details>

<details>
<summary><b>16. A secret was committed. Walk through your response.</b></summary>

Rotate the credential immediately, update deployments, review access logs, stop tracking the file and fix `.gitignore`, clean history if appropriate, and add prevention such as secret scanning.

</details>

---

## 🧾 32. Cheat Sheet

| 🎯 I want to... | 💻 How |
|---|---|
| Read a variable | `process.env.NAME` |
| Load `.env` (dotenv, ESM) | `import "dotenv/config"` |
| Load `.env` (dotenv, CommonJS) | `require("dotenv").config()` |
| Load `.env` (built-in, Node 20.6+) | `node --env-file=.env app.js` |
| Set a variable for one run (mac/Linux) | `PORT=4000 node app.js` |
| Set a variable (PowerShell) | `$env:PORT="4000"; node app.js` |
| Convert to number | `Number(value)` + validate |
| Convert to boolean | `value === "true"` or a strict parser |
| Optional default | `value ?? "default"` |
| Generate a secret | `node -e "console.log(require('node:crypto').randomBytes(32).toString('hex'))"` |
| Check if `.env` is ignored | `git check-ignore -v .env` |
| Is `.env` tracked? | `git ls-files .env` |
| Stop tracking `.env` | `git rm --cached .env` |
| Validate with Zod | `schema.safeParse(process.env)` |
| Remove a variable | `delete process.env.NAME` |
| Run tests | `node --test` |

### 🧊 Mini checklist before every commit

```text
□ git status shows no .env
□ No secrets in code or logs
□ .env.example is up to date
□ Required variables documented
```

---

## 📖 33. Glossary

| Term | Simple meaning |
|---|---|
| **Environment variable** | A named setting given to a running program |
| **process.env** | Node.js object holding environment variables |
| **.env file** | Local file of settings for development |
| **dotenv** | Package that loads `.env` into `process.env` |
| **Secret** | Sensitive value (password, key, token) |
| **Configuration** | Settings that control behavior |
| **Fail fast** | Crash early with a clear error |
| **Validation** | Checking that values are correct |
| **Coercion** | Converting a value's type (string → number) |
| **Schema** | A description of what valid data looks like |
| **Truthy / Falsy** | How JavaScript treats values in conditions |
| **Rotation** | Replacing a secret with a new one |
| **Secret manager** | Service that stores and controls access to secrets |
| **Staging** | Pre-production environment for final checks |
| **CORS** | Browser rule for which origins may call your API |

---

## ✅ 34. Progress Tracker

### Knowledge

- [ ] I can explain environment variables to a beginner
- [ ] I know `process.env` returns strings
- [ ] I can use dotenv and `--env-file`
- [ ] I understand why `.env` is never committed
- [ ] I know why `"false"` is truthy
- [ ] I can use `??` vs `||` correctly
- [ ] I can separate required and optional config
- [ ] I can build a centralized config module
- [ ] I can validate with Zod
- [ ] I know the steps when a secret leaks

### Practice

- [ ] Completed Labs 1-7
- [ ] Completed Tasks 1-14
- [ ] Did the Code Review and Debugging challenges
- [ ] Scored 8+ on the quiz
- [ ] Finished the Mini Project

### 🎖️ Badges

| Badge | Earned when |
|---|---|
| 🥉 **Config Starter** | Labs 1-3 done |
| 🥈 **String Slayer** | Lab 4 and Task 6 done |
| 🥇 **Validator** | Labs 5-7 done |
| 🛡️ **Secret Keeper** | Emergency drill and code review done |
| 🏆 **Config Master** | Mini project and quiz done |

---

## 🔮 35. Day 9 Preview

### 🌐 Creating HTTP Servers with Node.js

Next time you'll learn:

- The built-in `node:http` module
- Request and response objects
- HTTP methods and status codes
- Headers and content types
- URL and query-string parsing
- Reading request bodies
- Basic routing and error handling
- Graceful shutdown
- Node.js HTTP vs Express.js

---

## ⭐ Series Progress

```text
Day 0  → Getting Started & Roadmap             ✅
Day 1  → Node.js Runtime & Event Loop          ✅
Day 2  → CommonJS vs ES Modules                ✅
Day 3  → File System & File Handling           ✅
Day 4  → EventEmitter                          ✅
Day 5  → Streams & Buffers                     ✅
Day 6  → Callbacks, Promises & Async/Await     ✅
Day 7  → NPM & Package Management              ✅
Day 8  → Environment Variables & Configuration ✅ ← You are here
Day 9  → Creating HTTP Servers with Node.js    🔜
...
Day 20 → Production-Ready Express Architecture 🔜
```

---

<div align="center">

## 🧠 Final Mental Model

```text
Environment / .env
        ↓
Loader (dotenv / --env-file / platform)
        ↓
Validate + Convert Types
        ↓
Central config module (frozen)
        ↓
Services: Database • Redis • Auth • CORS
```

### *Externalize it. Validate it. Protect it.*

**Code is shared. Secrets are not.**

⭐ **Day 8 Complete** ⭐

</div>
