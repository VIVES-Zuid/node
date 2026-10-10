# ⚡ Async/Await

## 🎯 Syntactic Sugar for Promises

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Clean Async Syntax

Make promises look synchronous

</div>

---

## 📖 What is Async/Await?

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Definition

**Async/Await** is syntactic sugar built on top of Promises that makes asynchronous code look and behave more like synchronous code.

### Benefits

✅ Cleaner, more readable code  
✅ Looks like synchronous code  
✅ Easier to understand  
✅ Similar to C# async/await  
✅ Built on promises (not a replacement!)

### Key Keywords

- `async` - Declares an async function
- `await` - Waits for a promise to resolve

</div>

---

## 🔄 From Promises to Async/Await

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### With Promises

```javascript
getUser(1)
    .then(user => getRepositories(user.gitHubUsername))
    .then(repos => getCommits(repos[0]))
    .then(commits => console.log('Commits', commits))
    .catch(err => console.log('Error', err.message));
```

### With Async/Await

```javascript
const user = await getUser(1);
const repos = await getRepositories(user.gitHubUsername);
const commits = await getCommits(repos[0]);
console.log(commits);
```

✨ Looks like synchronous code!

</div>

---

## 🏗️ Creating Async Functions

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### The async Keyword

`await` can only be used inside an `async` function:

```javascript
async function displayCommits() {
    const user = await getUser(1);
    const repos = await getRepositories(user.gitHubUsername);
    const commits = await getCommits(repos[0]);
    console.log(commits);
}

displayCommits();
```

### Rules

⚠️ `await` must be inside an `async` function  
⚠️ `async` functions always return a Promise

</div>

---

## 🔍 How Await Works

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Understanding await

```javascript
const user = await getUser(1);
```

**What await does:**
1. Waits for the promise to resolve
2. Gets the resolved value
3. Assigns it to the variable

### Without await

```javascript
const user = getUser(1);  // user is a Promise!
console.log(user);  // Promise { <pending> }
```

### With await

```javascript
const user = await getUser(1);  // user is the actual value!
console.log(user);  // { id: 1, gitHubUsername: 'MilanVives' }
```

</div>

---

## 📝 Complete Async/Await Example

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Full Code

```javascript
console.log('Before');

async function displayCommits() {
    const user = await getUser(1);
    const repos = await getRepositories(user.gitHubUsername);
    const commits = await getCommits(repos[0]);
    console.log(commits);
}

displayCommits();

console.log('After');

function getUser(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('Reading a user from database...');
            resolve({ id: id, gitHubUsername: 'MilanVives' });
        }, 2000);
    });
}

// ... other promise functions ...
```

**Output:**
```
Before
After
Reading a user from database...
Calling Github API...
Getting commits...
[ 'commit1', 'commit2', 'commit3' ]
```

</div>

---

## ❌ Error Handling with Try/Catch

<div style="background-color: #ffebee; padding: 25px; border-radius: 10px; border-left: 5px solid #f44336; color: #1a1a1a;">

### The Problem

With promises we had `.catch()`, but there's no `.catch()` with async/await!

### Solution: try/catch Block

```javascript
async function displayCommits() {
    try {
        const user = await getUser(1);
        const repos = await getRepositories(user.gitHubUsername);
        const commits = await getCommits(repos[0]);
        console.log(commits);
    }
    catch (err) {
        console.log('Error', err.message);
    }
}

displayCommits();
```

**Benefits:**
- Catches errors from any await
- Traditional error handling
- Clean and readable

</div>

---

## 🆚 Comparison: All Three Approaches

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Callbacks (Old Way)

```javascript
getUser(1, function(user) {
    getRepositories(user.gitHubUsername, (repos) => {
        getCommits(repos[0], (commits) => {
            console.log(commits);
        });
    });
});
```

### Promises (Better)

```javascript
getUser(1)
    .then(user => getRepositories(user.gitHubUsername))
    .then(repos => getCommits(repos[0]))
    .then(commits => console.log(commits))
    .catch(err => console.log(err));
```

### Async/Await (Best!)

```javascript
async function display() {
    try {
        const user = await getUser(1);
        const repos = await getRepositories(user.gitHubUsername);
        const commits = await getCommits(repos[0]);
        console.log(commits);
    } catch (err) {
        console.log(err);
    }
}
```

</div>

---

## 🎯 Async Functions Return Promises

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Important Fact

`async` functions **always** return a Promise!

```javascript
async function getName() {
    return 'Milan';
}

const result = getName();
console.log(result);  // Promise { 'Milan' }
```

### To Get the Value

```javascript
getName().then(name => console.log(name));  // Milan

// Or with await
const name = await getName();
console.log(name);  // Milan
```

</div>

---

## 🔄 Parallel Execution with Async/Await

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Sequential (Slow)

```javascript
const user = await getUser(1);        // Wait 2s
const repos = await getRepositories();  // Wait 2s
// Total: 4 seconds
```

### Parallel (Fast)

```javascript
const userPromise = getUser(1);
const reposPromise = getRepositories();

const user = await userPromise;
const repos = await reposPromise;
// Total: 2 seconds (both run simultaneously)
```

### Even Better with Promise.all()

```javascript
const [user, repos] = await Promise.all([
    getUser(1),
    getRepositories()
]);
```

</div>

---

## 💡 Best Practices

<div style="background-color: #e3f2fd; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50; color: #1a1a1a;">

### Async/Await Guidelines

✅ **DO:**
- Use async/await for new code
- Always wrap await in try/catch
- Use async functions
- Use Promise.all() for parallel operations
- Keep async functions focused

❌ **DON'T:**
- Use await in non-async functions
- Forget error handling
- Make everything sequential (use Promise.all!)
- Mix async/await with .then()/.catch()
- Use await in loops (use Promise.all instead)

</div>

---

## 🚨 Common Mistakes

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; border-left: 5px solid #ff9800; color: #1a1a1a;">

### Mistake 1: Forgetting async

```javascript
// ❌ ERROR!
function getData() {
    const result = await fetchData();  // SyntaxError!
}
```

```javascript
// ✅ CORRECT
async function getData() {
    const result = await fetchData();
}
```

### Mistake 2: No Error Handling

```javascript
// ❌ Errors will crash the app
async function getData() {
    const result = await fetchData();
}
```

```javascript
// ✅ Always handle errors
async function getData() {
    try {
        const result = await fetchData();
    } catch (err) {
        console.log(err);
    }
}
```

</div>

---

## 🎯 Chapter 6 Complete!

<div style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### What You've Learned

✅ Synchronous vs Asynchronous code  
✅ Callbacks and callback hell  
✅ Promises and promise chains  
✅ Creating and consuming promises  
✅ Async/await syntax  
✅ Error handling with try/catch  
✅ Parallel execution with Promise.all()

**Next:** Learn MongoDB for data persistence!

</div>

---

<!-- lab:lab06-async -->
## Lab 06: Asynchronous JavaScript

Rewrite callback-based code with promises and async/await.

- **Starter template:** [MilanVives/node-lab-06-async](https://github.com/MilanVives/node-lab-06-async)
- **Accept the lab:** use the link on **your own group's page** in [Classroom 50 invites](../classroom50/README.md)

| Requirement | Points |
|---|---|
| R1: getCustomer returns a promise | 2 |
| R2: getTopBooks returns a promise | 2 |
| R3: sendEmail returns a promise | 2 |
| R4: notifyCustomer uses async/await | 3 |
| R5: notifyCustomer skips customers who are not gold | 1 |

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

## Code from the Lessons

[MilanVives/NodeLes6](https://github.com/MilanVives/NodeLes6): practice converting callbacks to promises to async/await.


---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 6 Home](./README.md)

[← Previous: Promises](./03-promises.md) | [Next Chapter: MongoDB →](../07-MongoDB/README.md)

</div>
