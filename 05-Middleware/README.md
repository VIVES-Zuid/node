# 🔌 Chapter 5: Middleware Functions

## 🎯 Chapter Overview

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Master Express Middleware

Learn to use and create middleware functions

</div>

---

## 🤔 What You'll Learn

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 10px;">

This chapter covers:

- 🔌 **Middleware Concept** - Request processing pipeline
- 📦 **Built-in Middleware** - express.json(), static files
- 🛡️ **Third-party Middleware** - helmet, morgan
- 🌍 **Environments** - Development, test, production
- ⚙️ **Configuration** - Managing settings with config
- 🎨 **Templating** - Pug for dynamic HTML
- 📁 **Project Structure** - Organizing Express apps

</div>

---

## 🔄 What is Middleware?

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### The Request Pipeline

Middleware functions are functions that have access to:
- **Request object** (req)
- **Response object** (res)
- **Next middleware function** (next)

```javascript
app.use((req, res, next) => {
    console.log('Logging...');
    next(); // Pass control to next middleware
});
```

### Middleware Flow

```
Request → Middleware 1 → Middleware 2 → Route Handler → Response
```

Each middleware can:
- Execute code
- Modify req/res objects
- End the request-response cycle
- Call the next middleware

</div>

---

## 🗂️ Chapter Slides

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 10px;">

1. **[Middleware Basics](./01-middleware-basics.md)** - Understanding middleware
2. **[Built-in Middleware](./02-builtin-middleware.md)** - express.json, static files
3. **[Third-party Middleware](./03-third-party-middleware.md)** - helmet, morgan
4. **[Environments & Config](./04-environments-config.md)** - Managing settings
5. **[Templating & Structure](./05-templating-structure.md)** - Pug and organization

</div>

---

## 💡 Key Concepts

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e8f5e9;">
<th style="padding: 15px;">Concept</th>
<th style="padding: 15px;">Description</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>Middleware</strong></td>
<td style="padding: 15px;">Functions that process requests in a pipeline</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>Built-in</strong></td>
<td style="padding: 15px;">Middleware included with Express</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>Third-party</strong></td>
<td style="padding: 15px;">npm packages providing middleware</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>Custom</strong></td>
<td style="padding: 15px;">Your own middleware functions</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>Environment</strong></td>
<td style="padding: 15px;">Development, test, or production mode</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>Router</strong></td>
<td style="padding: 15px;">Mini-app for organizing routes</td>
</tr>
</table>

---

## 🎯 Learning Objectives

By the end of this chapter, you will:

<div style="background-color: #fff3e0; padding: 20px; border-radius: 10px;">

✅ Understand middleware concept and pipeline  
✅ Use built-in Express middleware  
✅ Install and configure third-party middleware  
✅ Work with different environments  
✅ Manage configuration files securely  
✅ Use templating engines like Pug  
✅ Structure large Express applications  
✅ Separate routes into modules

</div>

---

## 🛠️ Tools You'll Use

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Middleware Packages

- 📦 **express.json()** - Parse JSON bodies
- 📦 **express.urlencoded()** - Parse URL-encoded bodies
- 📦 **express.static()** - Serve static files
- 🛡️ **helmet** - Security headers
- 📊 **morgan** - HTTP request logger
- ⚙️ **config** - Configuration management
- 🎨 **pug** - Templating engine

**Install:**

```bash
npm i helmet morgan config pug
```

</div>

---

## 🚀 Let's Get Started!

<div style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

Ready to master middleware?

**[Start with Middleware Basics →](./01-middleware-basics.md)**

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [← Previous: Chapter 4](../04-Express-API/README.md) | [Next Chapter: Async JS →](../06-Async-JS/README.md)

</div>
