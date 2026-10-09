# 📞 Callbacks

## 🎯 Traditional Async Pattern

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Callback Functions

The original way to handle async operations

</div>

---

## 📖 What is a Callback?

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Definition

A **callback** is a function passed as an argument to another function, to be executed when an async operation completes.

```javascript
function doSomething(callback) {
    // Do async work...
    callback(result);  // Call the callback with result
}
```

### Why Callbacks?

Since async operations don't return immediately, we need a way to handle the result **when it's ready**.

</div>

---

## 🔧 Basic Callback Example

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Simple Callback

```javascript
console.log('Before');

getUser(1, function(user) {
    console.log('User', user);
});

console.log('After');

function getUser(id, callback) {
    setTimeout(() => {
        console.log('Reading a user from database...');
        callback({ id: id, gitHubUsername: 'MilanVives' });
    }, 2000);
}
```

**Output:**
```
Before
After
Reading a user from database...
User { id: 1, gitHubUsername: 'MilanVives' }
```

</div>

---

## 🔗 Chaining Async Operations

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Multiple Async Calls

Often we need to chain async operations:

1. Get user from database
2. Get repositories from GitHub
3. Get commits from repository

```javascript
function getRepositories(username) {
    return ['repo1', 'repo2', 'repo3'];
}
```

### With Callbacks

```javascript
function getRepositories(username, callback) {
    setTimeout(() => {
        console.log('Calling Github API...');
        callback(['repo1', 'repo2', 'repo3']);
    }, 2000);
}
```

</div>

---

## 🔄 Nested Callbacks

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Chaining Example

```javascript
console.log('Before');

getUser(1, function(user) {
    getRepositories(user.gitHubUsername, (repos) => {
        console.log('Repos', repos);
    });
});

console.log('After');

function getUser(id, callback) {
    setTimeout(() => {
        console.log('Reading a user from database...');
        callback({ id: id, gitHubUsername: 'MilanVives' });
    }, 2000);
}

function getRepositories(username, callback) {
    setTimeout(() => {
        console.log('Calling Github API...');
        callback(['repo1', 'repo2', 'repo3']);
    }, 2000);
}
```

**Output:**
```
Before
After
Reading a user from database...
Calling Github API...
Repos [ 'repo1', 'repo2', 'repo3' ]
```

</div>

---

## 🎄 Callback Hell (Christmas Tree Problem)

<div style="background-color: #ffebee; padding: 25px; border-radius: 10px; border-left: 5px solid #f44336; color: #1a1a1a;">

### The Problem

Adding more async operations creates deeply nested code:

```javascript
getUser(1, function(user) {
    getRepositories(user.gitHubUsername, (repos) => {
        getCommits(repos[0], (commits) => {
            console.log('Commits', commits);
        });
    });
});
```

### Even Worse with More Operations

```javascript
getUser(1, function(user) {
    getRepositories(user.gitHubUsername, (repos) => {
        getCommits(repos[0], (commits) => {
            getDetails(commits[0], (details) => {
                // Keep nesting... 🎄
            });
        });
    });
});
```

This is called **"Callback Hell"** or the **"Christmas Tree Problem"**!

</div>

---

## 🆚 Synchronous vs Callback Hell

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### How It Could Look (Synchronous)

```javascript
const user = getUser(1);
const repos = getRepositories(user.gitHubUsername);
const commits = getCommits(repos[0]);
console.log(commits);
```

✅ Clean, readable, easy to understand

### How It Actually Looks (Callbacks)

```javascript
getUser(1, function(user) {
    getRepositories(user.gitHubUsername, (repos) => {
        getCommits(repos[0], (commits) => {
            console.log(commits);
        });
    });
});
```

❌ Nested, harder to read, callback hell

</div>

---

## 📝 Complete Callback Hell Example

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Full Code

```javascript
console.log('1. Before');

getUser(1, function(user) {
    getRepositories(user.gitHubUsername, (repos) => {
        getCommits(repos[0], (commits) => {
            console.log('Commits', commits);
        });
    });
});

console.log('3. After');

function getUser(id, callback) {
    setTimeout(() => {
        console.log('2. Reading user from DB...');
        callback({ id: id, gitHubUsername: 'MilanVives' });
    }, 2000);
}

function getRepositories(username, callback) {
    setTimeout(() => {
        console.log('4. Calling Github API...');
        callback(['repo1', 'repo2', 'repo3']);
    }, 2000);
}

function getCommits(repo, callback) {
    setTimeout(() => {
        console.log('5. Getting commits...');
        callback(['commit1', 'commit2', 'commit3']);
    }, 2000);
}
```

</div>

---

## 🛠️ Solution 1: Named Functions

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Replace Anonymous with Named Functions

Instead of:
```javascript
getCommits(repos[0], (commits) => {
    console.log(commits);
});
```

Create named function:
```javascript
function displayCommits(commits) {
    console.log(commits);
}

getCommits(repos[0], displayCommits);
```

⚠️ **Note:** No `()` after `displayCommits`!

### Benefits

- Less nesting
- More readable
- Reusable functions
- Easier to debug

</div>

---

## 🎁 Solution 2: Promises (Preview)

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### A Better Way

```javascript
getUser(1)
    .then(user => getRepositories(user.gitHubUsername))
    .then(repos => getCommits(repos[0]))
    .then(commits => console.log('Commits', commits))
    .catch(err => console.log('Error', err.message));
```

✅ Flat structure  
✅ Easy to read  
✅ Better error handling

We'll learn about Promises in the next slide!

</div>

---

## 💡 Best Practices

<div style="background-color: #e3f2fd; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50; color: #1a1a1a;">

### Callback Guidelines

✅ **DO:**
- Use named functions to avoid deep nesting
- Handle errors in callbacks
- Keep callbacks simple and focused

❌ **DON'T:**
- Nest callbacks more than 2-3 levels deep
- Forget to handle errors
- Use callbacks for new code (use Promises instead!)

### Modern Recommendation

Use **Promises** or **async/await** instead of callbacks for new projects!

</div>

---

## 🎯 Key Takeaways

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 10px; color: #1a1a1a;">

### Callbacks Summary

- **Callback** = Function passed to handle async result
- Callbacks execute **after** async operation completes
- **Callback Hell** = Deeply nested callbacks
- Makes code hard to read and maintain
- **Named functions** can help reduce nesting
- **Promises** are the modern solution

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 6 Home](./README.md)

[← Previous: Sync vs Async](./01-sync-vs-async.md) | [Next: Promises →](./03-promises.md)

</div>
