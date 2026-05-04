# Login Route, JWT & Environment Variables

## Step 1: Create the Login Route

Create `routes/auth.js`:

```javascript
const Joi = require('joi');
const bcrypt = require('bcrypt');
const { User } = require('../models/user');
const mongoose = require('mongoose');
const _ = require('lodash');
const express = require('express');
const router = express.Router();

router.post('/', async (req, res) => {
  const { error } = validate(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  let user = await User.findOne({ email: req.body.email });
  if (!user) return res.status(400).send('Invalid email or password');

  const validPassword = await bcrypt.compare(req.body.password, user.password);
  if (!validPassword) return res.status(400).send('Invalid email or password');

  res.send(true);
});

function validate(req) {
  const schema = Joi.object({
    email: Joi.string().min(5).max(50).email().required(),
    password: Joi.string().min(5).max(255).required()
  });
  return schema.validate(req);
}

module.exports = router;
```

Register it in `index.js`:

```javascript
const auth = require('./routes/auth');
// ...
app.use('/api/auth', auth);
```

Test in Postman: `POST http://localhost:3000/api/auth` with `{ "email": "...", "password": "..." }` — response: `true`.

---

## Step 2: JSON Web Tokens

In practice we don't send back `true` — we send a **JWT** (JSON Web Token).

A JWT is a long string that identifies a user. When a user logs in they receive a token; they send it with every subsequent request so the server knows who they are.

A JWT has three parts:
- **Header** — the algorithm used (e.g., HS256)
- **Payload** — the data (public user properties)
- **Signature** — a hash of header + payload + private key

> If the user tampers with the payload, the signature no longer matches. You can inspect any JWT at [jwt.io](https://jwt.io).

Install the package:

```bash
npm i jsonwebtoken
```

In `routes/auth.js`, generate and return a token after verifying the password:

```javascript
const jwt = require('jsonwebtoken');

// ...after validPassword check:
const token = jwt.sign({ _id: user._id }, 'jwtPrivateKey');
res.send(token);
```

Test again — you should receive a long JWT string. Paste it into [jwt.io](https://jwt.io) to see its three parts (header, payload, signature).

---

## Step 3: Environment Variables with config

Hardcoding `'jwtPrivateKey'` in the source code is a **security risk** — anyone with access to the repo can see it.

The solution: store it as an **environment variable** and read it via the `config` package.

```bash
npm i config
```

Create the folder `config/` and two files:

**config/default.json** — default (empty) values:
```json
{
  "jwtPrivateKey": ""
}
```

**config/custom-environment-variables.json** — maps config keys to env var names (the name is important!):
```json
{
  "jwtPrivateKey": "vivesbib_jwtPrivateKey"
}
```

### Set the environment variable

**Mac/Linux:**
```bash
export vivesbib_jwtPrivateKey=mySecureKey
```

**Windows CMD:**
```cmd
set vivesbib_jwtPrivateKey=mySecureKey
```

**Windows PowerShell:**
```powershell
$Env:vivesbib_jwtPrivateKey = 'mySecureKey'
```

> The environment variable is stored **locally on your machine**. If the app runs on another machine/server, the variable must be set there too. Prefixing with the app name (`vivesbib_`) avoids conflicts with other applications.

### Use config in auth.js

```javascript
const config = require('config');

// ...
const token = jwt.sign({ _id: user._id }, config.get('jwtPrivateKey'));
res.send(token);
```

### Startup check in index.js

Prevent the app from starting if the key is not configured:

```javascript
const config = require('config');

if (!config.get('jwtPrivateKey')) {
  console.error('FATAL ERROR: jwtPrivateKey not defined.');
  process.exit(1);
}
```

---

## Step 4: Return the Token in Response Headers

Currently we return the token in the response body from `routes/auth.js`. But we also want to return it when a user **registers** (so they don't have to log in immediately after signing up).

If we copy the token generation into `routes/users.js` as well, we have **duplicate code** — and the payload (`{ _id: user._id }`) will grow when we add fields like `isAdmin`.

### Better: send token in a response header

In `routes/users.js`, after saving the user:

```javascript
const jwt = require('jsonwebtoken');
const config = require('config');

// ...
const token = jwt.sign({ _id: user._id }, config.get('jwtPrivateKey'));
res.header('x-auth-token', token).send(_.pick(user, ['_id', 'name', 'email']));
```

> We prefix custom headers with `x-` by convention. The client receives the token in the response header `x-auth-token`.

---

## Step 5: Information Expert Principle — generateAuthToken

We now have token generation code in both `routes/auth.js` and `routes/users.js`. This violates the DRY principle, but there's a deeper issue: **where should this logic live?**

In OOP, the **Information Expert Principle** says the object that knows the most about the data should also contain the logic that operates on it. The `user` object knows its `_id` — so token generation belongs on the User model, not scattered across routes.

### Refactor models/user.js

Extract the schema into a named constant and add a method:

```javascript
const jwt = require('jsonwebtoken');
const config = require('config');

const userSchema = new mongoose.Schema({
  name: { ... },
  email: { ... },
  password: { ... }
});

userSchema.methods.generateAuthToken = function() {
  const token = jwt.sign({ _id: this._id }, config.get('jwtPrivateKey'));
  return token;
};

const User = mongoose.model('User', userSchema);
```

### Update routes/users.js

```javascript
const token = user.generateAuthToken();
res.header('x-auth-token', token).send(_.pick(user, ['_id', 'name', 'email']));
```

### Update routes/auth.js

```javascript
const token = user.generateAuthToken();
res.send(token);
```

Now all token generation logic lives in one place. When the payload changes (e.g., adding `isAdmin` later), you only update the model.

---

### REST Client — testing the register flow

In `api_calls/api_calls_users.http`:

```http
### Register a new user (token is in the x-auth-token response header)
POST {{base_URL}}
Content-Type: application/json

{
    "name": "Vives",
    "email": "milan12@vives.be",
    "password": "12345"
}
```

Check the response headers — you should see `x-auth-token: eyJhbGci...`.

---

[← Previous: User Model & Registration](02-user-model.md) | [🏠 Home](../README.md) | [Next: Auth Middleware →](04-auth-middleware.md)
