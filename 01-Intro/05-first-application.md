# 🚀 Your First Node.js Application

## 🏗️ Project Setup

<div style="background-color: #e3f2fd; padding: 20px; border-radius: 10px;">

### Creating Your Project

Let's create your first Node.js application step by step:

```bash
# Create a new directory for your project
mkdir first-app

# Navigate into the directory
cd first-app

# Open VS Code in the current directory
code .
```

</div>

---

## 📝 Creating app.js

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px; border-left: 5px solid #4caf50;">

### Step 1: Create the File

In VS Code, create a new file called `app.js`

### Step 2: Write Your First Code

```javascript
function sayHello(name) {
    console.log('Hello ' + name);
}

sayHello('Vives');
```

### Step 3: Run Your Application

Open the integrated terminal (Ctrl + \`) and run:

```bash
node app.js
```

### Expected Output

```bash
milan@first-app〽 node app.js
Hello Vives
milan@first-app〽
```

</div>

---

## 🎉 Congratulations!

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### You Just Ran Your First Node.js Application! 🎊

This simple program demonstrates that you can run JavaScript **outside the browser** using Node.js!

</div>

---

## 🧪 Try This Experiment

<div style="background-color: #fff3e0; padding: 20px; border-radius: 10px; border-left: 5px solid #ff9800;">

### Testing Browser-Specific Objects

Add this line to your `app.js`:

```javascript
console.log(window);
```

**Now run the application again:**

```bash
node app.js
```

### What Happens?

```bash
ReferenceError: window is not defined
```

**Why?** 🤔

The `window` object only exists in **browsers**, not in Node.js!

This proves that Node.js is a **different runtime environment** from the browser.

</div>

---

## 🔍 Understanding the Difference

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e3f2fd;">
<th style="padding: 15px;">🌐 Browser Environment</th>
<th style="padding: 15px;">🟢 Node.js Environment</th>
</tr>
<tr>
<td style="padding: 15px; background-color: #fff;">

**Available Objects:**
```javascript
window
document
navigator
localStorage
```

**Purpose:**
- DOM manipulation
- User interactions
- Web APIs

</td>
<td style="padding: 15px; background-color: #fff;">

**Available Objects:**
```javascript
process
global
module
require
fs (file system)
```

**Purpose:**
- File operations
- Server creation
- System interactions

</td>
</tr>
</table>

---

## 💡 Code Explanation

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 10px;">

Let's break down the code:

```javascript
// 1. Function Declaration
function sayHello(name) {
    // 2. Output to console
    console.log('Hello ' + name);
}

// 3. Function Call
sayHello('Vives');
```

**Line by Line:**

| Line | What It Does |
|------|--------------|
| `function sayHello(name)` | Declares a function named `sayHello` that accepts one parameter |
| `console.log('Hello ' + name)` | Prints text to the console (terminal) |
| `sayHello('Vives')` | Calls the function with the argument `'Vives'` |

</div>

---

## 🎨 Making It Better

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px;">

### Modern JavaScript Syntax

Let's improve our code with **ES6+ features**:

```javascript
// Using arrow function
const sayHello = (name) => {
    console.log(`Hello ${name}!`); // Template literal
}

sayHello('Vives');

// Even shorter!
const greet = name => console.log(`Hello ${name}!`);
greet('Node.js');
```

### With JSDoc Documentation

```javascript
/**
 * Prints a greeting message to the console
 * @param {string} name - The name to greet
 */
function sayHello(name) {
    console.log(`Hello ${name}!`);
}

sayHello('Vives');
```

</div>

---

<!-- lab:lab01-first-app -->
## Lab 01: First Application

Write your first Node.js program: assign students to one of seven groups based on their date of birth.

- **Starter template:** [MilanVives/node-lab-01-first-app](https://github.com/MilanVives/node-lab-01-first-app)
- **Accept the lab:** use the link on **your own group's page** in [Classroom 50 invites](../classroom50/README.md)

| Requirement | Points |
|---|---|
| R1: groups.js prints the group for a date of birth | 4 |
| R2: groups.js prints the quote of the day for that group | 3 |
| R3: groups.js rejects an invalid date of birth | 3 |

The README in your repository explains every requirement in detail.

**Work locally**

```bash
npm install
npm test                                     # all requirements
npm test -- --test-name-pattern="^R1:"       # one requirement
```

**Submit:** commit your work, then run `npm run submit`. You can submit as often as you like; the latest submission counts.

**Feedback:** after a few minutes the **Releases** page of your repository shows your score and a PASS/FAIL line per requirement with the reason for each failure. Your teacher can add comments in the **Feedback** pull request.
<!-- /lab -->

---

## 💡 Learning Points

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px;">

### What You've Learned

✅ **Running Node.js applications** from the command line  
✅ **Understanding Node.js runtime** vs browser environment  
✅ **Basic JavaScript syntax** in Node.js  
✅ **User input handling** with readline module  
✅ **Modulus operator** for mathematical operations  
✅ **Control flow** with switch statements

</div>

---

## 🔧 Debugging Tips

<div style="background-color: #fff3e0; padding: 20px; border-radius: 10px; border-left: 5px solid #ff9800;">

### Common Issues & Solutions

| Problem | Solution |
|---------|----------|
| `node: command not found` | Node.js not installed or not in PATH |
| `Cannot find module 'readline'` | This is a core module, no installation needed |
| `ReferenceError: variable is not defined` | Check variable spelling and declaration |
| Output not showing | Make sure you're calling the function |

### Using VS Code Debugger

1. Set a breakpoint (click left of line number)
2. Press `F5` to start debugging
3. Step through code with `F10` (Step Over)
4. Inspect variables in Debug panel

</div>

---

## 🎯 Challenge Yourself

<div style="background: linear-gradient(135deg, #fa709a 0%, #fee140 100%); padding: 25px; border-radius: 15px; color: white;">

### Additional Exercises

Try these to practice:

1. **Modify the program** to use groups 1-7 instead of 0-6
2. **Add validation** to ensure date is in correct format
3. **Create a function** that checks if the date is valid
4. **Add colors** to console output using ANSI codes
5. **Save group assignments** to a file using the `fs` module

</div>

---

## 📚 Additional Resources

<div style="background-color: #e3f2fd; padding: 20px; border-radius: 10px;">

### Want to Learn More?

- 📖 **Node.js Documentation**: [nodejs.org/docs](https://nodejs.org/docs/)
- 🎥 **Node.js Tutorial for Beginners**: YouTube search
- 💻 **Practice**: Try modifying the examples
- 👥 **Forum**: Ask questions on Toledo

</div>

---

## ✅ Completion Checklist

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 10px;">

Before moving to the next chapter:

- [ ] ✅ Created and ran `app.js` successfully
- [ ] ✅ Understood why `window` is not available in Node.js
- [ ] ✅ Completed the basic lab (group assignment)
- [ ] ✅ (Optional) Completed Extra 1 with user input
- [ ] ✅ (Optional) Completed Extra 2 with switch statement
- [ ] ✅ Pushed your code to GitHub

</div>

---

## 🎊 Well Done!

<div style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### You've Completed Chapter 1! 🎉

You now understand:
- ✅ What Node.js is and why it's powerful
- ✅ How Node.js works (architecture & async)
- ✅ How to install and set up your environment
- ✅ How to create and run Node.js applications

**Next Chapter:** We'll dive deeper into Node.js modules and npm! 🚀

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 1 Home](./README.md)

[← Previous: Installation & Setup](./04-installation-setup.md) | [Next Chapter: Node Modules →](../02-Node-Module-System/README.md)

</div>

