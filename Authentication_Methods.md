# Authentication Methods and Security Measures in Modern Technology

This document provides a comprehensive overview of modern authentication methods and security measures used in web applications, with a focus on practices employed by high-tech companies like Meta, Amazon, Google, and others. It includes previously implemented methods plus new additions: Passkeys (WebAuthn), Single Sign-On (SSO), Certificate-Based Authentication (CBA), and Behavioral Biometrics. Each method includes its concept, backend implementation (Node.js with Express where applicable), and frontend implementation (minimal HTML/JavaScript). Detailed comments explain each section, variable, and key, wiMth input/output examples embedded. Explanations for libraries and tasks from previous sections are not repeated.

## Prerequisites
- **Node.js Dependencies**: Install packages using `npm install express express-session jsonwebtoken axios speakeasy bcrypt passport passport-google-oauth20`.
- **Security Notes**: Use HTTPS in production. Store secrets (e.g., session secrets, JWT keys) in environment variables or a secure vault. Implement token revocation for token-based methods. Use secure hardware for certificate-based authentication.
- **Running the Code**: Each backend runs on unique ports (3000–3009). Adjust ports if running multiple servers.
- **Extensibility**: Additional methods like OpenID Connect can be added.
- **Additional Setup**: For OAuth/SSO, register with providers (e.g., Google). For WebAuthn, ensure browser support. For CBA, generate certificates. Behavioral Biometrics requires a third-party service in production.

---

## 1. Session-Based Authentication
**Concept**: The server generates a session ID upon login, stored server-side (e.g., Redis) and sent as a cookie. The client includes the cookie in requests. Stateful, used by companies like Meta for web apps.

### Backend Code
```javascript
// backend-session.js
const express = require('express');
const session = require('express-session');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());
app.use(session({
  secret: 'session-secret', // User-defined key for signing session cookies; store securely in env variables
  resave: false,
  saveUninitialized: false,
  cookie: { secure: false } // Set secure: true with HTTPS in production
}));

const users = [
  { id: 1, username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Login route to create session
// Input: POST /login { "username": "user1", "password": "pass1" }
// Output: { "message": "Login successful" }
app.post('/login', async (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username);
  if (user && await bcrypt.compare(password, user.password)) {
    req.session.userId = user.id;
    res.json({ message: 'Login successful' });
  } else {
    res.status(401).json({ message: 'Invalid credentials' });
  }
});

// Protected profile route
// Input: GET /profile (with session cookie)
// Output: { "message": "Welcome, user 1" }
app.get('/profile', (req, res) => {
  if (req.session.userId) {
    res.json({ message: `Welcome, user ${req.session.userId}` });
  } else {
    res.status(401).json({ message: 'Unauthorized' });
  }
});

// Logout route to destroy session
// Input: POST /logout (with session cookie)
// Output: { "message": "Logout successful" }
app.post('/logout', (req, res) => {
  req.session.destroy(err => {
    if (err) return res.status(500).json({ message: 'Logout failed' });
    res.json({ message: 'Logout successful' });
  });
});

app.listen(3000, () => console.log('Session server on port 3000'));
```

### Frontend Code
```html
<!-- frontend-session.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Session Auth</title>
</head>
<body>
  <h2>Login</h2>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Login</button>
  <button onclick="getProfile()">Profile</button>
  <button onclick="logout()">Logout</button>
  <div id="response"></div>

  <script>
    async function login() {
      // Input: { username: "user1", password: "pass1" }
      // Output: { message: "Login successful" }
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      try {
        const res = await fetch('http://localhost:3000/login', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username, password }),
          credentials: 'include'
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }

    async function getProfile() {
      // Output: { message: "Welcome, user 1" }
      try {
        const res = await fetch('http://localhost:3000/profile', { credentials: 'include' });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }

    async function logout() {
      // Output: { message: "Logout successful" }
      try {
        const res = await fetch('http://localhost:3000/logout', {
          method: 'POST',
          credentials: 'include'
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 2. JSON Web Token (JWT) Authentication
**Concept**: A stateless method where the server issues a signed JWT upon login. The client sends the token in the `Authorization` header. Used by Amazon for API authentication.

### Backend Code
```javascript
// backend-jwt.js
const express = require('express');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());

const SECRET_KEY = 'jwt-secret'; // User-defined key for signing JWTs; store securely

const users = [
  { id: 1, username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Login route to issue JWT
// Input: POST /login { "username": "user1", "password": "pass1" }
// Output: { "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
app.post('/login', async (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username);
  if (user && await bcrypt.compare(password, user.password)) {
    const token = jwt.sign({ userId: user.id }, SECRET_KEY, { expiresIn: '1h' });
    res.json({ token });
  } else {
    res.status(401).json({ message: 'Invalid credentials' });
  }
});

// Protected profile route
// Input: GET /profile Authorization: Bearer <token>
// Output: { "message": "Welcome, user 1" }
app.get('/profile', (req, res) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ message: 'No token' });
  try {
    const decoded = jwt.verify(token, SECRET_KEY);
    res.json({ message: `Welcome, user ${decoded.userId}` });
  } catch (err) {
    res.status(401).json({ message: 'Invalid token' });
  }
});

app.listen(3001, () => console.log('JWT server on port 3001'));
```

### Frontend Code
```html
<!-- frontend-jwt.html -->
<!DOCTYPE html>
<html>
<head>
  <title>JWT Auth</title>
</head>
<body>
  <h2>Login</h2>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Login</button>
  <button onclick="getProfile()">Profile</button>
  <div id="response"></div>

  <script>
    let token = '';

    async function login() {
      // Input: { username: "user1", password: "pass1" }
      // Output: { token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      try {
        const res = await fetch('http://localhost:3001/login', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username, password })
        });
        const data = await res.json();
        if (data.token) {
          token = data.token;
          localStorage.setItem('jwt', token);
        }
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }

    async function getProfile() {
      // Output: { message: "Welcome, user 1" }
      try {
        const res = await fetch('http://localhost:3001/profile', {
          headers: { 'Authorization': `Bearer ${token || localStorage.getItem('jwt')}` }
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 3. OAuth 2.0
**Concept**: A framework where a third-party provider (e.g., GitHub) issues an access token after user consent. Used by Google for social logins.

### Backend Code
```javascript
// backend-oauth.js
const express = require('express');
const axios = require('axios');
const app = express();

const CLIENT_ID = 'your-github-client-id'; // User-defined GitHub OAuth app client ID; obtain from GitHub
const CLIENT_SECRET = 'your-github-client-secret'; // User-defined GitHub OAuth app client secret; store securely
const REDIRECT_URI = 'http://localhost:3002/callback'; // User-defined redirect URI registered with GitHub

// Redirect to GitHub for authorization
// Input: GET /login
// Output: Redirects to GitHub authorization page
app.get('/login', (req, res) => {
  res.redirect(`https://github.com/login/oauth/authorize?client_id=${CLIENT_ID}&redirect_uri=${REDIRECT_URI}`);
});

// Callback to exchange code for token
// Input: GET /callback?code=abc123
// Output: { "message": "Welcome, <github-username>", "accessToken": "gho_..." }
app.get('/callback', async (req, res) => {
  const { code } = req.query;
  try {
    const response = await axios.post('https://github.com/login/oauth/access_token', {
      client_id: CLIENT_ID,
      client_secret: CLIENT_SECRET,
      code,
      redirect_uri: REDIRECT_URI
    }, { headers: { accept: 'application/json' } });
    const accessToken = response.data.access_token;
    const userData = await axios.get('https://api.github.com/user', {
      headers: { Authorization: `Bearer ${accessToken}` }
    });
    res.json({ message: `Welcome, ${userData.data.login}`, accessToken });
  } catch (err) {
    res.status(500).json({ message: 'OAuth error' });
  }
});

app.listen(3002, () => console.log('OAuth server on port 3002'));
```

### Frontend Code
```html
<!-- frontend-oauth.html -->
<!DOCTYPE html>
<html>
<head>
  <title>OAuth Auth</title>
</head>
<body>
  <h2>OAuth Login</h2>
  <button onclick="login()">Login with GitHub</button>
  <div id="response"></div>

  <script>
    async function login() {
      // Output: Redirects to GitHub authorization page
      window.location.href = 'http://localhost:3002/login';
    }

    // Handle callback
    // Output: { message: "Welcome, <username>", accessToken: "gho_..." }
    window.onload = async () => {
      const urlParams = new URLSearchParams(window.location.search);
      const code = urlParams.get('code');
      if (code) {
        try {
          const res = await fetch(`http://localhost:3002/callback?code=${code}`);
          const data = await res.json();
          document.getElementById('response').textContent = JSON.stringify(data);
        } catch (err) {
          document.getElementById('response').textContent = 'Error: ' + err.message;
        }
      }
    };
  </script>
</body>
</html>
```

---

## 4. API Key Authentication
**Concept**: Clients include a unique API key in requests. Simple, used by AWS for service APIs, but less secure for client-side apps.

### Backend Code
```javascript
// backend-api-key.js
const express = require('express');
const app = express();

app.use(express.json());

const API_KEYS = ['api-key-123']; // User-defined array of valid API keys; store securely

// Middleware to check API key
// Input: GET /data x-api-key=api-key-123
// Output: { "message": "Access granted" }
app.use((req, res, next) => {
  const apiKey = req.headers['x-api-key'] || req.query.apiKey;
  if (API_KEYS.includes(apiKey)) {
    next();
  } else {
    res.status(401).json({ message: 'Invalid API key' });
  }
});

// Protected data route
app.get('/data', (req, res) => {
  res.json({ message: 'Access granted' });
});

app.listen(3003, () => console.log('API Key server on port 3003'));
```

### Frontend Code
```html
<!-- frontend-api-key.html -->
<!DOCTYPE html>
<html>
<head>
  <title>API Key Auth</title>
</head>
<body>
  <h2>API Key Access</h2>
  <input id="apiKey" placeholder="API Key">
  <button onclick="getData()">Get Data</button>
  <div id="response"></div>

  <script>
    async function getData() {
      // Input: api-key-123
      // Output: { message: "Access granted" }
      const apiKey = document.getElementById('apiKey').value;
      try {
        const res = await fetch('http://localhost:3003/data', {
          headers: { 'x-api-key': apiKey }
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 5. Basic Authentication
**Concept**: The client sends Base64-encoded username and password in the `Authorization` header. Used in legacy systems by some companies.

### Backend Code
```javascript
// backend-basic-auth.js
const express = require('express');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());

const users = [
  { username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Middleware to check Basic Auth
// Input: GET /secure Authorization: Basic dXNlcjE6cGFzczE=
// Output: { "message": "Access granted" }
app.use((req, res, next) => {
  const authHeader = req.headers.authorization;
  if (!authHeader || !authHeader.startsWith('Basic ')) {
    return res.status(401).json({ message: 'Missing Authorization' });
  }
  const base64Credentials = authHeader.split(' ')[1];
  const credentials = Buffer.from(base64Credentials, 'base64').toString('ascii');
  const [username, password] = credentials.split(':');
  const user = users.find(u => u.username === username);
  if (user && await bcrypt.compare(password, user.password)) {
    next();
  } else {
    res.status(401).json({ message: 'Invalid credentials' });
  }
});

// Protected secure route
app.get('/secure', (req, res) => {
  res.json({ message: 'Access granted' });
});

app.listen(3004, () => console.log('Basic Auth server on port 3004'));
```

### Frontend Code
```html
<!-- frontend-basic-auth.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Basic Auth</title>
</head>
<body>
  <h2>Basic Auth</h2>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="accessSecure()">Access</button>
  <div id="response"></div>

  <script>
    async function accessSecure() {
      // Input: user1:pass1
      // Output: { message: "Access granted" }
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      try {
        const res = await fetch('http://localhost:3004/secure', {
          headers: {
            'Authorization': 'Basic ' + btoa(`${username}:${password}`)
          }
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 6. Multi-Factor Authentication (MFA) with TOTP
**Concept**: Requires multiple verification methods (password + TOTP from an authenticator app). Used by Google for enhanced security.

### Backend Code
```javascript
// backend-mfa.js
const express = require('express');
const speakeasy = require('speakeasy');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());

const users = [
  { id: 1, username: 'user1', password: bcrypt.hashSync('pass1', 10), mfaSecret: null }
];

// Setup MFA route
// Input: POST /setup-mfa { "username": "user1" }
// Output: { "secret": "JBSWY3DPEHPK3PXP..." }
app.post('/setup-mfa', (req, res) => {
  const { username } = req.body;
  const user = users.find(u => u.username === username);
  if (!user) return res.status(404).json({ message: 'User not found' });
  const secret = speakeasy.generateSecret({ name: 'MyApp' });
  user.mfaSecret = secret.base32;
  res.json({ secret: secret.base32 });
});

// MFA login route
// Input: POST /login-mfa { "username": "user1", "password": "pass1", "token": "123456" }
// Output: { "message": "MFA login successful" }
app.post('/login-mfa', async (req, res) => {
  const { username, password, token } = req.body;
  const user = users.find(u => u.username === username);
  if (!user || !(await bcrypt.compare(password, user.password))) {
    return res.status(401).json({ message: 'Invalid credentials' });
  }
  if (!user.mfaSecret) {
    return res.status(401).json({ message: 'MFA not set up' });
  }
  const verified = speakeasy.totp.verify({
    secret: user.mfaSecret,
    encoding: 'base32',
    token
  });
  if (verified) {
    res.json({ message: 'MFA login successful' });
  } else {
    res.status(401).json({ message: 'Invalid MFA token' });
  }
});

app.listen(3005, () => console.log('MFA server on port 3005'));
```

### Frontend Code
```html
<!-- frontend-mfa.html -->
<!DOCTYPE html>
<html>
<head>
  <title>MFA Auth</title>
</head>
<body>
  <h2>MFA Login</h2>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <input id="token" placeholder="MFA Token">
  <button onclick="setupMfa()">Setup MFA</button>
  <button onclick="loginMfa()">Login</button>
  <div id="response"></div>

  <script>
    async function setupMfa() {
      // Input: { username: "user1" }
      // Output: { secret: "JBSWY3DPEHPK3PXP..." }
      const username = document.getElementById('username').value;
      try {
        const res = await fetch('http://localhost:3005/setup-mfa', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username })
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }

    async function loginMfa() {
      // Input: { username: "user1", password: "pass1", token: "123456" }
      // Output: { message: "MFA login successful" }
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      const token = document.getElementById('token').value;
      try {
        const res = await fetch('http://localhost:3005/login-mfa', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username, password, token })
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 7. Token-Based Authentication with Refresh Tokens
**Concept**: Issues a short-lived access token and a long-lived refresh token. Used by Meta for persistent API sessions.

### Backend Code
```javascript
// backend-refresh-token.js
const express = require('express');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());

const ACCESS_SECRET = 'access-secret'; // User-defined key for access tokens; store securely
const REFRESH_SECRET = 'refresh-secret'; // User-defined key for refresh tokens; store securely
const refreshTokens = []; // Store refresh tokens (use database in production)

const users = [
  { id: 1, username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Login route to issue tokens
// Input: POST /login { "username": "user1", "password": "pass1" }
// Output: { "accessToken": "...", "refreshToken": "..." }
app.post('/login', async (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username);
  if (user && await bcrypt.compare(password, user.password)) {
    const accessToken = jwt.sign({ userId: user.id }, ACCESS_SECRET, { expiresIn: '15m' });
    const refreshToken = jwt.sign({ userId: user.id }, REFRESH_SECRET);
    refreshTokens.push(refreshToken);
    res.json({ accessToken, refreshToken });
  } else {
    res.status(401).json({ message: 'Invalid credentials' });
  }
});

// Refresh token route
// Input: POST /refresh { "refreshToken": "..." }
// Output: { "accessToken": "..." }
app.post('/refresh', (req, res) => {
  const { refreshToken } = req.body;
  if (!refreshToken || !refreshTokens.includes(refreshToken)) {
    return res.status(401).json({ message: 'Invalid refresh token' });
  }
  try {
    const decoded = jwt.verify(refreshToken, REFRESH_SECRET);
    const accessToken = jwt.sign({ userId: decoded.userId }, ACCESS_SECRET, { expiresIn: '15m' });
    res.json({ accessToken });
  } catch (err) {
    res.status(401).json({ message: 'Invalid refresh token' });
  }
});

// Protected profile route
// Input: GET /profile Authorization: Bearer <accessToken>
// Output: { "message": "Welcome, user 1" }
app.get('/profile', (req, res) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ message: 'No token' });
  try {
    const decoded = jwt.verify(token, ACCESS_SECRET);
    res.json({ message: `Welcome, user ${decoded.userId}` });
  } catch (err) {
    res.status(401).json({ message: 'Invalid token' });
  }
});

app.listen(3006, () => console.log('Refresh Token server on port 3006'));
```

### Frontend Code
```html
<!-- frontend-refresh-token.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Refresh Token Auth</title>
</head>
<body>
  <h2>Refresh Token Login</h2>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login()">Login</button>
  <button onclick="refreshToken()">Refresh</button>
  <button onclick="getProfile()">Profile</button>
  <div id="response"></div>

  <script>
    let accessToken = '';
    let refreshToken = '';

    async function login() {
      // Input: { username: "user1", password: "pass1" }
      // Output: { accessToken: "...", refreshToken: "..." }
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      try {
        const res = await fetch('http://localhost:3006/login', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username, password })
        });
        const data = await res.json();
        if (data.accessToken && data.refreshToken) {
          accessToken = data.accessToken;
          refreshToken = data.refreshToken;
          localStorage.setItem('accessToken', accessToken);
          localStorage.setItem('refreshToken', refreshToken);
        }
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }

    async function refreshToken() {
      // Input: { refreshToken: "..." }
      // Output: { accessToken: "..." }
      try {
        const res = await fetch('http://localhost:3006/refresh', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ refreshToken: refreshToken || localStorage.getItem('refreshToken') })
        });
        const data = await res.json();
        if (data.accessToken) {
          accessToken = data.accessToken;
          localStorage.setItem('accessToken', accessToken);
        }
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }

    async function getProfile() {
      // Output: { message: "Welcome, user 1" }
      try {
        const res = await fetch('http://localhost:3006/profile', {
          headers: { 'Authorization': `Bearer ${accessToken || localStorage.getItem('accessToken')}` }
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 8. Passkeys (FIDO2/WebAuthn)
**Concept**: Uses public-key cryptography for passwordless authentication via biometrics or hardware keys. Supported by Google, Apple, and Microsoft for secure, phishing-resistant logins.

### Backend Code
```javascript
// backend-passkey.js
const express = require('express');
const app = express();

app.use(express.json());

const users = [
  { id: 1, username: 'user1', credentialId: null, publicKey: null }
];

// Start WebAuthn registration
// Input: POST /register { "username": "user1" }
// Output: { "challenge": "..." }
app.post('/register', (req, res) => {
  const { username } = req.body;
  const user = users.find(u => u.username === username);
  if (!user) return res.status(404).json({ message: 'User not found' });
  const challenge = Buffer.from(crypto.randomBytes(32)).toString('base64'); // Generate random challenge
  user.challenge = challenge; // Store challenge temporarily
  res.json({ challenge });
});

// Complete registration
// Input: POST /register-complete { "username": "user1", "credentialId": "...", "publicKey": "..." }
// Output: { "message": "Registration successful" }
app.post('/register-complete', (req, res) => {
  const { username, credentialId, publicKey } = req.body;
  const user = users.find(u => u.username === username);
  if (!user || !user.challenge) return res.status(400).json({ message: 'Invalid registration' });
  user.credentialId = credentialId; // Store credential ID
  user.publicKey = publicKey; // Store public key
  user.challenge = null; // Clear challenge
  res.json({ message: 'Registration successful' });
});

// Authenticate with passkey
// Input: POST /login-passkey { "username": "user1", "credentialId": "...", "signature": "..." }
// Output: { "message": "Passkey login successful" }
app.post('/login-passkey', (req, res) => {
  const { username, credentialId, signature } = req.body;
  const user = users.find(u => u.username === username && u.credentialId === credentialId);
  if (!user) return res.status(401).json({ message: 'Invalid credentials' });
  // In production, verify signature with publicKey using a WebAuthn library
  res.json({ message: 'Passkey login successful' });
});

app.listen(3007, () => console.log('Passkey server on port 3007'));
```

### Frontend Code
```html
<!-- frontend-passkey.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Passkey Auth</title>
</head>
<body>
  <h2>Passkey Login</h2>
  <input id="username" placeholder="Username">
  <button onclick="register()">Register</button>
  <button onclick="login()">Login</button>
  <div id="response"></div>

  <script>
    async function register() {
      // Input: { username: "user1" }
      // Output: { challenge: "..." }
      const username = document.getElementById('username').value;
      try {
        const res = await fetch('http://localhost:3007/register', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username })
        });
        const data = await res.json();
        const publicKey = {
          challenge: Uint8Array.from(atob(data.challenge), c => c.charCodeAt(0)),
          rp: { name: 'MyApp' },
          user: { id: new Uint8Array(16), name: username, displayName: username },
          pubKeyCredParams: [{ type: 'public-key', alg: -7 }]
        };
        const credential = await navigator.credentials.create({ publicKey });
        const resComplete = await fetch('http://localhost:3007/register-complete', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            username,
            credentialId: btoa(String.fromCharCode(...new Uint8Array(credential.rawId))),
            publicKey: btoa(String.fromCharCode(...new Uint8Array(credential.response.getPublicKey())))
          })
        });
        const completeData = await resComplete.json();
        document.getElementById('response').textContent = JSON.stringify(completeData);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }

    async function login() {
      // Input: { username: "user1" }
      // Output: { message: "Passkey login successful" }
      const username = document.getElementById('username').value;
      try {
        const publicKey = {
          challenge: new Uint8Array(32),
          allowCredentials: [{ type: 'public-key', id: new Uint8Array(16) }]
        };
        const credential = await navigator.credentials.get({ publicKey });
        const res = await fetch('http://localhost:3007/login-passkey', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            username,
            credentialId: btoa(String.fromCharCode(...new Uint8Array(credential.rawId))),
            signature: btoa(String.fromCharCode(...new Uint8Array(credential.response.signature)))
          })
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 9. Single Sign-On (SSO) with Google
**Concept**: Allows users to authenticate once via a provider (e.g., Google) and access multiple services. Used by Google Workspace and enterprises.

### Backend Code
```javascript
// backend-sso.js
const express = require('express');
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;
const app = express();

app.use(express.json());
app.use(session({ secret: 'sso-secret', resave: false, saveUninitialized: false })); // User-defined session secret
app.use(passport.initialize());
app.use(passport.session());

passport.use(new GoogleStrategy({
  clientID: 'your-google-client-id', // User-defined Google OAuth client ID
  clientSecret: 'your-google-client-secret', // User-defined Google OAuth client secret
  callbackURL: 'http://localhost:3008/auth/google/callback'
}, (accessToken, refreshToken, profile, done) => {
  // Store user profile in session
  return done(null, profile);
}));

passport.serializeUser((user, done) => done(null, user));
passport.deserializeUser((obj, done) => done(null, obj));

// Google SSO login
// Input: GET /auth/google
// Output: Redirects to Google authentication
app.get('/auth/google', passport.authenticate('google', { scope: ['profile', 'email'] }));

// Callback route
// Input: GET /auth/google/callback
// Output: { "message": "Welcome, <user>" }
app.get('/auth/google/callback', passport.authenticate('google', { failureRedirect: '/' }), (req, res) => {
  res.json({ message: `Welcome, ${req.user.displayName}` });
});

// Protected profile route
// Input: GET /profile
// Output: { "message": "Welcome, <user>" }
app.get('/profile', (req, res) => {
  if (req.isAuthenticated()) {
    res.json({ message: `Welcome, ${req.user.displayName}` });
  } else {
    res.status(401).json({ message: 'Unauthorized' });
  }
});

app.listen(3008, () => console.log('SSO server on port 3008'));
```

### Frontend Code
```html
<!-- frontend-sso.html -->
<!DOCTYPE html>
<html>
<head>
  <title>SSO Auth</title>
</head>
<body>
  <h2>SSO Login</h2>
  <button onclick="login()">Login with Google</button>
  <button onclick="getProfile()">Profile</button>
  <div id="response"></div>

  <script>
    async function login() {
      // Output: Redirects to Google authentication
      window.location.href = 'http://localhost:3008/auth/google';
    }

    async function getProfile() {
      // Output: { message: "Welcome, <user>" }
      try {
        const res = await fetch('http://localhost:3008/profile', { credentials: 'include' });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 10. Certificate-Based Authentication (CBA)
**Concept**: Uses X.509 certificates for authentication, common in AWS for device authentication.

### Backend Code
```javascript
// backend-cba.js
const express = require('express');
const https = require('https');
const fs = require('fs');
const app = express();

app.use(express.json());

// Mock certificate validation (use real CA verification in production)
// Input: GET /secure (with client certificate)
// Output: { "message": "Access granted" }
app.get('/secure', (req, res) => {
  const cert = req.socket.getPeerCertificate();
  if (cert && cert.subject) {
    res.json({ message: 'Access granted', subject: cert.subject.CN });
  } else {
    res.status(401).json({ message: 'No valid certificate' });
  }
});

// HTTPS server with client certificate verification
https.createServer({
  key: fs.readFileSync('server.key'), // Server private key; generate with OpenSSL
  cert: fs.readFileSync('server.crt'), // Server certificate; generate with OpenSSL
  requestCert: true,
  rejectUnauthorized: false // Allow for demo; set to true in production
}, app).listen(3009, () => console.log('CBA server on port 3009'));
```

### Frontend Code
```html
<!-- frontend-cba.html -->
<!DOCTYPE html>
<html>
<head>
  <title>CBA Auth</title>
</head>
<body>
  <h2>Certificate Auth</h2>
  <button onclick="accessSecure()">Access</button>
  <div id="response"></div>

  <script>
    async function accessSecure() {
      // Output: { message: "Access granted", subject: "<CN>" }
      try {
        const res = await fetch('https://localhost:3009/secure', {
          agent: new https.Agent({ rejectUnauthorized: false }) // Disable for demo
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 11. Behavioral Biometrics
**Concept**: Authenticates based on user behavior (e.g., typing patterns). Used by companies like Amazon for fraud detection. Requires third-party services in production.

### Backend Code
```javascript
// backend-behavioral.js
const express = require('express');
const app = express();

app.use(express.json());

const users = [
  { id: 1, username: 'user1', behaviorProfile: { typingSpeed: 50 } }
];

// Authenticate based on behavior
// Input: POST /login-behavioral { "username": "user1", "typingSpeed": 50 }
// Output: { "message": "Behavioral login successful" }
app.post('/login-behavioral', (req, res) => {
  const { username, typingSpeed } = req.body;
  const user = users.find(u => u.username === username);
  if (user && Math.abs(user.behaviorProfile.typingSpeed - typingSpeed) < 10) {
    res.json({ message: 'Behavioral login successful' });
  } else {
    res.status(401).json({ message: 'Invalid behavior' });
  }
});

app.listen(3010, () => console.log('Behavioral server on port 3010'));
```

### Frontend Code
```html
<!-- frontend-behavioral.html -->
<!DOCTYPE html>
<html>
<head>
  <title>Behavioral Auth</title>
</head>
<body>
  <h2>Behavioral Login</h2>
  <input id="username" placeholder="Username">
  <input id="typingSpeed" placeholder="Typing Speed">
  <button onclick="login()">Login</button>
  <div id="response"></div>

  <script>
    async function login() {
      // Input: { username: "user1", typingSpeed: 50 }
      // Output: { message: "Behavioral login successful" }
      const username = document.getElementById('username').value;
      const typingSpeed = parseInt(document.getElementById('typingSpeed').value);
      try {
        const res = await fetch('http://localhost:3010/login-behavioral', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ username, typingSpeed })
        });
        const data = await res.json();
        document.getElementById('response').textContent = JSON.stringify(data);
      } catch (err) {
        document.getElementById('response').textContent = 'Error: ' + err.message;
      }
    }
  </script>
</body>
</html>
```

---

## 12. Modern Security Measures
**Overview**: Tech giants like Meta, Amazon, and Google implement advanced security measures to protect authentication systems. Below are key practices:

1. **End-to-End Encryption**: Use TLS (HTTPS) for all communications. AWS and Google enforce AES-256 for data at rest.
2. **Zero Trust Architecture**: Verify every request, regardless of source. Google’s BeyondCorp model uses continuous authentication.
3. **AI-Driven Threat Detection**: Machine learning models (e.g., Amazon GuardDuty) analyze user behavior to detect anomalies.
4. **Secure Key Management**: Store secrets in vaults like AWS KMS or Google Cloud KMS.
5. **Rate Limiting and WAF**: Implement rate limiting and Web Application Firewalls (e.g., AWS WAF) to prevent brute-force attacks.
6. **Compliance Frameworks**: Adhere to GDPR, SOC 2, and ISO 27001 for data protection.
7. **Secure Development Lifecycle**: Integrate security in CI/CD pipelines, as done by Meta.
8. **Token Revocation**: Maintain blacklists for JWTs and refresh tokens in databases.
9. **Multi-Region Redundancy**: Ensure authentication services are highly available, as in AWS IAM.
10. **Audit Logging**: Log all authentication events for monitoring, as used by Google Cloud.

### Example Implementation (Rate Limiting)
```javascript
// backend-rate-limiting.js
const express = require('express');
const rateLimit = require('express-rate-limit');
const app = express();

app.use(express.json());

// Rate limit middleware
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit to 100 requests per window
  message: 'Too many requests'
});

// Apply rate limiting to login
// Input: POST /login { "username": "user1", "password": "pass1" }
// Output: { "message": "Login successful" } or { "message": "Too many requests" }
app.post('/login', limiter, (req, res) => {
  res.json({ message: 'Login successful' });
});

app.listen(3011, () => console.log('Rate Limiting server on port 3011'));
```

---

## Additional Notes
- **Security**: All user-defined secrets (e.g., `session-secret`, `jwt-secret`, `access-secret`, `refresh-secret`, `api-key-123`, `CLIENT_ID`, `CLIENT_SECRET`, `sso-secret`) must be random, long strings stored in environment variables. Use HTTPS, token revocation, and secure hardware for CBA.
- **Scalability**: Use Redis for sessions, databases for tokens. Register OAuth/SSO apps with providers.
- **MFA**: Add QR code generation for TOTP. Consider SMS-based OTP.
- **Passkeys**: Requires modern browsers. Use WebAuthn libraries in production.
- **CBA**: Generate certificates with OpenSSL. Use CA verification in production.
- **Behavioral Biometrics**: Integrate third-party services like BioCatch for real-world use.
