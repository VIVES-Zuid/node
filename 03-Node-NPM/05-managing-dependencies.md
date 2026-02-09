# 🔧 Managing Dependencies

## 🎯 Keeping Your Project Healthy

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Update, Upgrade, and Maintain

Learn to manage your project's dependencies

</div>

---

## 📁 node_modules Directory

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### History: The Nested Nightmare

**Before npm 3:**
- Dependencies were nested inside each package folder
- Same package installed multiple times
- Windows path length limits were hit
- Huge directory sizes

**Now (npm 3+):**
- ✅ Flat structure
- ✅ Shared dependencies at root level
- ✅ Nested only when version conflicts exist
- ✅ Much smaller and efficient

</div>

---

## 🔍 Checking for Updates

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### npm outdated

See which packages have newer versions:

```bash
milan@npm-demo〽 npm outdated
```

**Output:**

```
Package     Current  Wanted   Latest   Location
mongoose    2.4.2    2.9.10   5.11.17  node_modules/mongoose
underscore  1.4.0    1.12.0   1.12.0   node_modules/underscore
```

| Column | Meaning |
|--------|---------|
| **Current** | Version you have installed |
| **Wanted** | Max version according to SemVer in package.json |
| **Latest** | Latest version available on npm |
| **Location** | Where the package is installed |

</div>

---

## 🔄 Updating Packages

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### npm update

Update to the "Wanted" version:

```bash
milan@npm-demo〽 npm update
```

**⚠️ Important:** This only updates **Minor** and **Patch** versions!

**After update:**

```bash
milan@npm-demo〽 npm outdated
Package     Current  Wanted   Latest   Location
mongoose    2.9.10   2.9.10   5.11.17  node_modules/mongoose
```

Notice: **Major** version (5.x.x) not installed automatically!

</div>

---

## ⬆️ Upgrading to Latest Version

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; border-left: 5px solid #ff9800;">

### Install Specific Version

For major version updates, install explicitly:

```bash
# Upgrade to latest
milan@npm-demo〽 npm i mongoose@latest

# Or specific version
milan@npm-demo〽 npm i mongoose@5.11.17

added 34 packages, removed 2 packages, changed 2 packages
```

**package.json updates:**

```json
{
  "dependencies": {
    "mongoose": "^5.11.17"  // Updated!
  }
}
```

</div>

---

## ⬇️ Downgrading Packages

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Install Older Version

Sometimes you need an older version:

```bash
milan@npm-demo〽 npm i mongoose@2.4.2

removed 34 packages, added 2 packages, changed 2 packages
```

**When to downgrade:**
- New version has bugs
- Compatibility issues
- Testing with specific version

</div>

---

## 🧪 Lab: Downgrade Underscore

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 25px; border-radius: 15px; color: white;">

### Practice Exercise

1. Check your current underscore version
2. Downgrade to version 1.4.0
3. Verify the installed version
4. Check package.json
5. Try `npm outdated` again

**Command:**
```bash
npm i underscore@1.4.0
```

</div>

---

## 🗑️ Uninstalling Packages

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### npm uninstall

Remove a package completely:

```bash
milan@npm-demo〽 npm un mongoose

# or
milan@npm-demo〽 npm uninstall mongoose

removed 4 packages, and audited 33 packages in 2s
```

**What happens:**
1. ✅ Removed from node_modules
2. ✅ Removed from package.json
3. ✅ Dependencies also removed (if not needed by others)

</div>

---

## 🌍 Global vs Local Packages

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Global Packages

Some packages are **command-line tools**, not app dependencies:

```bash
# Install globally
npm i -g npm

# Check global packages
npm -g list --depth=0

# Check for global updates
npm -g outdated

# Update global package
npm i -g npm@latest

# Uninstall global package
npm un -g package-name
```

**Common global packages:**
- `npm` - npm itself
- `nodemon` - Auto-restart on file changes
- `typescript` - TypeScript compiler
- `eslint` - JavaScript linter

</div>

---

## 🔒 Dev Dependencies

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Development-Only Dependencies

Some tools are only needed during development:

```bash
# Install as dev dependency
milan@npm-demo〽 npm i jshint --save-dev

added 31 packages, and audited 37 packages in 4s
```

**package.json:**

```json
{
  "devDependencies": {
    "jshint": "^2.12.0"
  }
}
```

**Use for:**
- Testing frameworks (Jest, Mocha)
- Build tools (Webpack, Babel)
- Linters (ESLint, JSHint)
- Type checkers (TypeScript)

</div>

---

## 🚫 Git and node_modules

<div style="background-color: #ffebee; padding: 25px; border-radius: 10px; border-left: 5px solid #f44336;">

### ⚠️ Never Commit node_modules!

**Why?**
- Often 100+ MB
- Can be thousands of files
- Easily regenerated
- Different per OS sometimes

### Solution: .gitignore

Create `.gitignore` file:

```
node_modules/
```

**When cloning a project:**

```bash
# After cloning, install dependencies
npm install
```

This reads package.json and installs everything!

</div>

---

## 💡 Useful npm Commands

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e3f2fd;">
<th style="padding: 15px;">Command</th>
<th style="padding: 15px;">Purpose</th>
</tr>
<tr>
<td style="padding: 15px;"><code>npm outdated</code></td>
<td style="padding: 15px;">Check for updates</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><code>npm update</code></td>
<td style="padding: 15px;">Update minor/patch versions</td>
</tr>
<tr>
<td style="padding: 15px;"><code>npm list</code></td>
<td style="padding: 15px;">List installed packages</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><code>npm view pkg</code></td>
<td style="padding: 15px;">View package info</td>
</tr>
<tr>
<td style="padding: 15px;"><code>npm audit</code></td>
<td style="padding: 15px;">Check for security issues</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><code>npm audit fix</code></td>
<td style="padding: 15px;">Fix security issues</td>
</tr>
</table>

---

## 🔧 Bonus Tool: npm-check-updates

<div style="background-color: #e3f2fd; padding: 20px; border-radius: 10px;">

### Update ALL Packages to Latest

```bash
# Install globally
npm i -g npm-check-updates

# Check for updates
ncu

# Update package.json
ncu -u

# Then install
npm install
```

⚠️ **Use with caution** - updates to latest major versions!

</div>

---

## 🎯 Best Practices

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50;">

### Dependency Management

✅ **DO:** Add node_modules to .gitignore  
✅ **DO:** Commit package.json and package-lock.json  
✅ **DO:** Run `npm audit` regularly  
✅ **DO:** Test after updating dependencies  
✅ **DO:** Use `--save-dev` for development tools

❌ **DON'T:** Commit node_modules  
❌ **DON'T:** Update all packages without testing  
❌ **DON'T:** Ignore security warnings  
❌ **DON'T:** Mix global and local installations

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 3 Home](./README.md)

[← Previous: Semantic Versioning](./04-semantic-versioning.md) | [Next: Publishing Packages →](./06-publishing-packages.md)

</div>
