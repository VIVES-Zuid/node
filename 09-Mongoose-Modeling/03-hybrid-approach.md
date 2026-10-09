# 🎭 Hybrid Approach

> **Combining the best of both worlds**

---

## 🤝 What is a Hybrid Approach?

A **hybrid approach** combines referencing and embedding to optimize for both **performance** and **consistency**.

Store a **reference** (`_id`) so you can always fetch the full document, plus a **snapshot** of the fields you display most often so you don't have to `populate()` for every query.

### The Concept

```mermaid
graph LR
    A[Full Author Document] -->|Reference _id| B[Course]
    A -->|Selected Properties snapshot| B
    style A fill:#4299e1,stroke:#2c5282,color:#fff
    style B fill:#48bb78,stroke:#2f855a,color:#fff
```

---

## 💡 The Pattern

### Full Author Document (separate collection)

```javascript
// Complete author in Authors collection
{
  _id: ObjectId('64a1...'),
  name: 'Vives',
  bio: 'Educational institution...',
  website: 'https://vives.be',
  email: 'info@vives.be',
  address: { /* ... */ },
  socialMedia: { /* ... */ },
  // ... 50 other properties
}
```

### Course Document (hybrid)

```javascript
// Only the _id reference + the fields needed for a quick display
{
  _id: ObjectId('64b2...'),
  name: 'Node.js Course',
  author: {
    _id:  ObjectId('64a1...'),  // Reference — use to fetch full author when needed
    name: 'Vives'               // Snapshot — use for fast list/card display
  }
}
```

---

## 🎯 Benefits

### ⚡ Performance

- ✅ Quick access to frequently needed data (name)
- ✅ No need to `populate()` for simple list/card displays
- ✅ Single query for basic information

### 🔗 Flexibility

- ✅ Can still `populate()` the full document when needed
- ✅ Can update author details separately in the Author collection
- ✅ Maintains relationship integrity via the stored `_id`

---

## 📊 Use Cases

### 1️⃣ Snapshot in Time

Perfect for **order history** where you need to preserve data as it was at the moment of purchase:

```javascript
// Order document — product is a hybrid (ref + price snapshot)
{
  _id: ObjectId('o1'),
  date: '2024-02-09',
  product: {
    _id:     ObjectId('p1'),     // Reference to current product
    name:    'Node.js Book',     // Snapshot of name at purchase time
    price:   29.99,              // Price at time of order!
    version: '3rd Edition'       // Version at time of order!
  },
  quantity: 2
}
```

**Why?** If the product price later changes to €39.99, this order still correctly shows €29.99 — the price that was actually paid!

---

### 2️⃣ Frequently Displayed Properties

Perfect for **comments** that need to show basic user info without hitting the Users collection:

```javascript
// Comment document — author is a hybrid (ref + display snapshot)
{
  _id: ObjectId('c1'),
  text: 'Great course!',
  author: {
    _id:    ObjectId('u1'),  // Reference — use to link to full profile
    name:   'Milan D.',      // Snapshot — show in comment
    avatar: 'url...'         // Snapshot — show in comment
  },
  timestamp: '2024-02-09T10:30:00Z'
}
```

**Why?** Render a page of 100 comments with one query — no `populate()` needed!

---

### 3️⃣ Large Documents with Summary

Perfect for **course listings** where you only need a few instructor fields on the card:

```javascript
// Course document — instructor is a hybrid (ref + summary snapshot)
{
  _id: ObjectId('c101'),
  title: 'Advanced Node.js',
  instructor: {
    _id:    ObjectId('i456'),      // Reference to full instructor profile
    name:   'Prof. Smith',         // For course listing card
    title:  'Senior Developer',    // For course listing card
    rating: 4.8                    // For course listing card
    // Full profile has 50+ more properties — fetch on demand
  },
  price: 99.99
}
```

**Why?** Course listings load fast; full instructor profile loads only when the user clicks through!

---

## 🔄 Implementation Example

```javascript
const mongoose = require('mongoose');

const authorSchema = new mongoose.Schema({
  name:    String,
  bio:     String,
  website: String,
  email:   String,
  // ... 45 more properties
});
const Author = mongoose.model('Author', authorSchema);

const courseSchema = new mongoose.Schema({
  name: String,
  author: {
    _id: {                                       // Reference field
      type: mongoose.Schema.Types.ObjectId,
      ref:  'Author',
      required: true,
    },
    name: {                                      // Snapshot field
      type: String,
      required: true,
    },
  },
});
const Course = mongoose.model('Course', courseSchema);
```

---

## ✍️ Creating a Course

```javascript
async function createCourse(name, author) {
  const course = new Course({
    name,
    author: {
      _id:  author._id,   // Store the reference
      name: author.name,  // Store the snapshot
    },
  });

  await course.save();
  return course;
}

// Usage
const author = await Author.findById('64a1...');
await createCourse('Node.js Course', author);
```

---

## 🔍 Querying

### Quick Display (No Population Needed)

```javascript
// Fast query — author.name is already stored in the course document
const courses = await Course.find().select('name author.name');
// Returns: [{ name: 'Node.js Course', author: { name: 'Vives' } }]
```

### Full Author Details (When Needed)

```javascript
// Populate replaces author._id with the full Author document
const course = await Course
  .findById('64b2...')
  .populate('author._id');

// After populate, author._id holds the complete Author document
console.log(course.author._id.bio);     // 'Educational institution...'
console.log(course.author._id.website); // 'https://vives.be'
console.log(course.author.name);        // 'Vives' (snapshot still accessible)
```

---

## ⚖️ Trade-offs

| Aspect | Impact |
|--------|--------|
| **Storage** | 💾 Slightly more (duplicated snapshots) |
| **Query Speed** | ⚡ Much faster for common queries |
| **Consistency** | ⚠️ Snapshot may become stale if author updates their name |
| **Complexity** | 📊 Medium (must keep snapshot in sync when needed) |
| **Flexibility** | ✅ High (choose when to `populate()`) |

---

## 💭 When to Use Hybrid?

✅ **Use hybrid when:**
- The referenced document has many properties but you only need a few for most views
- You need historical accuracy (e.g., price/version at time of purchase)
- Read performance matters but you occasionally need the full document
- The snapshotted fields change infrequently

❌ **Avoid hybrid when:**
- The embedded snapshot fields must always reflect the latest value
- Snapshot properties change very frequently (requires syncing everywhere)
- Simplicity is more important than performance

---

[← Previous: Referencing & Embedding](02-referencing-embedding.md) | [🏠 Home](../README.md) | [Next: Referencing Documents →](04-referencing-documents.md)
