# 🍃 Chapter 7: MongoDB

## 🎯 Chapter Overview

<div style="background: linear-gradient(135deg, #00c853 0%, #64dd17 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

### NoSQL Database with MongoDB

Learn to persist data with MongoDB and Mongoose

</div>

---

## 🤔 What You'll Learn

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 10px; color: #1a1a1a;">

This chapter covers:

- 🗄️ **MongoDB Basics** - NoSQL database fundamentals
- 🔌 **Mongoose** - ODM (Object Document Mapper) for MongoDB
- 📋 **Schemas** - Define document structure
- 🏗️ **Models** - Create document classes
- ✏️ **CRUD Operations** - Create, Read, Update, Delete
- 🔍 **Querying** - Filter, sort, paginate data
- 🎯 **Advanced Queries** - Regex, comparison operators

</div>

---

## 🗄️ What is MongoDB?

<div style="background-color: #e8f5e9; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### NoSQL Document Database

**MongoDB** is a document-based NoSQL database that stores data in JSON-like documents.

### Key Concepts

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #c8e6c9;">
<th style="padding: 15px;">MongoDB</th>
<th style="padding: 15px;">Relational DB</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>Collection</strong></td>
<td style="padding: 15px;">Table</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>Document</strong></td>
<td style="padding: 15px;">Row/Record</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>Field</strong></td>
<td style="padding: 15px;">Column</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>No fixed schema</strong></td>
<td style="padding: 15px;">Fixed schema</td>
</tr>
</table>

</div>

---

## 🗂️ Chapter Slides

<div style="background-color: #f5f5f5; padding: 20px; border-radius: 10px; color: #1a1a1a;">

1. **[MongoDB & Mongoose Setup](./01-mongodb-setup.md)** - Connection and basics
2. **[Schemas & Models](./02-schemas-models.md)** - Define document structure
3. **[Creating Documents](./03-creating-documents.md)** - CRUD: Create
4. **[Reading Documents](./04-reading-documents.md)** - CRUD: Read & Queries
5. **[Updating Documents](./05-updating-documents.md)** - CRUD: Update
6. **[Deleting Documents](./06-deleting-documents.md)** - CRUD: Delete

</div>

---

## 💡 Key Concepts

<table style="width: 100%; border-collapse: collapse;">
<tr style="background-color: #c8e6c9;">
<th style="padding: 15px;">Concept</th>
<th style="padding: 15px;">Description</th>
</tr>
<tr>
<td style="padding: 15px;"><strong>Mongoose</strong></td>
<td style="padding: 15px;">ODM library for MongoDB and Node.js</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>Schema</strong></td>
<td style="padding: 15px;">Template that defines document structure</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>Model</strong></td>
<td style="padding: 15px;">Class compiled from schema to create documents</td>
</tr>
<tr style="background-color: #f5f5f5;">
<td style="padding: 15px;"><strong>Document</strong></td>
<td style="padding: 15px;">Instance of a model (like an object)</td>
</tr>
<tr>
<td style="padding: 15px;"><strong>ObjectID</strong></td>
<td style="padding: 15px;">Unique identifier assigned by MongoDB</td>
</tr>
</table>

---

## 🎯 Learning Objectives

By the end of this chapter, you will:

<div style="background-color: #fff3e0; padding: 20px; border-radius: 10px; color: #1a1a1a;">

✅ Understand NoSQL vs SQL databases  
✅ Install and connect to MongoDB  
✅ Use Mongoose as an ODM  
✅ Create schemas to define document structure  
✅ Build models from schemas  
✅ Perform CRUD operations (Create, Read, Update, Delete)  
✅ Query documents with filters and conditions  
✅ Use pagination for large datasets  
✅ Apply comparison and logical operators  
✅ Use regular expressions for text search

</div>

---

## 🛠️ CRUD Operations

<div style="background-color: #e3f2fd; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### The Four Basic Operations

- **Create** 📝 - Insert new documents
  - `.save()`
  
- **Read** 📖 - Query documents
  - `.find()`, `.findById()`
  
- **Update** ✏️ - Modify existing documents
  - `.update()`, `.findByIdAndUpdate()`
  
- **Delete** 🗑️ - Remove documents
  - `.deleteOne()`, `.deleteMany()`

</div>

---

## 🔍 Query Operators

<div style="background-color: #f5f5f5; padding: 25px; border-radius: 10px; color: #1a1a1a;">

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `$eq` | Equal |
| `$ne` | Not equal |
| `$gt` | Greater than |
| `$gte` | Greater than or equal |
| `$lt` | Less than |
| `$lte` | Less than or equal |
| `$in` | In array |
| `$nin` | Not in array |

### Logical Operators

| Operator | Meaning |
|----------|---------|
| `$and` | All conditions must be true |
| `$or` | At least one condition must be true |
| `$not` | Inverts condition |
| `$nor` | All conditions must be false |

</div>

---

## 🚀 Let's Get Started!

<div style="background: linear-gradient(135deg, #00c853 0%, #64dd17 100%); padding: 30px; border-radius: 15px; color: white; text-align: center;">

Ready to master MongoDB?

**[Start with MongoDB Setup →](./01-mongodb-setup.md)**

</div>

---

<div style="text-align: center; padding: 20px; color: #666;">

[🏠 Course Home](../README.md) | [← Previous: Chapter 6](../06-Async-JS/README.md) | [Next Chapter: Data Validation →](../08-Data-Validation/README.md)

</div>
