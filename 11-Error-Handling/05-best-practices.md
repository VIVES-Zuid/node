# ✨ Error Handling Best Practices

> **Chapter 11: Production-Ready Error Handling**

---

## 📋 Overview

Following best practices ensures:
- 🛡️ **Application resilience**
- 🔍 **Better debugging**
- 🔒 **Security**
- 👥 **Good user experience**

---

## 🎯 Golden Rules

### 1. ⚠️ Always Handle Errors

```javascript
// ❌ BAD - Unhandled promise rejection
async function fetchData() {
  const response = await fetch('/api/data');
  return response.json();
}

// ✅ GOOD - Proper error handling
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    return await response.json();
  } catch (error) {
    logger.error('Failed to fetch data:', error);
    throw error;
  }
}
```

---

### 2. 🎨 Use Custom Error Classes

```javascript
// ✅ Create meaningful error hierarchy
class AppError extends Error {
  constructor(message, statusCode, isOperational = true) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = isOperational;
    Error.captureStackTrace(this, this.constructor);
  }
}

class ValidationError extends AppError {
  constructor(message, errors = {}) {
    super(message, 400);
    this.errors = errors;
  }
}

class NotFoundError extends AppError {
  constructor(resource, id) {
    super(`${resource} with id ${id} not found`, 404);
    this.resource = resource;
    this.resourceId = id;
  }
}

class DatabaseError extends AppError {
  constructor(message, originalError) {
    super(message, 500);
    this.originalError = originalError;
  }
}
```

---

### 3. 🔍 Distinguish Error Types

```javascript
// Operational errors (expected, can be handled)
class OperationalError extends AppError {
  constructor(message, statusCode) {
    super(message, statusCode, true);
  }
}

// Programming errors (bugs, should crash)
class ProgrammerError extends AppError {
  constructor(message) {
    super(message, 500, false);
  }
}

// Usage
function processPayment(amount, currency) {
  if (amount <= 0) {
    // Operational - bad input from user
    throw new OperationalError('Amount must be positive', 400);
  }
  
  if (typeof amount !== 'number') {
    // Programmer error - code bug
    throw new ProgrammerError('Amount must be a number');
  }
}
```

---

### 4. 📝 Provide Context in Errors

```javascript
// ❌ BAD - No context
throw new Error('Failed');

// ✅ GOOD - Rich context
throw new DatabaseError(
  `Failed to update user profile`,
  {
    userId: user.id,
    operation: 'UPDATE',
    table: 'users',
    timestamp: new Date(),
    originalError: err
  }
);

// ✅ EVEN BETTER - Use error factory
class ErrorFactory {
  static databaseError(operation, table, originalError, context = {}) {
    return new DatabaseError(
      `Database ${operation} failed on ${table}`,
      {
        operation,
        table,
        originalError,
        ...context,
        timestamp: new Date()
      }
    );
  }
}

throw ErrorFactory.databaseError('UPDATE', 'users', err, { userId: 123 });
```

---

## 🔒 Security Best Practices

### Don't Leak Sensitive Information

```javascript
// ❌ BAD - Exposing internal details
app.use((err, req, res, next) => {
  res.status(500).json({
    error: err.message,
    stack: err.stack,  // ❌ Stack trace in production!
    query: req.query,  // ❌ Might contain sensitive data!
    env: process.env   // ❌ Environment variables exposed!
  });
});

// ✅ GOOD - Safe error response
app.use((err, req, res, next) => {
  const isProd = process.env.NODE_ENV === 'production';
  
  // Log full details internally
  logger.error('Request failed', {
    error: err,
    stack: err.stack,
    url: req.originalUrl,
    method: req.method,
    ip: req.ip,
    userId: req.user?.id
  });
  
  // Send safe response to client
  res.status(err.statusCode || 500).json({
    success: false,
    error: {
      message: err.isOperational 
        ? err.message 
        : 'An error occurred',
      ...((!isProd) && { stack: err.stack }) // Only in dev
    }
  });
});
```

---

## 📊 Proper Error Logging

```javascript
// ✅ Structured logging
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ 
      filename: 'error.log', 
      level: 'error' 
    }),
    new winston.transports.File({ 
      filename: 'combined.log' 
    })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

// Log with context
function logError(error, context = {}) {
  logger.error({
    message: error.message,
    name: error.name,
    statusCode: error.statusCode,
    stack: error.stack,
    isOperational: error.isOperational,
    timestamp: new Date().toISOString(),
    ...context
  });
}

// Usage
try {
  await processPayment(orderId);
} catch (error) {
  logError(error, {
    orderId,
    userId: req.user.id,
    endpoint: req.originalUrl
  });
  throw error;
}
```

---

## 🔄 Handle Uncaught Exceptions

```javascript
// Uncaught exceptions (synchronous)
process.on('uncaughtException', (error) => {
  logger.error('UNCAUGHT EXCEPTION! 💥 Shutting down...', {
    error: error.message,
    stack: error.stack
  });
  
  // Give time to log, then exit
  process.exit(1);
});

// Unhandled promise rejections (asynchronous)
process.on('unhandledRejection', (reason, promise) => {
  logger.error('UNHANDLED REJECTION! 💥', {
    reason,
    promise
  });
  
  // Optionally exit the process
  // process.exit(1);
});

// Graceful shutdown
const server = app.listen(3000);

process.on('SIGTERM', () => {
  logger.info('SIGTERM received. Shutting down gracefully...');
  
  server.close(() => {
    logger.info('Process terminated');
    process.exit(0);
  });
  
  // Force shutdown after 10 seconds
  setTimeout(() => {
    logger.error('Forced shutdown after timeout');
    process.exit(1);
  }, 10000);
});
```

---

## 🧪 Input Validation Errors

```javascript
const Joi = require('joi');

// Validation schema
const userSchema = Joi.object({
  email: Joi.string().email().required(),
  age: Joi.number().integer().min(0).max(120).required(),
  name: Joi.string().min(2).max(50).required()
});

// Validation middleware
const validateUser = (req, res, next) => {
  const { error, value } = userSchema.validate(req.body, {
    abortEarly: false // Get all errors
  });
  
  if (error) {
    const errors = error.details.reduce((acc, err) => {
      acc[err.path[0]] = err.message;
      return acc;
    }, {});
    
    return next(new ValidationError('Validation failed', errors));
  }
  
  req.validatedData = value;
  next();
};

// Usage
app.post('/users', validateUser, asyncHandler(async (req, res) => {
  const user = await User.create(req.validatedData);
  res.status(201).json(user);
}));
```

---

## 🎯 Error Recovery Strategies

### Retry Pattern

```javascript
async function fetchWithRetry(url, maxRetries = 3, delay = 1000) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      return await response.json();
    } catch (error) {
      if (attempt === maxRetries) {
        throw new Error(`Failed after ${maxRetries} attempts: ${error.message}`);
      }
      
      logger.warn(`Attempt ${attempt} failed, retrying in ${delay}ms...`);
      await new Promise(resolve => setTimeout(resolve, delay));
      delay *= 2; // Exponential backoff
    }
  }
}
```

### Circuit Breaker Pattern

```javascript
class CircuitBreaker {
  constructor(threshold = 5, timeout = 60000) {
    this.threshold = threshold;
    this.timeout = timeout;
    this.failures = 0;
    this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    this.nextAttempt = Date.now();
  }
  
  async execute(fn) {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) {
        throw new Error('Circuit breaker is OPEN');
      }
      this.state = 'HALF_OPEN';
    }
    
    try {
      const result = await fn();
      this.onSuccess();
      return result;
    } catch (error) {
      this.onFailure();
      throw error;
    }
  }
  
  onSuccess() {
    this.failures = 0;
    this.state = 'CLOSED';
  }
  
  onFailure() {
    this.failures++;
    if (this.failures >= this.threshold) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + this.timeout;
      logger.warn('Circuit breaker opened');
    }
  }
}

// Usage
const breaker = new CircuitBreaker(5, 60000);

async function callExternalAPI(data) {
  return breaker.execute(async () => {
    const response = await fetch('/api/external', {
      method: 'POST',
      body: JSON.stringify(data)
    });
    return response.json();
  });
}
```

---

## 📈 Monitoring and Alerting

```javascript
// Error tracking service integration
const Sentry = require('@sentry/node');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0
});

// Error middleware with Sentry
app.use((err, req, res, next) => {
  // Log to console/file
  logError(err, {
    url: req.originalUrl,
    method: req.method,
    userId: req.user?.id
  });
  
  // Send to error tracking service
  if (err.isOperational === false || err.statusCode >= 500) {
    Sentry.captureException(err, {
      user: { id: req.user?.id },
      extra: {
        url: req.originalUrl,
        method: req.method,
        body: req.body
      }
    });
  }
  
  // Send response
  res.status(err.statusCode || 500).json({
    success: false,
    error: { message: err.message }
  });
});
```

---

## 💡 Summary Checklist

### ✅ Essential Practices

- [ ] Use custom error classes with meaningful hierarchy
- [ ] Distinguish operational vs programmer errors
- [ ] Handle all async errors (async/await, promises, callbacks)
- [ ] Implement centralized error middleware
- [ ] Log errors with context and structure
- [ ] Don't expose sensitive data in production
- [ ] Handle uncaught exceptions and rejections
- [ ] Validate input and return clear error messages
- [ ] Implement retry logic for transient failures
- [ ] Use circuit breakers for external services
- [ ] Monitor errors with tracking services
- [ ] Test error scenarios
- [ ] Document expected errors for API consumers

---

## 🎨 Complete Production Example

```javascript
// errors/index.js
class AppError extends Error {
  constructor(message, statusCode, isOperational = true) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = isOperational;
    Error.captureStackTrace(this, this.constructor);
  }
}

// middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  
  logger.error('Error occurred', {
    error: err.message,
    stack: err.stack,
    url: req.originalUrl,
    method: req.method,
    userId: req.user?.id,
    timestamp: new Date()
  });
  
  if (process.env.NODE_ENV === 'production') {
    if (err.isOperational) {
      res.status(err.statusCode).json({
        success: false,
        error: { message: err.message }
      });
    } else {
      res.status(500).json({
        success: false,
        error: { message: 'An error occurred' }
      });
    }
  } else {
    res.status(err.statusCode).json({
      success: false,
      error: {
        message: err.message,
        stack: err.stack
      }
    });
  }
};

// app.js
const express = require('express');
const app = express();

// Routes
app.use('/api', routes);

// 404 handler
app.use((req, res, next) => {
  next(new AppError(`Route ${req.originalUrl} not found`, 404));
});

// Error handler
app.use(errorHandler);

// Global error handlers
process.on('uncaughtException', (error) => {
  logger.error('UNCAUGHT EXCEPTION', { error });
  process.exit(1);
});

process.on('unhandledRejection', (reason) => {
  logger.error('UNHANDLED REJECTION', { reason });
});

module.exports = app;
```

---

<!-- lab:lab11-error-handling -->
## Lab 11: Error Handling

Replace ad-hoc error responses in the Vivesbib API with custom error classes and central error-handling middleware.

This lab continues where lab10-auth ended; the starter contains a working version of that lab.

- **Starter template:** [MilanVives/node-lab-11-error-handling](https://github.com/MilanVives/node-lab-11-error-handling)
- **Accept the lab:** use the link on **your own group's page** in [Classroom 50 invites](../classroom50/README.md)

| Requirement | Points |
|---|---|
| R1: Custom error classes | 2 |
| R2: asyncHandler forwards errors to Express | 2 |
| R3: The error middleware sends JSON errors | 2 |
| R4: Unknown routes return a JSON 404 | 2 |
| R5: The genres routes use the error classes | 2 |

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

[⬅️ Previous: Express Errors](./04-express-errors.md) | [🏠 Home](../README.md) | [➡️ Next Chapter: Testing](../12-Testing/01-introduction.md)
