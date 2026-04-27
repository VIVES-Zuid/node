# Auth Middleware

## Protecting Routes with Authentication

We want certain endpoints to be accessible only by **authenticated users** (e.g., the POST route in `/routes/genres`).

We could add the token check inside each route:

```javascript
router.post('/', async (req, res) => {
  const token = req.header('x-auth-token');
  // if no token: res.send(401)...
});
```

But copying this logic into every route is repetitive. Instead we **extract it into a middleware function**.

---

### Creating Auth Middleware

Create `middleware/auth.js`:

```javascript
const jwt = require('jsonwebtoken');
const config = require('config');

module.exports = function (req, res, next) {
  const token = req.header('x-auth-token');
  if (!token) return res.status(401).send('Access denied. No token provided.');

  try {
    const decoded = jwt.verify(token, config.get('jwtPrivateKey'));
    req.user = decoded;
    next();
  } catch (ex) {
    res.status(400).send('Invalid token.');
  }
}
```

---

### How It Works

```mermaid
sequenceDiagram
    participant C as Client
    participant M as Auth Middleware
    participant R as Route Handler
    participant DB as Database
    
    C->>M: Request with x-auth-token
    M->>M: Check if token exists
    alt No token
        M->>C: 401 Access denied
    else Has token
        M->>M: Verify token
        alt Invalid token
            M->>C: 400 Invalid token
        else Valid token
            M->>M: Decode payload
            M->>M: Add to req.user
            M->>R: next()
            R->>DB: Query with req.user
            DB->>R: Response
            R->>C: 200 OK
        end
    end
```

---

### Key Points

- **Token in Header**: reads from the `x-auth-token` request header
- **Verification**: uses the JWT secret to check the token's signature
- **Decoded Payload**: the decoded data (containing `_id`) is attached to `req.user`
- **next()**: passes control to the route handler if the token is valid

---

### HTTP Status Codes

| Status | Meaning | When Used |
|--------|---------|-----------|
| **401** | Unauthorized | No token provided |
| **400** | Bad Request | Invalid/malformed token |
| **403** | Forbidden | Valid token, insufficient permissions |
| **200** | OK | Success |

---

[← Previous: Login & JWT](03-login-and-jwt.md) | [🏠 Home](../README.md) | [Next: Protecting Routes →](05-protecting-routes.md)
