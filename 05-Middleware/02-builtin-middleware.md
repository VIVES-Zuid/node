# 📦 Built-in Middleware

## 🎯 Express Built-in Middleware

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Essential Middleware Included with Express

No installation required!

</div>

---

## 📋 Built-in Middleware Overview

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Three Main Built-in Middleware

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e8f5e9;">
<th style="padding: 15px;">Middleware</th>
<th style="padding: 15px;">Purpose</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>express.json()</strong></td>
<td style="padding: 15px;">Parse JSON request bodies</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>express.urlencoded()</strong></td>
<td style="padding: 15px;">Parse URL-encoded form data</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>express.static()</strong></td>
<td style="padding: 15px;">Serve static files</td>
</tr>
</table>

All built into Express - no separate installation needed!

</div>

---

## 📝 express.json()

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Parsing JSON Request Bodies

Without this middleware, `req.body` is undefined!

```javascript
const express = require('express');
const app = express();

// Enable JSON parsing
app.use(express.json());

app.post('/api/courses', (req, res) => {
    console.log(req.body); // Now accessible!
    res.send(req.body);
});
```

### What It Does

- Parses incoming requests with JSON payloads
- Populates `req.body` with the parsed data
- Only parses requests with `Content-Type: application/json`

</div>

---

## 📄 express.urlencoded()

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Parsing HTML Form Data

For traditional HTML forms that POST data:

```javascript
app.use(express.urlencoded({ extended: true }));
```

### The `extended` Option

```javascript
// Simple parsing (using querystring library)
app.use(express.urlencoded({ extended: false }));

// Rich parsing (using qs library) - recommended
app.use(express.urlencoded({ extended: true }));
```

**Extended: true** allows:
- Nested objects
- Arrays
- Rich data structures

</div>

---

## 🧪 Testing express.json() with REST Client

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Create test.http

```http
### Test JSON parsing
POST http://localhost:3000/api/courses
Content-Type: application/json

{
    "name": "New Course"
}
```

### Server Code

```javascript
const express = require('express');
const app = express();

app.use(express.json());

app.post('/api/courses', (req, res) => {
    console.log(req.body);
    res.send(req.body);
});

app.listen(3000);
```

**Response:**
```json
{
    "name": "New Course"
}
```

</div>

---

## 📁 express.static()

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Serving Static Files

Serve CSS, JavaScript, images, and other static assets:

```javascript
app.use(express.static('public'));
```

### Directory Structure

```
project/
├── index.js
├── public/
│   ├── readme.txt
│   ├── style.css
│   └── app.js
```

### Example

**Create `public/readme.txt`:**
```
This is a readme file!
```

**Add to `index.js`:**
```javascript
app.use(express.static('public'));
```

**Access in browser:**
```
http://localhost:3000/readme.txt
```

⚠️ Note: `public/` is NOT in the URL!

</div>

---

## 🔍 How Static Files Work

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### URL Mapping

| File Path | URL |
|-----------|-----|
| `public/readme.txt` | `/readme.txt` |
| `public/css/style.css` | `/css/style.css` |
| `public/img/logo.png` | `/img/logo.png` |

### Multiple Static Directories

```javascript
app.use(express.static('public'));
app.use(express.static('files'));
```

Express looks in `public` first, then `files`.

### Virtual Path Prefix

```javascript
app.use('/static', express.static('public'));
```

Now files are accessed with `/static` prefix:
- `/static/readme.txt`
- `/static/css/style.css`

</div>

---

## 🎨 Practical Static File Example

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Serving a Website

**Directory:**
```
public/
├── index.html
├── css/
│   └── style.css
├── js/
│   └── app.js
└── images/
    └── logo.png
```

**index.js:**
```javascript
const express = require('express');
const app = express();

app.use(express.static('public'));

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

**Visit:** `http://localhost:3000/`  
→ Automatically serves `public/index.html`

</div>

---

## 🔧 Complete Setup Example

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### All Built-in Middleware Together

```javascript
const express = require('express');
const app = express();

// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies (HTML forms)
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));

// API endpoint
app.post('/api/data', (req, res) => {
    // req.body is now available
    res.send(req.body);
});

app.listen(3000, () => {
    console.log('Listening on port 3000...');
});
```

</div>

---

## 💡 Best Practices

<div style="background-color: #e3f2fd; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50;">

### Built-in Middleware Guidelines

✅ **DO:** Always use `express.json()` for APIs  
✅ **DO:** Use `urlencoded({ extended: true })` for forms  
✅ **DO:** Put middleware before route handlers  
✅ **DO:** Use `static()` for CSS, JS, images  
✅ **DO:** Organize static files in a public folder

❌ **DON'T:** Forget to enable JSON parsing for APIs  
❌ **DON'T:** Serve sensitive files with `static()`  
❌ **DON'T:** Put middleware after routes  
❌ **DON'T:** Use extended: false unless you have a reason

</div>

---

## 📊 Middleware Order Reminder

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Correct Order

```javascript
const express = require('express');
const app = express();

// 1. Built-in middleware first
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static('public'));

// 2. Then custom middleware
app.use(logger);

// 3. Then routes
app.get('/api/courses', (req, res) => {
    res.send(courses);
});

// 4. Error handling last
app.use((err, req, res, next) => {
    res.status(500).send('Something broke!');
});
```

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 5 Home](./README.md)

[← Previous: Middleware Basics](./01-middleware-basics.md) | [Next: Third-party Middleware →](./03-third-party-middleware.md)

</div>
