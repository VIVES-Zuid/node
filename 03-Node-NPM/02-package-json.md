# 📄 package.json

## 🎯 What is package.json?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### The Heart of Every Node.js Project

Configuration file containing project metadata and dependencies

</div>

---

## 📦 Purpose of package.json

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

The `package.json` file stores information about your application:

- 📝 **Project metadata** (name, version, description)
- 📦 **Dependencies** (packages your app needs)
- 🔧 **Scripts** (commands you can run)
- 👤 **Author information**
- 📜 **License**
- 🔗 **Repository information**

### Every Node.js project that uses npm has this file!

</div>

---

## 🏗️ Creating package.json

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Step-by-Step Setup

```bash
# Create a new project directory
milan@nodeVb〽 mkdir npm-demo
milan@nodeVb〽 cd npm-demo

# Initialize npm (interactive wizard)
milan@npm-demo〽 npm init
```

**npm will ask you questions:**

```
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

package name: (npm-demo) 
version: (1.0.0) 
description: My first npm project
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: Your Name
license: (ISC) 
```

</div>

---

## ⚡ Quick Initialization

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; border-left: 5px solid #ff9800;">

### Skip the Wizard

Use the `-y` or `--yes` flag to accept all defaults:

```bash
milan@npm-demo〽 npm init --yes
```

**Output:**

```bash
Wrote to /Users/milan/Dev/npm-demo/package.json:

{
  "name": "npm-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

You can edit it later!

</div>

---

## 📋 package.json Structure

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Common Fields

```json
{
  "name": "npm-demo",
  "version": "1.0.0",
  "description": "A demo Node.js project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  },
  "keywords": ["demo", "nodejs"],
  "author": "Your Name <your.email@example.com>",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

</div>

---

## 🔑 Important Fields Explained

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e3f2fd;">
<th style="padding: 15px;">Field</th>
<th style="padding: 15px;">Description</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>name</strong></td>
<td style="padding: 15px;">Package name (must be unique if publishing)</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>version</strong></td>
<td style="padding: 15px;">Current version (follows SemVer)</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>main</strong></td>
<td style="padding: 15px;">Entry point file</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>scripts</strong></td>
<td style="padding: 15px;">Commands you can run with npm</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>dependencies</strong></td>
<td style="padding: 15px;">Production dependencies</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>devDependencies</strong></td>
<td style="padding: 15px;">Development-only dependencies</td>
</tr>
</table>

---

## 🎯 var, let, or const?

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### JavaScript Variable Declaration (ES6+)

Since ES6 (2015), we have three ways to declare variables:

### `var` - The Old Way ❌

```javascript
var greeter = "hey hi";
var greeter = "say Hello instead"; // Can redeclare
greeter = "changed again";          // Can change
```

**Problems:**
- Function-scoped (not block-scoped)
- Can be redeclared
- Hoisting issues

</div>

---

## 📝 let and const

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### `let` - Block-Scoped Variable ✅

```javascript
let greeting = "say Hi";
greeting = "say Hello instead";     // ✅ Can change

let greeting = "say Hello instead"; // ❌ Cannot redeclare
// Error: Identifier 'greeting' has already been declared
```

**Use for:** Values that will change

---

### `const` - Block-Scoped Constant ✅✅

```javascript
const greeting = "say Hi";
greeting = "say Hello instead";     // ❌ Cannot change
// Error: Assignment to constant variable

const greeting = "say Hello instead"; // ❌ Cannot redeclare
// Error: Identifier 'greeting' has already been declared
```

**Use for:** Values that won't change (most cases!)

</div>

---

## 💡 Best Practice

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50;">

### Modern JavaScript

```javascript
// ✅ GOOD: Use const by default
const name = "Alice";
const age = 25;

// ✅ GOOD: Use let when value will change
let counter = 0;
counter++;

// ❌ AVOID: var has confusing behavior
var oldStyle = "avoid this";
```

**Rule of thumb:**
1. Always use `const` first
2. Only use `let` if the value must change
3. Never use `var` in modern code

</div>

---

## 🔜 What's Next?

Now that you have a `package.json` file, let's learn how to **install third-party packages**!

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 3 Home](./README.md)

[← Previous: What is NPM](./01-what-is-npm.md) | [Next: Installing Packages →](./03-installing-packages.md)

</div>
