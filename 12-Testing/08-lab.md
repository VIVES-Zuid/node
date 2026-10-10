# 🧪 Lab: Testing a Todo API

## Chapter 12: Hands-On Practice

---

## 🎯 Lab Objectives

Build and test a complete Todo API with:

- ✅ Unit tests for business logic
- 🔗 Integration tests for routes
- 🎭 Mocking for external dependencies
- 📊 Code coverage reporting
- 🏆 Best practices implementation

---

<!-- lab:lab12-testing -->
## Lab 12: Testing

Write unit and integration tests for a Todo API with the Node.js test runner.

- **Starter template:** [MilanVives/node-lab-12-testing](https://github.com/MilanVives/node-lab-12-testing)
- **Accept the lab:** use the link on **your own group's page** in [Classroom 50 invites](../classroom50/README.md)

| Requirement | Points |
|---|---|
| R1: Your test suite passes | 2 |
| R2: Your tests cover at least 80% of the lines | 3 |
| R3: Your tests catch a bug in the Todo model | 2 |
| R4: Your tests catch a bug in the TodoService | 1 |
| R5: Your tests catch a bug in the routes | 2 |

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

The template already contains the Todo API below and a `package.json` with the scripts `test` and `test:coverage`. The rest of this page walks through the code and the tests you write.

---

## 📁 Project Structure

```
todo-api-testing/
├── src/
│   ├── models/
│   │   └── todo.js
│   ├── services/
│   │   └── todoService.js
│   ├── routes/
│   │   └── todoRoutes.js
│   ├── app.js
│   └── server.js
└── test/
    ├── unit/
    │   ├── todo.test.js
    │   └── todoService.test.js
    ├── integration/
    │   └── todoRoutes.test.js
    └── helpers.js
```

---

## 🏗️ Implementation: Todo Model

### src/models/todo.js

```javascript
export class Todo {
  constructor(title, description = '') {
    if (!title || title.trim() === '') {
      throw new Error('Title is required');
    }
    
    this.id = Date.now();
    this.title = title.trim();
    this.description = description.trim();
    this.completed = false;
    this.createdAt = new Date();
  }
  
  complete() {
    this.completed = true;
  }
  
  uncomplete() {
    this.completed = false;
  }
  
  update(updates) {
    if (updates.title !== undefined) {
      if (!updates.title || updates.title.trim() === '') {
        throw new Error('Title cannot be empty');
      }
      this.title = updates.title.trim();
    }
    
    if (updates.description !== undefined) {
      this.description = updates.description.trim();
    }
  }
}
```

---

## 🧪 Task 1: Unit Test the Model

### test/unit/todo.test.js

```javascript
import { describe, test } from 'node:test';
import assert from 'node:assert';
import { Todo } from '../../src/models/todo.js';

describe('Todo Model', () => {
  describe('constructor', () => {
    test('creates todo with title', () => {
      // TODO: Implement test
      // Hint: Create a todo and check title property
    });
    
    test('creates todo with title and description', () => {
      // TODO: Implement test
    });
    
    test('throws error when title is empty', () => {
      // TODO: Implement test
      // Hint: Use assert.throws()
    });
    
    test('trims whitespace from title', () => {
      // TODO: Implement test
    });
    
    test('sets completed to false by default', () => {
      // TODO: Implement test
    });
    
    test('generates unique id', () => {
      // TODO: Implement test
      // Hint: Create two todos and compare ids
    });
  });
  
  describe('complete', () => {
    test('sets completed to true', () => {
      // TODO: Implement test
    });
  });
  
  describe('uncomplete', () => {
    test('sets completed to false', () => {
      // TODO: Implement test
    });
  });
  
  describe('update', () => {
    test('updates title', () => {
      // TODO: Implement test
    });
    
    test('updates description', () => {
      // TODO: Implement test
    });
    
    test('throws error when updating to empty title', () => {
      // TODO: Implement test
    });
  });
});
```

---

## 🏗️ Implementation: Todo Service

### src/services/todoService.js

```javascript
export class TodoService {
  constructor() {
    this.todos = new Map();
  }
  
  create(todo) {
    this.todos.set(todo.id, todo);
    return todo;
  }
  
  findById(id) {
    const todo = this.todos.get(id);
    if (!todo) {
      throw new Error('Todo not found');
    }
    return todo;
  }
  
  findAll() {
    return Array.from(this.todos.values());
  }
  
  update(id, updates) {
    const todo = this.findById(id);
    todo.update(updates);
    return todo;
  }
  
  delete(id) {
    const exists = this.todos.has(id);
    if (!exists) {
      throw new Error('Todo not found');
    }
    this.todos.delete(id);
  }
  
  complete(id) {
    const todo = this.findById(id);
    todo.complete();
    return todo;
  }
}
```

---

## 🧪 Task 2: Unit Test the Service

### test/unit/todoService.test.js

```javascript
import { describe, test, beforeEach } from 'node:test';
import assert from 'node:assert';
import { TodoService } from '../../src/services/todoService.js';
import { Todo } from '../../src/models/todo.js';

describe('TodoService', () => {
  let service;
  
  beforeEach(() => {
    service = new TodoService();
  });
  
  describe('create', () => {
    test('adds todo to collection', () => {
      // TODO: Implement test
    });
  });
  
  describe('findById', () => {
    test('returns todo when found', () => {
      // TODO: Implement test
      // Hint: Create a todo first, then find it
    });
    
    test('throws error when not found', () => {
      // TODO: Implement test
    });
  });
  
  describe('findAll', () => {
    test('returns empty array when no todos', () => {
      // TODO: Implement test
    });
    
    test('returns all todos', () => {
      // TODO: Implement test
      // Hint: Create multiple todos
    });
  });
  
  describe('update', () => {
    test('updates todo properties', () => {
      // TODO: Implement test
    });
    
    test('throws error when todo not found', () => {
      // TODO: Implement test
    });
  });
  
  describe('delete', () => {
    test('removes todo from collection', () => {
      // TODO: Implement test
    });
    
    test('throws error when todo not found', () => {
      // TODO: Implement test
    });
  });
  
  describe('complete', () => {
    test('marks todo as completed', () => {
      // TODO: Implement test
    });
  });
});
```

---

## 🏗️ Implementation: Express App

### src/app.js

```javascript
import express from 'express';
import { TodoService } from './services/todoService.js';
import { Todo } from './models/todo.js';

export function createApp(todoService = new TodoService()) {
  const app = express();
  app.use(express.json());
  
  // GET /todos
  app.get('/todos', (req, res) => {
    const todos = todoService.findAll();
    res.json(todos);
  });
  
  // POST /todos
  app.post('/todos', (req, res) => {
    try {
      const { title, description } = req.body;
      const todo = new Todo(title, description);
      todoService.create(todo);
      res.status(201).json(todo);
    } catch (error) {
      res.status(400).json({ error: error.message });
    }
  });
  
  // GET /todos/:id
  app.get('/todos/:id', (req, res) => {
    try {
      const id = parseInt(req.params.id);
      const todo = todoService.findById(id);
      res.json(todo);
    } catch (error) {
      res.status(404).json({ error: error.message });
    }
  });
  
  // PUT /todos/:id
  app.put('/todos/:id', (req, res) => {
    try {
      const id = parseInt(req.params.id);
      const todo = todoService.update(id, req.body);
      res.json(todo);
    } catch (error) {
      res.status(404).json({ error: error.message });
    }
  });
  
  // DELETE /todos/:id
  app.delete('/todos/:id', (req, res) => {
    try {
      const id = parseInt(req.params.id);
      todoService.delete(id);
      res.status(204).send();
    } catch (error) {
      res.status(404).json({ error: error.message });
    }
  });
  
  // POST /todos/:id/complete
  app.post('/todos/:id/complete', (req, res) => {
    try {
      const id = parseInt(req.params.id);
      const todo = todoService.complete(id);
      res.json(todo);
    } catch (error) {
      res.status(404).json({ error: error.message });
    }
  });
  
  return app;
}
```

---

## 🧪 Task 3: Integration Tests

### test/integration/todoRoutes.test.js

```javascript
import { describe, test, beforeEach } from 'node:test';
import assert from 'node:assert';
import { createApp } from '../../src/app.js';
import { TodoService } from '../../src/services/todoService.js';

// Helper to make requests
async function request(app, method, path, body) {
  return new Promise((resolve) => {
    const req = {
      method,
      url: path,
      body,
      params: {},
      get: () => 'application/json'
    };
    
    const res = {
      statusCode: 200,
      headers: {},
      data: null,
      status(code) {
        this.statusCode = code;
        return this;
      },
      json(data) {
        this.data = data;
        resolve(this);
      },
      send() {
        resolve(this);
      }
    };
    
    // Mock Express behavior
    app._router.handle(req, res, () => {});
  });
}

describe('Todo Routes', () => {
  let app;
  let service;
  
  beforeEach(() => {
    service = new TodoService();
    app = createApp(service);
  });
  
  describe('POST /todos', () => {
    test('creates a new todo', async () => {
      // TODO: Implement test
      // Hint: Use supertest or your request helper
    });
    
    test('returns 400 when title is missing', async () => {
      // TODO: Implement test
    });
  });
  
  describe('GET /todos', () => {
    test('returns empty array when no todos', async () => {
      // TODO: Implement test
    });
    
    test('returns all todos', async () => {
      // TODO: Implement test
      // Hint: Create some todos first
    });
  });
  
  describe('GET /todos/:id', () => {
    test('returns todo by id', async () => {
      // TODO: Implement test
    });
    
    test('returns 404 when todo not found', async () => {
      // TODO: Implement test
    });
  });
  
  // Add more tests for PUT, DELETE, and POST /todos/:id/complete
});
```

---

## 🎯 Task 4: Test Helpers

### test/helpers.js

```javascript
import { Todo } from '../src/models/todo.js';

export function createTestTodo(overrides = {}) {
  return new Todo(
    overrides.title || 'Test Todo',
    overrides.description || 'Test Description'
  );
}

export function createMultipleTodos(count) {
  return Array.from({ length: count }, (_, i) => 
    createTestTodo({ title: `Todo ${i + 1}` })
  );
}
```

---

## ✅ Verification Checklist

After implementing all tests:

- [ ] All unit tests pass
- [ ] All integration tests pass
- [ ] Code coverage > 80%
- [ ] Tests are fast (< 1 second total)
- [ ] Tests are independent
- [ ] Edge cases are covered
- [ ] Error cases are tested

---

## 🚀 Bonus Challenges

### Challenge 1: Add Filtering

Add support for filtering todos by completion status:

```javascript
// GET /todos?completed=true
```

Write tests first (TDD approach)!

### Challenge 2: Add Sorting

Add support for sorting todos:

```javascript
// GET /todos?sort=createdAt&order=desc
```

### Challenge 3: Add Validation

Add input validation using a library like Joi or Zod.
Test validation errors.

---

## 📊 Expected Coverage Report

```
------------------------------------------
| File                    | Line % | Branch % |
------------------------------------------
| src/models/todo.js      | 100.00 | 100.00   |
| src/services/todoService| 100.00 | 100.00   |
| src/app.js              |  95.00 |  90.00   |
------------------------------------------
| All files               |  98.33 |  96.67   |
------------------------------------------
```

---

## 💡 Solution Tips

1. **Start with unit tests** - Test models and services first
2. **Use beforeEach** - Fresh state for each test
3. **Test helpers** - Reduce duplication
4. **Mock when needed** - Keep tests fast
5. **Test errors** - Don't skip unhappy paths

---

## 🎓 Learning Outcomes

After completing this lab, you should be able to:

- ✅ Write unit tests for business logic
- ✅ Write integration tests for APIs
- ✅ Use mocking effectively
- ✅ Measure and improve code coverage
- ✅ Follow testing best practices
- ✅ Organize test files logically
- ✅ Use test helpers and factories

---

[← Previous: Best Practices](./07-best-practices.md) | [🏠 Home](../README.md) | [Next: Chapter 13 →](../13-Deployment/01-introduction.md)
