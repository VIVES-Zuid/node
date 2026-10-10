# 🛠️ Creating and Loading Modules

## 📝 Creating Your First Module

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Example: Logger Module

Let's create a simple logging module.

**Create `logger.js`:**

```javascript
var url = "http://mylogger.io/log";

function log(message) {
  // Send HTTP request (simplified)
  console.log(message);
}
```

This is a simple module, but we can't use it in other files yet!

</div>

---

## 📤 Exporting from a Module

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Making Functions Available

To use the `log` function in other files, we must **export** it:

**In `logger.js`:**

```javascript
var url = "http://mylogger.io/log";

function log(message) {
  console.log(message);
}

// Export the log function
module.exports.log = log;
```

</div>

---

## ⚠️ Implementation Details

<div style="background-color: #ffebee; padding: 25px; border-radius: 10px; border-left: 5px solid #f44336;">

### What Should You Export?

```javascript
// ❌ BAD: Exporting implementation details
module.exports.log = log;
module.exports.logurl = url; // URL is an implementation detail!
```

```javascript
// ✅ GOOD: Only export what's necessary
module.exports.log = log;
```

### Best Practice

**Don't export implementation details!** Only export the public API.

- ✅ Export: Functions, classes that others need
- ❌ Don't export: Internal variables, helper functions

</div>

---

## 📥 Loading a Module with `require()`

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Using Your Module

**In `app.js`:**

```javascript
// Load the module (without assignment)
require("./logger");

// Better: Assign to a variable
var logger = require("./logger");
console.log(logger);
```

**Run it:**

```bash
milan@les2〽 node app.js
{ log: [Function: log], logurl: 'http://mylogger.io:log' }
milan@les2〽
```

### What Happened?

`require()` returns the `module.exports` object from `logger.js`!

</div>

---

## 🎯 Using the Exported Function

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Calling the Log Function

```javascript
const logger = require("./logger");
logger.log("message");
```

**Output:**

```bash
milan@les2〽 node app.js
message
milan@les2〽
```

</div>

---

## 🔒 `var` vs `const` for require()

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; border-left: 5px solid #ff9800;">

### The Problem with `var`

```javascript
var logger = require("./logger");
logger = 1; // ⚠️ Oops! Accidentally reassigned
logger.log("message"); // 💥 TypeError!
```

**Result:**

```bash
TypeError: logger.log is not a function
```

### The Solution: Use `const`

```javascript
const logger = require("./logger");
logger = 1; // ❌ Error at compile time, not runtime!
logger.log("message");
```

### Best Practice

✅ **Always use `const` for `require()`**

- Prevents accidental reassignment
- Catches errors earlier
- More predictable code

</div>

---

## 🎁 Exporting a Single Function

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Better Export Pattern

Instead of exporting an object with a function, export the function directly:

**In `logger.js`:**

```javascript
var url = "http://mylogger.io:log";

function log(message) {
  console.log(message);
}

// Export the function directly
module.exports = log;
```

**In `app.js`:**

```javascript
const log = require("./logger");
log("message"); // Cleaner!
```

### Benefits

- ✅ Simpler API
- ✅ More intuitive to use
- ✅ Less typing

</div>

---

## 📝 Quick Refactoring Tip

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px;">

### VS Code Shortcut

Rename `logger.log` to just `log`:

1. Select `logger` variable
2. Press **F2** (or **Fn + F2**)
3. Type new name: `log`
4. All instances renamed!

**Before:**

```javascript
const logger = require("./logger");
logger.log("message");
```

**After:**

```javascript
const log = require("./logger");
log("message");
```

</div>

---

## 🔄 Understanding `exports` Shorthand

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Two Ways to Export

Remember the module wrapper function?

```javascript
(function (exports, require, module, __filename, __dirname) {
  // Your code
});
```

`exports` is a **reference** to `module.exports`!

### This Works ✅

```javascript
module.exports.log = log;
// OR
exports.log = log; // Same thing!
```

### This Does NOT Work ❌

```javascript
exports = log; // ❌ Breaks the reference!
```

### Why?

`exports` is just a reference to `module.exports`. Reassigning `exports` breaks that reference!

</div>

---

## 📋 Module Export Patterns

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Pattern 1: Export Object with Methods

```javascript
// logger.js
function log(message) {
  console.log(message);
}
function error(message) {
  console.error(message);
}

module.exports.log = log;
module.exports.error = error;

// OR shorthand
exports.log = log;
exports.error = error;
```

**Usage:**

```javascript
const logger = require("./logger");
logger.log("Info");
logger.error("Error!");
```

---

### Pattern 2: Export Single Function

```javascript
// logger.js
function log(message) {
  console.log(message);
}

module.exports = log;
```

**Usage:**

```javascript
const log = require("./logger");
log("Message");
```

---

### Pattern 3: Export Class

```javascript
// logger.js
class Logger {
  log(message) {
    console.log(message);
  }
  error(message) {
    console.error(message);
  }
}

module.exports = Logger;
```

**Usage:**

```javascript
const Logger = require("./logger");
const logger = new Logger();
logger.log("Message");
```

</div>

---

## ⚙️ Tool Recommendation: JSHint

<div style="background-color: #e3f2fd; padding: 20px; border-radius: 10px;">

### Code Quality Tool

Install JSHint for error checking:

```bash
npm install -g jshint
```

Run it on your files:

```bash
jshint app.js
```

JSHint will catch common mistakes like:

- Using `var` instead of `const`
- Undefined variables
- Unused variables
- And more!

</div>

---

## 🎯 Best Practices Summary

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e8f5e9;">
<th style="padding: 15px;">✅ DO</th>
<th style="padding: 15px;">❌ DON'T</th>
</tr>
<tr>
<td style="padding: 15px;">Use <code>const</code> for require()</td>
<td style="padding: 15px;">Use <code>var</code> for require()</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;">Export only public API</td>
<td style="padding: 15px;">Export implementation details</td>
</tr>
<tr>
<td style="padding: 15px;">Use <code>module.exports =</code></td>
<td style="padding: 15px;">Use <code>exports =</code> (breaks reference)</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;">Give modules clear names</td>
<td style="padding: 15px;">Use vague names</td>
</tr>
<tr>
<td style="padding: 15px;">Use <code>./</code> for local modules</td>
<td style="padding: 15px;">Omit <code>./</code> for local modules</td>
</tr>
</table>

---

## Practice (not graded)

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 25px; border-radius: 15px; color: white;">

### Create Your Own Module

1. Create a `calculator.js` module
2. Add functions: `add`, `subtract`, `multiply`, `divide`
3. Export them
4. Use them in `app.js`

**Bonus:** Try both export patterns and see which you prefer!

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 2 Home](./README.md)

[← Previous: Module System](./02-module-system.md) | [Next: Built-in Modules →](./04-builtin-modules.md)

</div>
