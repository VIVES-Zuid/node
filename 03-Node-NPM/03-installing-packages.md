# 📥 Installing Packages

## 🎯 Adding Third-Party Libraries

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Extend Your App with npm Packages

Install and use packages from the npm registry

</div>

---

## 📦 Installing a Package

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Example: Installing Underscore

Underscore is a popular JavaScript utility library.

**Find it on npm:**
🔗 https://www.npmjs.com/package/underscore

**Install it:**

```bash
milan@npm-demo〽 npm i underscore

added 1 package, and audited 2 packages in 2s

found 0 vulnerabilities
milan@npm-demo〽
```

</div>

---

## 📝 What Happened?

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Three Things Changed

**1. package.json Updated**

```json
{
  "dependencies": {
    "underscore": "^1.12.0"
  }
}
```

**2. node_modules Folder Created**

```
npm-demo/
├── node_modules/
│   └── underscore/
├── package.json
└── package-lock.json
```

**3. package-lock.json Created**

Locks exact versions of all dependencies.

</div>

---

## ⚠️ Old Syntax (No Longer Needed)

<div style="background-color: #fff3e0; padding: 20px; border-radius: 10px; border-left: 5px solid #ff9800;">

### Before npm 5

You had to use `--save`:

```bash
npm install package --save  # ❌ No longer needed
```

### Now (npm 5+)

Automatically saves to package.json:

```bash
npm i package  # ✅ Automatically saves
```

</div>

---

## 💻 Using the Installed Package

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Create index.js

```javascript
const _ = require('underscore');

// Use underscore's contains method
const result = _.contains([1, 2, 3], 2);
console.log(result);
```

**Run it:**

```bash
milan@npm-demo〽 node index.js
true
milan@npm-demo〽
```

</div>

> 📖 **Documentation:** http://underscorejs.org/

---

## 🔍 How require() Works

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Search Order

When you `require('package')`, Node.js searches in this order:

**1. Core Modules**
```javascript
require('fs')        // ✅ Built-in module
require('node:fs')   // ✅ Explicit core module
```

**2. File or Directory**
```javascript
require('./logger')     // ✅ Local file
require('./lib/utils')  // ✅ Local directory
```

**3. node_modules**
```javascript
require('underscore')   // ✅ npm package
require('express')      // ✅ npm package
```

</div>

---

## 🧪 Lab: Install Mongoose

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 25px; border-radius: 15px; color: white;">

### Practice Exercise

1. Create a new project directory
2. Initialize npm
3. Install the `mongoose` package
4. Check the installed version
5. Explore the two package.json files (yours and mongoose's)
6. Look inside the node_modules folder

**Questions:**
- What version was installed?
- How many files are in node_modules?
- What dependencies does mongoose have?

</div>

---

## 📁 node_modules Directory

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### What's Inside?

The `node_modules` folder contains:
- Your direct dependencies
- Dependencies of your dependencies
- All their dependencies (recursive)

### Example Structure

```
node_modules/
├── underscore/
│   ├── package.json
│   ├── underscore.js
│   └── ...
├── express/
│   ├── package.json
│   ├── lib/
│   └── ...
└── ... (many more!)
```

</div>

---

## 🎯 Key Points

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px;">

### Installing Packages

✅ Use `npm i package-name` to install  
✅ Automatically adds to package.json  
✅ Creates node_modules folder  
✅ Use `require('package-name')` in your code  
✅ Core modules don't need installation  
✅ Local files need `./` prefix

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 3 Home](./README.md)

[← Previous: package.json](./02-package-json.md) | [Next: Semantic Versioning →](./04-semantic-versioning.md)

</div>
