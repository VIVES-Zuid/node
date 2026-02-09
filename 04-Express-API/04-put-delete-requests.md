# 🔄 PUT & DELETE Requests

## 🎯 Updating and Deleting Resources

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### PUT & DELETE - Modify and Remove

Complete CRUD operations

</div>

---

## 📝 PUT vs PATCH

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Understanding the Difference

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e8f5e9;">
<th style="padding: 15px;">Method</th>
<th style="padding: 15px;">Purpose</th>
<th style="padding: 15px;">Usage</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>PUT</strong></td>
<td style="padding: 15px;">Replace entire resource</td>
<td style="padding: 15px;">Update all fields</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>PATCH</strong></td>
<td style="padding: 15px;">Partial update</td>
<td style="padding: 15px;">Update specific fields</td>
</tr>
</table>

### In Practice

```javascript
// PUT - Replace entire course
PUT /api/courses/1
{ "id": 1, "name": "Updated Course", "duration": 10 }

// PATCH - Update just the name
PATCH /api/courses/1
{ "name": "Updated Course" }
```

</div>

---

## 🔄 PUT Request Steps

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Implementation Steps

1. **Look up the resource** (by ID)
2. **Return 404** if not found
3. **Validate input** with Joi
4. **Return 400** if validation fails
5. **Update the resource**
6. **Return updated resource**

### Reusing Code

Steps 1-4 are similar to GET and POST!  
→ We can create a `validateCourse()` function

</div>

---

## ✅ Refactoring: Validation Function

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Extract Validation Logic

```javascript
function validateCourse(course) {
    const schema = Joi.object({
        name: Joi.string().min(3).required()
    });
    
    return schema.validate(course);
}
```

Now reuse in both POST and PUT!

### Benefits

✅ DRY (Don't Repeat Yourself)  
✅ Easier to maintain  
✅ Consistent validation  
✅ Single source of truth

</div>

---

## 🔄 Complete PUT Implementation

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Full PUT Route

```javascript
app.put('/api/courses/:id', (req, res) => {
    // 1. Look up course
    const course = courses.find(c => c.id === parseInt(req.params.id));
    
    // 2. Return 404 if not found
    if (!course) {
        return res.status(404).send('Course with given ID not found');
    }
    
    // 3. Validate input
    const result = validateCourse(req.body);
    
    // 4. Return 400 if validation fails
    if (result.error) {
        return res.status(400).send(result.error.details[0].message);
    }
    
    // 5. Update course
    course.name = req.body.name;
    
    // 6. Return updated course
    res.send(course);
});
```

</div>

---

## 🧪 Testing PUT with Postman

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### Update a Course

**Method:** PUT  
**URL:** `http://localhost:3000/api/courses/1`  
**Body:** raw → JSON

```json
{
    "name": "Updated Course Name"
}
```

**Click Send**

**Response (200 OK):**
```json
{
    "id": 1,
    "name": "Updated Course Name"
}
```

### Test Error Cases

**Non-existent ID (404):**
```
PUT /api/courses/10
→ "Course with given ID not found"
```

**Invalid input (400):**
```json
{ "name": "ab" }
→ "name" length must be at least 3 characters long
```

</div>

---

## 🗑️ DELETE Request

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### DELETE Steps

1. **Look up the resource** (by ID)
2. **Return 404** if not found
3. **Delete the resource**
4. **Return deleted resource** (convention)

### Implementation

```javascript
app.delete('/api/courses/:id', (req, res) => {
    // 1. Look up course
    const course = courses.find(c => c.id === parseInt(req.params.id));
    
    // 2. Return 404 if not found
    if (!course) {
        return res.status(404).send('Course with given ID not found');
    }
    
    // 3. Delete course
    const index = courses.indexOf(course);
    courses.splice(index, 1);
    
    // 4. Return deleted course
    res.send(course);
});
```

</div>

---

## �� Understanding Array Methods

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px;">

### Array.indexOf()

Returns the position of an item:

```javascript
const courses = [
    { id: 1, name: 'course1' },
    { id: 2, name: 'course2' },
    { id: 3, name: 'course3' }
];

const course = courses.find(c => c.id === 2);
const index = courses.indexOf(course);  // Returns 1
```

### Array.splice()

Removes items from array:

```javascript
courses.splice(index, 1);
// splice(startIndex, deleteCount)
```

**Before:** `[{id:1}, {id:2}, {id:3}]`  
**After:** `[{id:1}, {id:3}]`

</div>

---

## 🧪 Testing DELETE with Postman

<div style="background-color: #fff3e0; padding: 25px; border-radius: 10px;">

### Delete a Course

**Method:** DELETE  
**URL:** `http://localhost:3000/api/courses/3`  

**Click Send**

**Response (200 OK):**
```json
{
    "id": 3,
    "name": "course3"
}
```

### Test Not Found

**URL:** `http://localhost:3000/api/courses/10`

**Response (404 Not Found):**
```
Course with given ID not found
```

</div>

---

## 📊 Complete API Summary

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px;">

### All CRUD Operations

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #e8f5e9;">
<th style="padding: 15px;">Method</th>
<th style="padding: 15px;">Endpoint</th>
<th style="padding: 15px;">Action</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>GET</strong></td>
<td style="padding: 15px;">/api/courses</td>
<td style="padding: 15px;">Get all courses</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>GET</strong></td>
<td style="padding: 15px;">/api/courses/:id</td>
<td style="padding: 15px;">Get one course</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>POST</strong></td>
<td style="padding: 15px;">/api/courses</td>
<td style="padding: 15px;">Create course</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>PUT</strong></td>
<td style="padding: 15px;">/api/courses/:id</td>
<td style="padding: 15px;">Update course</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>DELETE</strong></td>
<td style="padding: 15px;">/api/courses/:id</td>
<td style="padding: 15px;">Delete course</td>
</tr>
</table>

</div>

---

## 🔧 Full API Code

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px;">

### Complete Implementation

```javascript
const express = require('express');
const Joi = require('joi');
const app = express();

app.use(express.json());

const courses = [
    { id: 1, name: 'course1' },
    { id: 2, name: 'course2' },
    { id: 3, name: 'course3' }
];

// GET all
app.get('/api/courses', (req, res) => {
    res.send(courses);
});

// GET one
app.get('/api/courses/:id', (req, res) => {
    const course = courses.find(c => c.id === parseInt(req.params.id));
    if (!course) return res.status(404).send('Not found');
    res.send(course);
});

// POST
app.post('/api/courses', (req, res) => {
    const result = validateCourse(req.body);
    if (result.error) return res.status(400).send(result.error.details[0].message);
    
    const course = { id: courses.length + 1, name: req.body.name };
    courses.push(course);
    res.send(course);
});

// PUT
app.put('/api/courses/:id', (req, res) => {
    const course = courses.find(c => c.id === parseInt(req.params.id));
    if (!course) return res.status(404).send('Not found');
    
    const result = validateCourse(req.body);
    if (result.error) return res.status(400).send(result.error.details[0].message);
    
    course.name = req.body.name;
    res.send(course);
});

// DELETE
app.delete('/api/courses/:id', (req, res) => {
    const course = courses.find(c => c.id === parseInt(req.params.id));
    if (!course) return res.status(404).send('Not found');
    
    const index = courses.indexOf(course);
    courses.splice(index, 1);
    res.send(course);
});

function validateCourse(course) {
    const schema = Joi.object({ name: Joi.string().min(3).required() });
    return schema.validate(course);
}

app.listen(3000, () => console.log('Listening on port 3000...'));
```

</div>

---

## 💡 Best Practices

<div style="background-color: #e8f5e9; padding: 20px; border-radius: 10px; border-left: 5px solid #4caf50;">

### PUT & DELETE Guidelines

✅ **DO:** Return 404 for non-existent resources  
✅ **DO:** Validate input on PUT  
✅ **DO:** Return the affected resource  
✅ **DO:** Reuse validation functions  
✅ **DO:** Use early returns for errors

❌ **DON'T:** Allow deleting without finding first  
❌ **DON'T:** Skip validation on updates  
❌ **DON'T:** Forget to remove from array (DELETE)  
❌ **DON'T:** Use wrong HTTP status codes

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [📘 Chapter 4 Home](./README.md)

[← Previous: POST Requests](./03-post-requests.md) | [Next: Tools & Best Practices →](./05-tools-best-practices.md)

</div>
