# 🔬 Lab Assignment

## Practice Data Validation with MongoDB

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 2rem; border-radius: 10px; color: white; margin: 1rem 0;">
  <h3>🎯 Assignment</h3>
  <p>Apply what you've learned to extend the Library API with MongoDB persistence and validation</p>
</div>

---

<!-- lab:lab08-validation -->
## Lab 08: Data Validation and Persistence

Replace the in-memory arrays of the Vivesbib API with MongoDB and add a customers API with validation.

This lab continues where lab05-middleware ended; the starter contains a working version of that lab.

- **Starter template:** [MilanVives/node-lab-08-validation](https://github.com/MilanVives/node-lab-08-validation)
- **Accept the lab:** use the link on **your own group's page** in [Classroom 50 invites](../classroom50/README.md)

| Requirement | Points |
|---|---|
| R1: The Genre model validates its data | 2 |
| R2: GET genre endpoints read from MongoDB | 2 |
| R3: POST /api/genres saves to MongoDB | 2 |
| R4: PUT and DELETE genres change MongoDB | 1 |
| R5: The customers API works with MongoDB | 3 |

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

## 🎯 What You'll Practice

### 1. Schema Design
- Create Mongoose schemas with validation
- Define appropriate data types
- Set up constraints (required, min/max, etc.)

### 2. Model Creation
- Convert schemas to models
- Understand model-collection relationship
- Implement proper naming conventions

### 3. CRUD Implementation
- Refactor all endpoints to use MongoDB
- Implement async/await patterns
- Handle errors appropriately

### 4. Validation
- Use Mongoose validators
- Combine with Joi validation
- Create custom validation rules

---

## 💡 Tips for Success

<div style="background: #dbeafe; border-left: 4px solid #3b82f6; padding: 1rem; margin: 1rem 0;">
  <strong>📝 Plan First</strong>
  <ul>
    <li>Sketch your schema before coding</li>
    <li>List all required validations</li>
    <li>Consider field relationships</li>
  </ul>
</div>

<div style="background: #f0fdf4; border-left: 4px solid #10b981; padding: 1rem; margin: 1rem 0;">
  <strong>🧪 Test Thoroughly</strong>
  <ul>
    <li>Test happy paths</li>
    <li>Test validation errors</li>
    <li>Test edge cases</li>
  </ul>
</div>

<div style="background: #fef3c7; border-left: 4px solid #f59e0b; padding: 1rem; margin: 1rem 0;">
  <strong>🔍 Follow Patterns</strong>
  <ul>
    <li>Use the Genres route as a reference</li>
    <li>Keep consistent error messages</li>
    <li>Maintain code structure</li>
  </ul>
</div>

---

## 🛠️ Suggested Workflow

```mermaid
graph TD
    A[📖 Read Assignment] --> B[📝 Design Schema]
    B --> C[💾 Create Model]
    C --> D[🔄 Refactor GET]
    D --> E[➕ Refactor POST]
    E --> F[✏️ Refactor PUT]
    F --> G[🗑️ Refactor DELETE]
    G --> H[🧪 Test All Endpoints]
    H --> I[📚 Document Code]
    I --> J[🚀 Submit]
    
    style A fill:#3b82f6,stroke:#1e40af,color:#fff
    style J fill:#10b981,stroke:#059669,color:#fff
```

---

## 📚 Key Concepts to Apply

### Schema Definition
```javascript
const schema = new mongoose.Schema({
  field: {
    type: Type,
    required: true,
    // Add validators
  }
});
```

### Model Creation
```javascript
const Model = mongoose.model('ModelName', schema);
```

### Async Operations
```javascript
router.get('/', async (req, res) => {
  const items = await Model.find();
  res.send(items);
});
```

### Error Handling
```javascript
if (!item) 
  return res.status(404).send('Not found');
```

---

## ✅ Checklist Before Submission

- [ ] MongoDB connection established
- [ ] All schemas defined with validation
- [ ] All models created correctly
- [ ] GET all endpoint works
- [ ] GET by ID endpoint works
- [ ] POST endpoint creates documents
- [ ] PUT endpoint updates documents
- [ ] DELETE endpoint removes documents
- [ ] Validation works on all endpoints
- [ ] Error handling is consistent
- [ ] Code is clean and commented
- [ ] All endpoints tested
- [ ] REST Client file created (optional)

---

## 🎓 Learning Objectives Review

After completing this lab, you should be able to:

✅ Define schemas with built-in validators  
✅ Create and use Mongoose models  
✅ Implement CRUD operations with MongoDB  
✅ Handle asynchronous database operations  
✅ Validate data at multiple levels  
✅ Test RESTful APIs with persistence  

---

## 🆘 Getting Help

If you're stuck:

1. Review the chapter slides
2. Check the Genres route implementation
3. Review Mongoose documentation
4. Ask questions in the course forum
5. Attend office hours

---

## 🎉 Bonus Challenges

Once you've completed the basic assignment:

### 🌟 Advanced Validation
- Implement custom validators
- Add async validators
- Create conditional required fields

### 🌟 Advanced Queries
- Add filtering to GET endpoints
- Implement pagination
- Add sorting options

### 🌟 Advanced Error Handling
- Create custom error middleware
- Improve error messages
- Add logging

---

<div style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); padding: 2rem; border-radius: 10px; color: white; margin: 1rem 0; text-align: center;">
  <h3>🚀 Good Luck!</h3>
  <p>You've got all the tools you need. Time to build something great!</p>
</div>

---

[← Previous: Testing](09-testing.md) | [🏠 Home](../README.md) | [Next Chapter: Mongoose Modeling →](../09-Mongoose-Modeling/01-intro.md)
