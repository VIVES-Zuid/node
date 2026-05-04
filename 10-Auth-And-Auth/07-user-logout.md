# User Logout

## How JWT Logout Works

The token is **not stored on the server** — it lives only on the client. This means there is no server-side "session to delete".

To log out, the client simply **deletes the token** from wherever it stored it (localStorage, sessionStorage, memory). Without the token, future requests will receive a 401.

**Key points:**
- It is **bad practice** to store tokens in the database (defeats statelessness, adds overhead)
- If you must store tokens in a DB (e.g., for emergency revocation), **encrypt them first**
- An extra security measure: only send tokens over **HTTPS** (encrypted transport)

---

### Comparison: Session vs JWT Logout

| | Session-Based | JWT-Based |
|---|---|---|
| Server storage | Yes | No |
| Logout action | Delete session on server | Delete token on client |
| Immediate effect | Instant | Until token expiration |
| Scalability | Harder | Easier |

---

### Best Practices

1. **Short token expiration** — limit the damage window if a token is stolen:
   ```javascript
   jwt.sign({ _id: this._id }, key, { expiresIn: '1h' });
   ```

2. **HTTPS only** in production — tokens in transit are encrypted

3. **Refresh tokens** for long-lived sessions — use short-lived access tokens (15min–1hr) paired with long-lived refresh tokens that can be revoked

---

[← Previous: Getting Current User](06-current-user.md) | [🏠 Home](../README.md) | [Next: Role-Based Authorization →](08-role-based-auth.md)
