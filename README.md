<div align="center">

# 🚀 Day 3 — Node.js File System & File Handling

![Node.js](https://img.shields.io/badge/Node.js-20.x-339933?style=for-the-badge&logo=node.js&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Series](https://img.shields.io/badge/20%20Days-20%20Concepts-blueviolet?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)

**A 20-Day Node.js & Express.js Learning Series**

### 📁 Learn how Node.js works with files, directories, paths, and persistent data

**Read → Understand → Code → Break → Debug → Build**

</div>

---

## 👋 Welcome to Day 3

In **Day 1**, we understood the Node.js Runtime and Event Loop.
In **Day 2**, we learned how Node.js organizes code using Modules.

Now in **Day 3**, we're going one step deeper:

> 🧠 **How does a Node.js application interact with the file system?**

A backend application frequently needs to work with files:

`Reading configs` · `Creating logs` · `Uploading files` · `Storing temp data` · `Generating reports` · `Reading JSON` · `Creating directories` · `Renaming/copying/deleting files` · `Processing uploads` · `Certificates & keys`

Node.js provides the built-in **File System (`fs`) module** for these operations.

---

## 📚 Table of Contents

<details>
<summary><strong>Click to expand full index (39 sections)</strong></summary>

1. [What is a File System?](#1--what-is-a-file-system)
2. [Why Does Backend Need File Handling?](#2--why-does-backend-need-file-handling)
3. [Node.js fs Module](#3--nodejs-fs-module)
4. [Importing fs](#4--importing-fs)
5. [Three Ways to Work with fs](#5--three-ways-to-work-with-fs)
6. [Synchronous File Operations](#6--synchronous-file-operations)
7. [Asynchronous Callback API](#7--asynchronous-callback-api)
8. [fs/promises API](#8--fspromises-api)
9. [Reading a File](#9--reading-a-file)
10. [Writing a File](#10--writing-a-file)
11. [Appending to a File](#11--appending-to-a-file)
12. [Deleting a File](#12--deleting-a-file)
13. [Renaming a File](#13--renaming-a-file)
14. [Copying a File](#14--copying-a-file)
15. [Working with Directories](#15--working-with-directories)
16. [Creating Directories](#16--creating-directories)
17. [Reading Directories](#17--reading-directories)
18. [Removing Directories](#18--removing-directories)
19. [The path Module](#19--the-path-module)
20. [Absolute vs Relative Paths](#20--absolute-vs-relative-paths)
21. [__dirname and __filename](#21--dirname-and-filename)
22. [Paths in ES Modules](#22--paths-in-es-modules)
23. [File Information with stat](#23--file-information-with-stat)
24. [Working with JSON Files](#24--working-with-json-files)
25. [Building a JSON Data Store](#25--building-a-json-data-store)
26. [Error Handling](#26--error-handling)
27. [Encoding](#27--encoding)
28. [Text Files vs Binary Files](#28--text-files-vs-binary-files)
29. [Sync vs Async](#29--sync-vs-async)
30. [Why Blocking File Operations Matter](#30--why-blocking-file-operations-matter)
31. [Real Backend Use Cases](#31--real-backend-use-cases)
32. [Production Considerations](#32--production-considerations)
33. [Common Mistakes](#33--common-mistakes)
34. [Practical Project](#34--practical-project)
35. [Interview Questions](#35--interview-questions)
36. [Practice Tasks](#36--practice-tasks)
37. [Day 3 Challenge](#37--day-3-challenge--build-a-file-based-notes-api)
38. [Key Takeaways](#-day-3-quick-reference)
39. [Day 4 Preview](#39--day-4-preview)

</details>

---

## 1. 📁 What is a File System?

A **file system** is the mechanism used by an operating system to organize and manage data stored on a device.

```text
C:/
│
├── Users/
├── Program Files/
├── Projects/
└── Documents/
```

Inside a project:

```text
my-backend/
│
├── src/
├── package.json
├── README.md
├── .env
└── logs/
```

The OS manages: `Files` · `Directories` · `File names` · `Locations` · `Permissions` · `Metadata` · `Storage`

Node.js talks to the OS's file system through the `fs` module.

---

## 2. 🤔 Why Does Backend Need File Handling?

> "Backend-la database irukku. Appo files ethukku?" 🤔

Good question — databases aren't the only place applications store or access information.

| Use case | Example |
|---|---|
| 📝 **Logging** | `logs/application.log` |
| ⚙️ **Configuration** | `config/app.json` |
| 📤 **User uploads** | `uploads/profile.jpg`, `resume.pdf` |
| 📊 **Generated reports** | `reports/monthly-report.pdf` |
| 🗃️ **Temporary files** | `tmp/processing-data.json` |

File handling is an essential backend skill. 💪

---

## 3. 🟢 Node.js fs Module

Node.js provides the built-in `fs` module — **File System**.

```text
Files                     Directories
│                         │
├── Read                  ├── Create
├── Write                 ├── Read
├── Append                └── Remove
├── Rename
├── Copy
├── Delete
└── Inspect
```

It also gives access to file metadata and other file-system operations.

---

## 4. 📦 Importing fs

```js
// CommonJS
const fs = require("node:fs");

// ES Modules
import fs from "node:fs";

// Promise-based API
import fs from "node:fs/promises";
```

> The `node:` prefix clearly marks the module as a Node.js built-in.

---

## 5. 🧩 Three Ways to Work with fs

```text
                File System
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
     Sync API   Callback API   Promise API
        │            │            │
    Blocking     Async style   Async/Await
```

| Style | Example | Behavior |
|---|---|---|
| **1. Synchronous** | `fs.readFileSync("notes.txt", "utf8")` | Blocks until finished |
| **2. Callback** | `fs.readFile("notes.txt", "utf8", (err, data) => {...})` | Asynchronous |
| **3. Promise** | `await fs.readFile("notes.txt", "utf8")` | Async/await friendly |

> 💡 For modern backend code, the **promise API** is usually the most convenient — it integrates cleanly with `async` functions.

---

## 6. ⏸️ Synchronous File Operations

Synchronous operations **block** JS execution until the operation completes.

```js
const fs = require("node:fs");

console.log("1");
const data = fs.readFileSync("notes.txt", "utf8");
console.log(data);
console.log("2");
```

```text
console.log("1") → readFileSync() → Wait → File completed
                  → console.log(data) → console.log("2")
```

**✅ Reasonable for:** small startup-config reads · simple CLI scripts · build scripts · one-time init
**⚠️ Use carefully in:** request-handling paths of a server

---

## 7. 🔄 Asynchronous Callback API

```js
const fs = require("node:fs");

fs.readFile("notes.txt", "utf8", (err, data) => {
    if (err) {
        console.error("Failed to read file:", err);
        return;
    }
    console.log(data);
});

console.log("File operation started");
```

```text
Main JavaScript
      │
      ├── Start read
      │
      └── Continue executing
               │
               ↓
          File operation
               │
               ↓
            Callback
```

> This prevents the JS thread from sitting idle while the file operation finishes.

---

## 8. ⚡ fs/promises API

```js
import fs from "node:fs/promises";

async function readNotes() {
    try {
        const data = await fs.readFile("notes.txt", "utf8");
        console.log(data);
    } catch (error) {
        console.error("Unable to read file:", error.message);
    }
}

readNotes();
```

> Especially useful in modern Node.js applications.

---

## 9. 📖 Reading a File

```js
import fs from "node:fs/promises";

const content = await fs.readFile("notes.txt", "utf8");
console.log(content);
// Node.js is a JavaScript runtime.
```

> ⚠️ **Without encoding**, `fs.readFile("notes.txt")` returns a **`Buffer`** — covered deeply in **Day 5**.

---

## 10. ✍️ Writing a File

```js
import fs from "node:fs/promises";

await fs.writeFile("message.txt", "Hello from Node.js");
```

> If the file doesn't exist, Node creates it. If it exists, `writeFile` **replaces** its contents.

```js
async function createFile() {
    await fs.writeFile("user.txt", "Jerome\nBackend Developer");
    console.log("File created");
}
```

---

## 11. ➕ Appending to a File

Use `appendFile()` when you want to **add**, not replace:

```js
import fs from "node:fs/promises";

await fs.appendFile("logs.txt", "Server started\n");
await fs.appendFile("logs.txt", "Database connected\n");
```

```text
logs.txt
────────
Server started
Database connected
```

> Great for simple file-based logs.

---

## 12. 🗑️ Deleting a File

```js
import fs from "node:fs/promises";

try {
    await fs.rm("old-file.txt");
    console.log("File deleted");
} catch (error) {
    console.error("Delete failed:", error.message);
}
```

```js
await fs.rm("old-file.txt", { force: true });
```

---

## 13. ✏️ Renaming a File

```js
import fs from "node:fs/promises";

await fs.rename("old-name.txt", "new-name.txt");
```

```text
old-name.txt  →  new-name.txt
```

---

## 14. 📋 Copying a File

```js
import fs from "node:fs/promises";

await fs.copyFile("original.txt", "backup.txt");
```

```text
original.txt  +  backup.txt   (both contain the same content)
```

---

## 15. 📂 Working with Directories

```text
project/
│
├── src/
├── uploads/
├── logs/
└── temp/
```

```text
Create directory → Read directory → Rename directory → Remove directory
```

---

## 16. ➕ Creating Directories

```js
import fs from "node:fs/promises";

await fs.mkdir("uploads");
```

**Nested directories:**

```js
await fs.mkdir("storage/uploads/images", { recursive: true });
```

> `recursive: true` lets Node create all required parent directories.

---

## 17. 📋 Reading Directories

```js
import fs from "node:fs/promises";

const files = await fs.readdir("uploads");
console.log(files);
// ["profile.jpg", "resume.pdf", "certificate.pdf"]
```

**Getting file types:**

```js
const entries = await fs.readdir("uploads", { withFileTypes: true });

for (const entry of entries) {
    console.log(entry.name, entry.isFile(), entry.isDirectory());
}
```

---

## 18. 🗑️ Removing Directories

```js
// Empty directory
await fs.rmdir("temp");

// Modern & flexible
await fs.rm("temp", { recursive: true, force: true });
```

> ⚠️ **Be extremely careful** with recursive deletion — it can remove an entire directory tree.

---

## 19. 🧭 The path Module

> How do we safely construct file paths?

**🚫 Don't do this:**
```js
const file = "uploads/" + userId + "/" + fileName;
```

Path separators differ across OSes — use the built-in `path` module instead.

```js
import path from "node:path";
```

| Method | Purpose |
|---|---|
| `path.join()` | Builds a path with the correct OS separator |
| `path.resolve()` | Produces an absolute path |
| `path.basename()` | Gets the file name |
| `path.dirname()` | Gets the directory |
| `path.extname()` | Gets the extension |
| `path.parse()` | Breaks a path into parts |

```js
path.join("uploads", "users", "profile.jpg");
// uploads/users/profile.jpg

path.resolve("uploads", "profile.jpg");
// /absolute/path/uploads/profile.jpg

path.basename("/uploads/profile/user.jpg");
// user.jpg

path.extname("resume.pdf");
// .pdf

path.dirname("/uploads/profile/user.jpg");
// /uploads/profile
```

---

## 20. 🛣️ Absolute vs Relative Paths

| Type | Example |
|---|---|
| **Relative** | `./data/users.json` (depends on context) |
| **Absolute (Windows)** | `C:\Projects\backend\data\users.json` |
| **Absolute (Linux)** | `/home/user/backend/data/users.json` |

> ⚠️ `fs.readFile("./data/users.json")` can be problematic — the relative path depends on the process's **current working directory**. Building paths from a known application location is usually safer.

---

## 21. 📍 __dirname and __filename

Available by default in **CommonJS**:

```js
console.log(__dirname);   // directory of current module
console.log(__filename);  // full path of current module
```

```js
const path = require("node:path");

const filePath = path.join(__dirname, "data", "users.json");
console.log(filePath);
```

---

## 22. 🟩 Paths in ES Modules

> ⚠️ `__dirname` and `__filename` are **not automatically available** in ES Modules.

```js
import { fileURLToPath } from "node:url";
import path from "node:path";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const filePath = path.join(__dirname, "data", "users.json");
```

---

## 23. 🔍 File Information with stat()

```js
import fs from "node:fs/promises";

const stats = await fs.stat("notes.txt");

console.log("Size:", stats.size);
console.log("Modified:", stats.mtime);
console.log("Is File:", stats.isFile());
console.log("Is Directory:", stats.isDirectory());
```

**Useful for:** file validation · size checks · upload handling · metadata · debugging

---

## 24. 🧾 Working with JSON Files

A common beginner pattern — JSON as temporary/local storage.

```json
[
    { "id": 1, "name": "Jerome", "role": "Backend Developer" },
    { "id": 2, "name": "Alex", "role": "Frontend Developer" }
]
```

```text
Read JSON → Convert JSON → Object → Modify data
          → Convert Object → JSON → Write JSON
```

---

## 25. 🏗️ Building a JSON Data Store

```text
day-3-file-system/
│
├── package.json
├── app.js
├── data/
│   └── users.json
└── utils/
    └── userStore.js
```

**`userStore.js`**
```js
import fs from "node:fs/promises";
import path from "node:path";

const filePath = path.resolve("data", "users.json");

export async function getUsers() {
    const content = await fs.readFile(filePath, "utf8");
    return JSON.parse(content);
}

export async function saveUsers(users) {
    const content = JSON.stringify(users, null, 2);
    await fs.writeFile(filePath, content);
}
```

**`app.js`**
```js
import { getUsers, saveUsers } from "./utils/userStore.js";

const users = await getUsers();

users.push({ id: 1, name: "Jerome", role: "Backend Developer" });

await saveUsers(users);
console.log("User saved");
```

```text
users.json → readFile() → JSON string → JSON.parse() → Array
           → Modify → JSON.stringify() → writeFile() → users.json
```

---

## 26. 🚨 Error Handling

```text
File doesn't exist → Permission denied → Invalid path
    → Directory doesn't exist → Disk/storage problem
```

```js
import fs from "node:fs/promises";

try {
    const content = await fs.readFile("unknown.txt", "utf8");
    console.log(content);
} catch (error) {
    console.error("File operation failed:", error.message);
}
```

**Checking the error code:**

```js
try {
    await fs.readFile("users.json", "utf8");
} catch (error) {
    if (error.code === "ENOENT") {
        console.log("File does not exist");
    } else {
        throw error;
    }
}
```

---

## 27. 🔤 Encoding

```js
const content = await fs.readFile("message.txt", "utf8");
```

Without encoding → you get a `Buffer` instead of a string.

> **UTF-8** can represent a huge range of characters: `Hello` · `வணக்கம்` · `こんにちは`

---

## 28. 🧱 Text Files vs Binary Files

| Text Files | Binary Files |
|---|---|
| `.txt` `.json` `.csv` `.html` `.js` `.css` | `.jpg` `.png` `.pdf` `.mp3` `.mp4` `.zip` |
| Readable as UTF-8 text | Handled as `Buffer` |

```js
const imageData = await fs.readFile("profile.jpg");
// imageData is a Buffer
```

> 🔜 Buffers & Streams get a deep dive in **Day 5**.

---

## 29. ⚖️ Sync vs Async

| Feature | Sync | Async |
|---|---|---|
| Blocks current JS execution | ✅ Yes | ❌ No |
| Simple syntax | ✅ | ✅ |
| Good for server request paths | ❌ Usually no | ✅ Usually yes |
| CLI/scripts | ✅ Can be useful | ✅ Useful |
| Handles concurrent work better | ❌ No | ✅ Yes |
| Typical modern server choice | Limited | **Preferred** |

```js
// Sync
const data = fs.readFileSync("data.txt", "utf8");

// Async
const data = await fs.readFile("data.txt", "utf8");
```

---

## 30. 🚦 Why Blocking File Operations Matter

```text
Request A ── readFileSync() ── BLOCK
                                 │
Request B ──────────────────────┤ waiting
Request C ──────────────────────┤ waiting
Request D ──────────────────────┤ waiting
                                 │
                          operation ends
                                 │
                                 ↓
                       Event Loop continues
```

> ⚠️ Blocking operations inside frequently executed HTTP request handlers can hurt server responsiveness.

---

## 31. 🏢 Real Backend Use Cases

| # | Use Case | Example |
|---|---|---|
| 1 | Logging | `logs/application.log` |
| 2 | File Uploads | `uploads/profile.jpg`, `resume.pdf` |
| 3 | Report Generation | `reports/monthly-report.csv` |
| 4 | Configuration | `config/application.json` |
| 5 | Temporary Processing | `tmp/upload-1` |
| 6 | Static Assets | images, documents, public assets |

---

## 32. 🏭 Production Considerations

> ⚠️ Moving beyond beginner-level code.

1. **Don't use JSON files as your production database.** Use `PostgreSQL`, `MySQL`, or `MongoDB` for concurrency control, transactions, indexing, querying, constraints, recovery, and scalability.
2. **Don't load huge files into memory unnecessarily.** Use **streams** (Day 5) for large files.
3. **Validate file paths.** Never trust user-controlled paths blindly — watch for `../../../../some-sensitive-file`.
4. **Handle errors.** Never assume `fs.readFile(file)` always succeeds.
5. **Be careful with concurrent writes.** Multiple simultaneous writes to the same file → race conditions. A database handles this better.
6. **Don't store secrets in files casually.** Keep passwords, API keys, and credentials out of files that might get committed.

---

## 33. ❌ Common Mistakes

| # | Mistake | Fix |
|---|---|---|
| 1 | `fs.readFileSync()` in every request handler | Use async APIs for high-concurrency servers |
| 2 | Hardcoding OS-specific paths | Use `path.join()` / `path.resolve()` |
| 3 | Forgetting error handling | Wrap in `try/catch` |
| 4 | Using the wrong encoding | `"utf8"` for text, raw `Buffer` for binary |
| 5 | Reading huge files fully into memory | Use **Streams** |
| 6 | Treating JSON as a database | Use a real DB for production |

---

## 34. 🛠️ Practical Project — File Manager CLI

```text
day-3-file-manager/
│
├── package.json
├── app.js
├── data/
│   └── notes.txt
└── utils/
    └── fileManager.js
```

**`fileManager.js`**
```js
import fs from "node:fs/promises";
import path from "node:path";

const dataDirectory = path.resolve("data");

export async function createDirectory() {
    await fs.mkdir(dataDirectory, { recursive: true });
}

export async function writeNote(fileName, content) {
    const filePath = path.join(dataDirectory, fileName);
    await fs.writeFile(filePath, content, "utf8");
}

export async function readNote(fileName) {
    const filePath = path.join(dataDirectory, fileName);
    return fs.readFile(filePath, "utf8");
}

export async function deleteNote(fileName) {
    const filePath = path.join(dataDirectory, fileName);
    await fs.rm(filePath);
}
```

**`app.js`**
```js
import { createDirectory, writeNote, readNote, deleteNote } from "./utils/fileManager.js";

async function main() {
    await createDirectory();
    await writeNote("notes.txt", "Learning Node.js File System");

    const content = await readNote("notes.txt");
    console.log("Content:");
    console.log(content);

    // await deleteNote("notes.txt");
}

main().catch(console.error);
```

---

## 35. 🎤 Interview Questions

<details>
<summary><strong>🟢 Beginner (1–5)</strong></summary>

1. **What is the Node.js `fs` module?**
   Node.js's built-in File System module — APIs for interacting with files and directories.

2. **Is `fs` a third-party package?**
   No, it's built in: `import fs from "node:fs";`

3. **How do you read a file?**
   ```js
   const data = await fs.readFile("file.txt", "utf8");
   ```

4. **How do you write a file?**
   ```js
   await fs.writeFile("file.txt", "Hello");
   ```

5. **How do you append content?**
   ```js
   await fs.appendFile("file.txt", "New content");
   ```

</details>

<details>
<summary><strong>🟡 Intermediate (6–10)</strong></summary>

6. **Difference between `readFile()` and `readFileSync()`?**
   `readFile()` is asynchronous; `readFileSync()` blocks until the operation completes.

7. **What does `fs/promises` provide?**
   Promise-based versions of many file-system operations, usable with `async/await`.

8. **What happens if `readFile()` doesn't specify an encoding?**
   The result is returned as a `Buffer`.

9. **Why is `path.join()` useful?**
   It builds paths using the platform-specific separator, avoiding manual string concatenation.

10. **What is `path.resolve()`?**
    It resolves path segments into an absolute path.

</details>

<details>
<summary><strong>🔴 Advanced (11–15)</strong></summary>

11. **Why avoid synchronous file operations inside HTTP request handlers?**
    They block the JS execution thread, delaying other requests and hurting responsiveness.

12. **Why are streams useful for large files?**
    They process data incrementally instead of loading the entire file into memory.

13. **What is a Buffer?**
    Raw binary data representation in Node.js, common when reading binary files or network data.

14. **Why shouldn't JSON files normally be used as a production database?**
    They lack the concurrency, querying, transaction, indexing, and consistency capabilities of a real database.

15. **What is `ENOENT`?**
    A common Node.js filesystem error code indicating a referenced file/directory doesn't exist.

</details>

---

## 36. 🧪 Practice Tasks

> Don't just copy the examples — **type them yourself.**

- [ ] **Task 1 — Create a File:** Create `hello.txt` with `"Hello Node.js"`.
- [ ] **Task 2 — Read a File:** Read `hello.txt` and print the content.
- [ ] **Task 3 — Append Data:** Append `"Learning File System"` without deleting previous content.
- [ ] **Task 4 — Rename:** Rename `hello.txt` → `node.txt`.
- [ ] **Task 5 — Copy:** Create `backup.txt` by copying `node.txt`.
- [ ] **Task 6 — Directory Manager:** Create `storage/images/`, `storage/documents/`, `storage/temp/`.
- [ ] **Task 7 — Directory Listing:** Print every file/directory inside `storage/`.
- [ ] **Task 8 — File Metadata:** Use `stat()` to print size, modified time, `isFile()`, `isDirectory()`.
- [ ] **Task 9 — JSON CRUD:** Build Create/Read/Update/Delete for `users.json` using `fs/promises`.

---

## 37. 🔥 Day 3 Challenge — Build a File-Based Notes API

```text
notes-app/
│
├── package.json
├── app.js
├── data/
│   └── notes.json
├── services/
│   └── notes.service.js
└── utils/
    └── file.util.js
```

**Requirements**

| Function | Signature |
|---|---|
| Create Note | `createNote(title, content)` |
| Get Notes | `getNotes()` |
| Get Single Note | `getNoteById(id)` |
| Update Note | `updateNote(id, data)` |
| Delete Note | `deleteNote(id)` |

**Data format**
```json
[
    { "id": 1, "title": "Learn Node.js", "content": "Understand the event loop" },
    { "id": 2, "title": "Learn Express", "content": "Understand middleware" }
]
```

**Architecture**
```text
app.js → notes.service.js → file.util.js → notes.json
```

> This mirrors real backend architecture — later, the JSON file can be swapped for **PostgreSQL** without changing the app's responsibility structure.

---

## 🧠 Mental Model

```text
Node.js Application
        │
        ↓
     fs Module
        │
   ┌────┼────┐
   ↓    ↓    ↓
 Read Write Delete
   │    │    │
   └────┼────┘
        ↓
     File System
        │
        ↓
 Operating System
        │
        ↓
     Storage
```

**Async flow**
```text
Your Code → fs/promises → Node.js Runtime → Operating System
          → File System → File Operation → Promise resolves
          → await continues
```

---

## 🔥 The Most Important Lesson

Don't memorize `readFile()`, `writeFile()`, `appendFile()`, `mkdir()`, `rm()` as isolated APIs.

```text
Application → Needs data/file → Node.js fs API
            → Operating System → File System → Data
```

> **Node.js is designed around asynchronous I/O, so understanding blocking vs non-blocking file operations matters more than memorizing API names.**

---

## 📌 Day 3 Quick Reference

<table>
<tr>
<td valign="top" width="50%">

```js
// Read
await fs.readFile("file.txt", "utf8");

// Write
await fs.writeFile("file.txt", "Hello");

// Append
await fs.appendFile("file.txt", "More data");

// Rename
await fs.rename("old.txt", "new.txt");

// Copy
await fs.copyFile("source.txt", "backup.txt");
```

</td>
<td valign="top" width="50%">

```js
// Delete
await fs.rm("file.txt");

// Create Directory
await fs.mkdir("uploads", { recursive: true });

// Read Directory
const files = await fs.readdir("uploads");

// File Info
const stats = await fs.stat("file.txt");

// Path
const filePath = path.join("uploads", "profile.jpg");
```

</td>
</tr>
</table>

---

## ✅ Day 3 Checklist

- [ ] Understand what a file system is
- [ ] Understand Node.js `fs`
- [ ] Understand sync, callback & `fs/promises` APIs
- [ ] Read / write / append / rename / copy / delete files
- [ ] Create / read / remove directories
- [ ] Understand `path`, relative & absolute paths
- [ ] Understand `__dirname`/`__filename` (CJS) & ESM path handling
- [ ] Use `stat()`
- [ ] Understand encoding & Buffer basics
- [ ] Handle filesystem errors
- [ ] Understand sync vs async & blocking operations
- [ ] Complete JSON CRUD
- [ ] Complete the Notes App challenge
- [ ] Practice interview questions

---

## 🎯 What You Should Be Able to Explain After Day 3

**"How does Node.js read a file?"**
```text
Application → fs/promises → readFile() → Node.js/OS file system interaction
            → File data → Promise resolves → await receives result
```

**"Why shouldn't I use `readFileSync()` in every API request?"**
```text
readFileSync() → Blocks JS execution → Other work delayed → Poorer server responsiveness
```

**"What should I use for huge files?"**
```text
Large File → Streams → Process incrementally → Lower memory pressure
```

> That's exactly why **Day 5 covers Streams & Buffers**.

---

## 🚀 39. Day 4 Preview

### EventEmitter — Events & Event-Driven Architecture

```text
Event → EventEmitter → emit() → Listener → Callback
```

**Topics:** `on()` · `once()` · `emit()` · `off()` · event arguments · custom events · event-driven architecture · error events · EventEmitter internals · real-world use cases · building a custom event system

---

<div align="center">

### ⭐ Learning Philosophy

```text
Learn → Understand → Write Code → Make Mistakes → Debug → Build → Repeat
```

**Don't just learn Node.js APIs. Understand why they exist, what happens underneath, and when you should use them.**

---

### 🚀 Day 3 — File System & File Handling

**Read • Write • Understand • Build**

**One concept closer to becoming a stronger backend developer.**

---

*Part of the 20 Days • 20 Concepts — Node.js & Express.js Learning Series*

</div>
