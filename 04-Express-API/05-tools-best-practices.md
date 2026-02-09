# 🔧 Tools & Best Practices

## 🎯 Professional Development Setup

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### Essential Tools & Techniques

Optimize your Express development workflow

</div>

---

## 🔄 nodemon - Auto-Restart Server

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### The Problem

Every time you change code, you must:
1. Stop the server (CTRL+C)
2. Restart: `node index.js`

This gets tedious quickly! ⏰

### The Solution: nodemon

Automatically restarts your server when files change!

### Installation

```bash
npm i -g nodemon
```

**Global installation** because it's a dev tool.

</div>

---

## 🚀 Using nodemon

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Start Your Server

Instead of:
```bash
node index.js
```

Use:
```bash
nodemon index.js
```

**Output:**
```
[nodemon] 2.0.7
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node index.js`
Listening on port 3000...
```

### Auto-Restart

When you save a file:
```
[nodemon] restarting due to changes...
[nodemon] starting `node index.js`
Listening on port 3000...
```

✅ No more manual restarts!

</div>

---

## 🌍 Environment Variables

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Don't Hardcode Configuration!

**❌ Bad Practice:**
```javascript
app.listen(3000, () => {
    console.log('Listening on port 3000...');
});
```

**✅ Better:**
```javascript
const port = process.env.PORT || 3000;
app.listen(port, () => {
    console.log(`Listening on port ${port}...`);
});
```

### Why?

- Different environments use different ports
- Production servers set `PORT` variable
- Flexible configuration
- Deployment ready

</div>

---

## 🔧 Setting Environment Variables

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### On Different Platforms

**Mac/Linux:**
```bash
export PORT=5000
nodemon index.js
```

**Windows CMD:**
```cmd
set PORT=5000
nodemon index.js
```

**Windows PowerShell:**
```powershell
$Env:PORT = 5000
nodemon index.js
```

### Result

```
Listening on port 5000...
```

</div>

---

## 🔒 Environment Variables Best Practices

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px; border-left: 5px solid #2196f3;">

### Important Notes

⚠️ **Environment variables are bound to a shell session!**

- A variable in one terminal won't exist in another
- Best practice: Set in VS Code's integrated terminal
- For persistent vars: Use `.bashrc`, `.zshrc`, or system settings

### Production Setup

Create a `.env` file (not committed to git!):

```
PORT=5000
DB_CONNECTION=mongodb://localhost/mydb
API_KEY=your-secret-key
```

Use a package like `dotenv`:
```bash
npm i dotenv
```

```javascript
require('dotenv').config();
const port = process.env.PORT || 3000;
```

</div>

---

## 🧪 Vivesbib API Lab

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 25px; border-radius: 15px; color: white;">

### Build Your First Real API

**Assignment:** Create an endpoint for book genres

**URL:** `https://bib.vivesapp.be/api/genres`

**Your Task:**
1. Create a new Express project
2. Set up routes for book genres
3. Implement GET, POST, PUT, DELETE
4. Add input validation
5. Test with Postman

**See:** GitHub Classroom - Lecture 4  
**Link:** On Toledo

</div>

---

## 📋 API Development Checklist

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Before You Start

- [ ] Install Express and Joi
- [ ] Install nodemon globally
- [ ] Install Postman or REST Client extension
- [ ] Create package.json
- [ ] Set up .gitignore

### For Each Endpoint

- [ ] Define the route
- [ ] Handle request parameters
- [ ] Validate input (if needed)
- [ ] Return appropriate status codes
- [ ] Test with Postman
- [ ] Handle error cases

</div>

---

## 💡 Express Best Practices

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50;">

### Code Organization

✅ **DO:** Use middleware for common tasks  
✅ **DO:** Extract validation functions  
✅ **DO:** Use environment variables  
✅ **DO:** Implement proper error handling  
✅ **DO:** Return consistent response formats

❌ **DON'T:** Hardcode configuration  
❌ **DON'T:** Skip input validation  
❌ **DON'T:** Use wrong HTTP status codes  
❌ **DON'T:** Expose sensitive error details  
❌ **DON'T:** Repeat validation logic

</div>

---

## 🎨 Response Format Standards

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Consistent API Responses

**Success Response:**
```json
{
    "success": true,
    "data": {
        "id": 1,
        "name": "course1"
    }
}
```

**Error Response:**
```json
{
    "success": false,
    "error": {
        "message": "Course not found",
        "code": 404
    }
}
```

### Benefits

- Predictable structure
- Easier for clients to parse
- Better error handling
- Professional API

</div>

---

## 📊 HTTP Status Codes Reference

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e8f5e9;">
<th style="padding: 15px;">Code</th>
<th style="padding: 15px;">Meaning</th>
<th style="padding: 15px;">When to Use</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>200</strong></td>
<td style="padding: 15px;">OK</td>
<td style="padding: 15px;">Successful GET, PUT, DELETE</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>201</strong></td>
<td style="padding: 15px;">Created</td>
<td style="padding: 15px;">Successful POST</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>400</strong></td>
<td style="padding: 15px;">Bad Request</td>
<td style="padding: 15px;">Validation failed</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>404</strong></td>
<td style="padding: 15px;">Not Found</td>
<td style="padding: 15px;">Resource doesn't exist</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>500</strong></td>
<td style="padding: 15px;">Server Error</td>
<td style="padding: 15px;">Something went wrong</td>
</tr>
</table>

---

## 🎯 Chapter 4 Complete!

<div style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### What You've Learned

✅ RESTful API principles  
✅ Express.js framework  
✅ CRUD operations (GET, POST, PUT, DELETE)  
✅ Route parameters and query strings  
✅ Input validation with Joi  
✅ Using Postman and nodemon  
✅ Environment variables

**Next:** Learn about middleware in Express!

</div>

---

## 📝 Assignment

<div style="background-color: #fce4ec; padding: 25px; border-radius: 10px;">

### GitHub Classroom Lab

Complete the Chapter 4 lab assignment:

🔗 **See Toledo for the GitHub Classroom link (Lecture 4)**

Build the Vivesbib API with book genres!

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 4 Home](./README.md)

[← Previous: PUT & DELETE](./04-put-delete-requests.md) | [Next Chapter: Middleware →](../05-Middleware/README.md)

</div>
