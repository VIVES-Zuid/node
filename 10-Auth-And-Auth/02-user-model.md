# User Model & Registration

## Step 1: Create the User Model

Create `models/user.js`:

```javascript
const Joi = require('joi');
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    minlength: 5,
    maxlength: 50
  },
  email: {
    type: String,
    required: true,
    minlength: 5,
    maxlength: 255,
    unique: true
  },
  password: {
    type: String,
    required: true,
    minlength: 5,
    maxlength: 1024
  }
});

const User = mongoose.model('User', userSchema);

function validateUser(user) {
  const schema = Joi.object({
    name: Joi.string().min(5).max(50).required(),
    email: Joi.string().min(5).max(255).email().required(),
    password: Joi.string().min(5).max(255).required()
  });
  return schema.validate(user);
}

exports.User = User;
exports.validate = validateUser;
```

> **Joi note:** from version 17 onward use `Joi.object({...})`. Earlier versions used `Joi.validate(user, schema)`.

---

## Step 2: Create the Register Route

Create `routes/users.js`:

```javascript
const { User, validate } = require('../models/user');
const mongoose = require('mongoose');
const express = require('express');
const router = express.Router();

router.post('/', async (req, res) => {
  const { error } = validate(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  let user = await User.findOne({ email: req.body.email });
  if (user) return res.status(400).send('User already registered.');

  user = new User({
    name: req.body.name,
    email: req.body.email,
    password: req.body.password
  });
  await user.save();
  res.send(user);
});

module.exports = router;
```

---

## Step 3: Register the Route in index.js

In `index.js`, import and use the users router:

```javascript
const users = require('./routes/users');
// ...
app.use('/api/users', users);
```

---

## Step 4: Test with REST Client

Create `api_calls/api_calls_users.http`:

```http
# Users CALLS
@base_URL=http://localhost:3000/api/users

### Register a new user
POST {{base_URL}}
Content-Type: application/json

{
    "name": "Vives",
    "email": "student@vives.be",
    "password": "12345"
}
```

You should get back the user object including the password in plaintext — we'll fix that next.

---

## Step 5: Filter the Response with lodash

We don't want to return the password to the client. Two options:

- Manually specify: `res.send({ name: user.name, email: user.email });`
- Use **lodash** — a utility library with a `pick` method:

```bash
npm i lodash
```

In `routes/users.js`:

```javascript
const _ = require('lodash');

// ...
user = new User(_.pick(req.body, ['name', 'email', 'password']));
await user.save();
res.send(_.pick(user, ['_id', 'name', 'email']));
```

`_.pick(object, keys)` selects only the listed keys — handy for both reading from `req.body` and filtering what we send back.

---

## Step 6: Hash Passwords with bcrypt

Right now passwords are stored as **plaintext** in the database — a serious security risk. We use **bcrypt** to hash them.

```bash
npm i bcrypt
```

### How bcrypt works

To avoid recognizable hashes for common passwords, bcrypt adds a **salt** — a random string appended to the password before hashing.

Test it in a temporary file `hash.js`:

```javascript
const bcrypt = require('bcrypt');

async function run() {
  const salt = await bcrypt.genSalt(10); // 10 rounds
  const hashed = await bcrypt.hash('1234', salt);
  console.log(salt);
  console.log(hashed);
}
run();
```

Running `node hash.js` outputs something like:
```
$2b$10$CU0KWo9xy/uI6s5xB1QeVu
$2b$10$CU0KWo9xy/uI6s5xB1QeVuiRk3QfYk6w/p93pdeGNLIO7H682XDNq
```

### Apply bcrypt in the register route

In `routes/users.js`, import bcrypt and hash the password before saving:

```javascript
const bcrypt = require('bcrypt');

// ...inside router.post('/'):
user = new User(_.pick(req.body, ['name', 'email', 'password']));

const salt = await bcrypt.genSalt(10);
user.password = await bcrypt.hash(user.password, salt);

await user.save();
res.send(_.pick(user, ['_id', 'name', 'email']));
```

Test again — the password in MongoDB Compass should now be a long bcrypt hash like `$2b$10$JJCvJDlxQ6faQJx6BQ6RBOUHpLPc5SvIBx7rbr6M1.BzadKLCDl.C`.

---

[← Previous: Introduction](01-intro.md) | [🏠 Home](../README.md) | [Next: Login & JWT →](03-login-and-jwt.md)
