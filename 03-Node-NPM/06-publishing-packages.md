# 📤 Publishing Packages

## 🎯 Share Your Code with the World

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Publish Your Own npm Package

Contribute to the npm ecosystem!

</div>

---

## 🏗️ Creating a Package

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Step 1: Create Your Project

```bash
# Create project directory
mkdir vives-lib
cd vives-lib

# Initialize npm
npm init
```

### Step 2: Create Your Module

**index.js:**

```javascript
module.exports.add = function(a, b) {
    return a + b;
};

module.exports.multiply = function(a, b) {
    return a * b;
};
```

</div>

---

## 👤 npm Account Setup

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Create an Account

**Option 1: On the website**
- Go to [npmjs.com](https://www.npmjs.com/)
- Click "Sign Up"
- Fill in details

**Option 2: Via command line**

```bash
npm adduser
```

### Login

```bash
milan@vives-lib〽 npm login
npm notice Log in on https://registry.npmjs.org/
Username: vives
Password: 
Email: (this IS public) milan.dima@vives.be
Logged in as vives on https://registry.npmjs.org/.
```

⚠️ **Note:** Your email will be public!

</div>

---

## 📦 Publishing Your Package

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### The Publish Command

```bash
milan@vives-lib〽 npm publish

npm notice 
npm notice 📦  vives-lib@1.0.0
npm notice === Tarball Contents === 
npm notice 95B  index.js      
npm notice 224B package.json  
npm notice === Tarball Details === 
npm notice name:          vives-lib
npm notice version:       1.0.0
npm notice package size:  400 B
npm notice unpacked size: 319 B
npm notice shasum:        abc123...
npm notice integrity:     sha512-...
npm notice total files:   2
npm notice 
+ vives-lib@1.0.0
```

🎉 **Published!**

</div>

---

## ⚠️ Package Name Must Be Unique

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; border-left: 5px solid #ff9800;">

### Name Conflicts

If the name is already taken, you'll get an error:

```
npm ERR! 403 Forbidden - PUT https://registry.npmjs.org/vives-lib
npm ERR! You do not have permission to publish "vives-lib".
npm ERR! Are you logged in as the correct user?
```

### Solutions

1. **Choose a different name** in package.json
2. **Use a scoped package**: `@username/package-name`

```json
{
  "name": "@vives/my-lib",
  "version": "1.0.0"
}
```

</div>

---

## 🔍 View Your Published Package

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### On npmjs.com

Visit: `https://www.npmjs.com/package/vives-lib`

You'll see:
- 📊 Package information
- 📖 README content
- 📥 Installation instructions
- 📈 Download stats
- 🔗 Links (repository, homepage)

</div>

---

## 🧪 Lab: Use Your Published Package

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 25px; border-radius: 15px; color: white;">

### Test Your Package

**Create a new project:**

```bash
mkdir adder
cd adder
npm init -y
npm i vives-lib
```

**Use it (index.js):**

```javascript
const vives = require('vives-lib');

const result = vives.add(1, 2);
console.log(result);  // 3

const product = vives.multiply(3, 4);
console.log(product);  // 12
```

**Run it:**

```bash
node index.js
```

</div>

---

## 📥 Installing Your Package

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Anyone Can Install It Now!

```bash
milan@viveslib-vb〽 npm i vives-lib

added 1 package, and audited 2 packages in 3s

found 0 vulnerabilities
```

**Use it:**

```javascript
const vives = require('vives-lib');
const result = vives.add(1, 2);
console.log(result);
```

**Output:**

```bash
milan@viveslib-vb〽 node index.js
3
milan@viveslib-vb〽
```

</div>

---

## 🔄 Publishing Updates

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Update Your Code

**Add a new function to index.js:**

```javascript
module.exports.add = function(a, b) {
    return a + b;
};

module.exports.multiply = function(a, b) {
    return a * b;
};

// New function!
module.exports.subtract = function(a, b) {
    return a - b;
};
```

### Can't Publish Yet!

```bash
milan@vives-lib〽 npm publish
npm ERR! 403 Forbidden - PUT https://registry.npmjs.org/vives-lib
npm ERR! You cannot publish over the previously published version 1.0.0.
```

</div>

---

## 📈 Updating Version Numbers

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Option 1: Manual Update

Edit package.json:

```json
{
  "version": "1.1.0"  // Changed from 1.0.0
}
```

### Option 2: npm version Command (Better!)

```bash
# Patch update (1.0.0 → 1.0.1)
npm version patch

# Minor update (1.0.0 → 1.1.0)
npm version minor

# Major update (1.0.0 → 2.0.0)
npm version major
```

**Example:**

```bash
milan@vives-lib〽 npm version minor
v1.1.0
```

This also creates a git tag automatically!

</div>

---

## 📤 Publish the Update

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Now Publish

```bash
milan@vives-lib〽 npm publish

npm notice 
npm notice 📦  vives-lib@1.1.0
npm notice === Tarball Contents === 
npm notice 145B index.js      
npm notice 224B package.json  
npm notice === Tarball Details === 
...
+ vives-lib@1.1.0
```

✅ **Version 1.1.0 published!**

### Users Can Now Update

```bash
npm update vives-lib
# or
npm i vives-lib@latest
```

</div>

---

## 📋 Best Practices

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50;">

### Publishing Guidelines

✅ **DO:** Choose a unique, descriptive name  
✅ **DO:** Include a README.md file  
✅ **DO:** Follow semantic versioning  
✅ **DO:** Test your package before publishing  
✅ **DO:** Include a license  
✅ **DO:** Document your API

❌ **DON'T:** Publish untested code  
❌ **DON'T:** Include secrets or credentials  
❌ **DON'T:** Publish node_modules  
❌ **DON'T:** Use misleading names  
❌ **DON'T:** Forget to update version numbers

</div>

---

## 📝 Adding a README

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Create README.md

npm automatically displays your README on the package page:

```markdown
# vives-lib

Simple math utility library for Node.js

## Installation

\`\`\`bash
npm install vives-lib
\`\`\`

## Usage

\`\`\`javascript
const vives = require('vives-lib');

console.log(vives.add(1, 2));      // 3
console.log(vives.multiply(3, 4)); // 12
console.log(vives.subtract(5, 2)); // 3
\`\`\`

## API

### add(a, b)
Returns the sum of a and b.

### multiply(a, b)
Returns the product of a and b.

### subtract(a, b)
Returns the difference of a and b.
```

</div>

---

## 🗑️ Unpublishing Packages

<div style="background-color: #ffebee; padding: 20px; border-radius: 10px; border-left: 5px solid #f44336;">

### Remove a Package

**Within 72 hours:**

```bash
npm unpublish vives-lib
```

**After 72 hours:**
- Can only unpublish if no one depends on it
- Better to deprecate instead

**Deprecate a package:**

```bash
npm deprecate vives-lib "No longer maintained"
```

⚠️ **Warning:** Unpublishing breaks the ecosystem!

</div>

---

## 🎯 Chapter 3 Complete!

<div style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### What You've Learned

✅ What npm is and how to use it  
✅ Creating and managing package.json  
✅ Installing and using packages  
✅ Understanding semantic versioning  
✅ Managing dependencies  
✅ Publishing your own packages

**Next:** Build web applications with Express.js!

</div>

---

## 📝 Assignment

<div style="background-color: #fce4ec; padding: 25px; border-radius: 10px;">

### GitHub Classroom Lab

Complete the Chapter 3 lab assignment:

🔗 **See Toledo for the GitHub Classroom link (Les 3)**

Practice creating, publishing, and using npm packages!

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 3 Home](./README.md)

[← Previous: Managing Dependencies](./05-managing-dependencies.md) | [Next Chapter: Express API →](../04-Express-API/README.md)

</div>
