# Role-Based Authorization

## Adding isAdmin

What if deleting data should only be allowed by admins?

In `models/user.js`, add `isAdmin` to the schema:

```javascript
const userSchema = new mongoose.Schema({
  name: { ... },
  email: { ... },
  password: { ... },
  isAdmin: {
    type: Boolean,
    default: false
  }
});
```

---

### Set isAdmin Manually in MongoDB Compass

Since we don't have an admin UI yet, set the property directly:

1. Open **MongoDB Compass** → navigate to your database → users collection
2. Find a user → click the pencil icon (edit)
3. Click **+** next to the password field → "Add field after password"
4. Field name: `isAdmin`, value: `true`, type: **Boolean**
5. Click **UPDATE**

---

### Include isAdmin in the JWT Token

Now update `generateAuthToken` in `models/user.js` to include `isAdmin` in the payload:

```javascript
userSchema.methods.generateAuthToken = function() {
  const token = jwt.sign(
    { _id: this._id, isAdmin: this.isAdmin },
    config.get('jwtPrivateKey')
  );
  return token;
};
```

Because all token generation is centralised in `generateAuthToken`, this is the **only place** that needs to change.

The token payload will now contain:
```json
{
  "_id": "609429731a37803084ef0abc",
  "isAdmin": true,
  "iat": 1652025001
}
```

---

### Why Include the Role in the Token?

- **No extra DB lookup** needed — the role is already in the decoded token
- **Faster authorization** — the middleware just reads `req.user.isAdmin`
- **Trade-off:** if a user's role changes, they need a new token (must re-login)

---

[← Previous: User Logout](07-user-logout.md) | [🏠 Home](../README.md) | [Next: Admin Middleware →](09-admin-middleware.md)
