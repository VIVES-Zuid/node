# 🔧 Preparing for Production

## Chapter 13: Production-Ready Node.js

---

## 🌱 Development vs Production

```javascript
// Development
app.listen(3000, () => console.log('Dev server on port 3000'));

// Production — read from environment
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server on port ${PORT}`));
```

> **Rule**: Never hardcode ports, URLs, passwords, or secrets.

---

## 🔐 Environment Variables

### Development: `.env` file

```bash
PORT=3000
MONGODB_URI=mongodb://localhost:27017/myapp
JWT_SECRET=my_dev_secret
NODE_ENV=development
```

### Production: set on the hosting platform

```bash
# Never commit .env to git!
echo ".env" >> .gitignore
```

### Load with `dotenv`

```javascript
import 'dotenv/config';  // loads .env automatically
```

---

## 📦 `package.json` Scripts

```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js",
    "test": "node --test"
  }
}
```

- Hosting platforms run `npm start` by default
- **Never** use `nodemon` or `--watch` in production

---

## 🌍 NODE_ENV

```javascript
const isProduction = process.env.NODE_ENV === 'production';

// Hide stack traces from users in production
app.use((err, req, res, next) => {
  res.status(500).json({
    message: err.message,
    ...(isProduction ? {} : { stack: err.stack })
  });
});
```

Set `NODE_ENV=production` on your hosting platform.

---

## 🛡️ Security Checklist

```javascript
import helmet from 'helmet';
import rateLimit from 'express-rate-limit';

// Security headers
app.use(helmet());

// Rate limiting
app.use(rateLimit({
  windowMs: 15 * 60 * 1000,  // 15 minutes
  max: 100
}));

// Don't leak server info
app.disable('x-powered-by');  // helmet does this too
```

Install: `npm install helmet express-rate-limit`

---

## ❤️ Health Check Endpoint

```javascript
app.get('/health', (req, res) => {
  res.json({
    status: 'ok',
    uptime: process.uptime(),
    timestamp: new Date().toISOString()
  });
});
```

> Hosting platforms use this to check if your app is alive.

---

## 🛑 Graceful Shutdown

```javascript
const server = app.listen(PORT);

process.on('SIGTERM', () => {
  server.close(() => {
    mongoose.connection.close();
    process.exit(0);
  });
});
```

> `SIGTERM` is sent when the container/VM stops. This lets your app finish in-flight requests before exiting.

---

## ✅ Pre-Deploy Checklist

- [ ] All secrets in environment variables
- [ ] `.env` added to `.gitignore`
- [ ] `NODE_ENV=production` configured
- [ ] `npm start` works without errors
- [ ] `helmet` installed
- [ ] MongoDB URI points to cloud DB (MongoDB Atlas)
- [ ] `/health` endpoint exists

---

[← Introduction](./01-introduction.md) | [🏠 Home](../README.md) | [Next: Linux VM →](./03-linux-vm.md)
