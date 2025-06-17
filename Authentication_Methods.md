# Comprehensive Guide to Authentication Methods & Modern Security Practices (2024-2025)

This guide provides a detailed exploration of authentication methods and modern security practices used by tech giants like Meta, Amazon, Google, and others. It covers traditional and cutting-edge methods, each with backend (Node.js/Express) and frontend (HTML/JavaScript) implementations, detailed comments, and input/output examples. The methods are sequenced logically, from foundational to advanced, to reflect their practical importance and adoption in 2024-2025. Each section explains **why the method is used** and **why you should learn it**, ensuring you understand its relevance in modern application development. The guide concludes with modern security practices to secure authentication systems against current and future threats.

## Table of Contents
1. [Prerequisites & Setup](#prerequisites--setup)
2. [Authentication Methods](#authentication-methods)
   - [Basic Authentication](#1-basic-authentication)
   - [Session-Based Authentication](#2-session-based-authentication)
   - [API Key Authentication](#3-api-key-authentication)
   - [JSON Web Token (JWT) Authentication](#4-json-web-token-jwt-authentication)
   - [Token-Based Authentication with Refresh Tokens](#5-token-based-authentication-with-refresh-tokens)
   - [OAuth 2.0](#6-oauth-20)
   - [Single Sign-On (SSO) with Google](#7-single-sign-on-sso-with-google)
   - [Multi-Factor Authentication (MFA) with TOTP](#8-multi-factor-authentication-mfa-with-totp)
   - [Passkeys (FIDO2/WebAuthn)](#9-passkeys-fido2webauthn)
   - [Certificate-Based Authentication (CBA)](#10-certificate-based-authentication-cba)
   - [Behavioral Biometrics](#11-behavioral-biometrics)
3. [Modern Security Practices](#modern-security-practices)
   - [Zero Trust Architecture](#zero-trust-architecture)
   - [AI-Powered Risk Assessment](#ai-powered-risk-assessment)
   - [Modern Token Management with Rotation](#modern-token-management-with-rotation)
   - [Adaptive Authentication Flow](#adaptive-authentication-flow)
   - [Privacy-Preserving Analytics](#privacy-preserving-analytics)
   - [Decentralized Identity (DID) Integration](#decentralized-identity-did-integration)
   - [Rate Limiting Implementation](#rate-limiting-implementation)
   - [Key Security Principles](#key-security-principles)
   - [Production Deployment Checklist](#production-deployment-checklist)
4. [Additional Notes](#additional-notes)

## Prerequisites & Setup
- **Node.js Dependencies**: Install required packages using:
  ```bash
  npm install express express-session jsonwebtoken axios speakeasy bcrypt passport passport-google-oauth20 helmet express-rate-limit
  ```
- **Security Requirements**:
  - Use HTTPS in production with HSTS headers.
  - Store secrets (e.g., session keys, JWT secrets, OAuth credentials) in environment variables or secure vaults (e.g., AWS KMS, HashiCorp Vault).
  - Implement token revocation for JWT and refresh tokens using databases (e.g., Redis).
  - Use secure hardware for Certificate-Based Authentication (CBA).
- **Ports**: Each backend runs on unique ports (3000–3011). Adjust if conflicts occur.
- **Additional Setup**:
  - **OAuth/SSO**: Register applications with providers (e.g., Google, GitHub).
  - **Passkeys**: Ensure browser support for WebAuthn (modern browsers like Chrome, Firefox, Safari).
  - **CBA**: Generate X.509 certificates using OpenSSL; use CA verification in production.
  - **Behavioral Biometrics**: Integrate third-party services (e.g., BioCatch) for production use.
  - **MFA**: Consider QR code generation for TOTP setup or SMS-based OTP as alternatives.
- **Extensibility**: Methods like OpenID Connect or SAML can be added for enterprise use cases.

## Authentication Methods
The following methods are ordered from simple, legacy approaches to advanced, modern techniques, reflecting their evolution and relevance in 2024-2025. Each method includes a **concept**, **why it’s used**, **why learn it**, backend and frontend implementations, and input/output examples.

### 1. Basic Authentication
**Concept**: The client sends Base64-encoded username and password in the `Authorization` header. It’s a simple, stateless method but insecure unless paired with HTTPS.

**Why It’s Used**: Common in legacy systems, internal tools, or low-security APIs due to its simplicity. Still found in some enterprise or IoT applications.

**Why Learn It**: Understanding Basic Authentication provides insight into HTTP authentication fundamentals and highlights the need for secure transport (HTTPS). It’s a stepping stone to more robust methods.

#### Backend Implementation
```javascript
// backend-basic-auth.js
const express = require('express');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());

const users = [
  { username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Middleware to validate Basic Auth header
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

// Protected route
app.get('/secure', (req, res) => {
  res.json({ message: 'Access granted' });
});

app.listen(3000, () => console.log('Basic Auth server on port 3000'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>Basic Auth</title>
</head>
<body>
  <h1>Basic Auth</h1>
  <input type="text" id="username" placeholder="Username">
  <input type="password" id="password" placeholder="Password">
  <button onclick="access()">Access</button>
  <pre id="result"></pre>

  <script>
    async function access() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      const response = await fetch('http://localhost:3000/secure', {
        headers: {
          'Authorization': 'Basic ' + btoa(`${username}:${password}`)
        }
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 2. Session-Based Authentication
**Concept**: The server creates a session ID upon login, stored server-side (e.g., in Redis) and sent to the client as a cookie. The client includes the cookie in subsequent requests.

**Why It’s Used**: Widely used by companies like Meta for web applications due to its simplicity and stateful nature, ideal for managing user sessions in monolithic apps.

**Why Learn It**: Session-based authentication is a foundational method for web apps, teaching session management, cookie security, and server-side state handling. It’s still prevalent in traditional web development.

#### Backend Implementation
```javascript
// backend-session.js
const express = require('express');
const session = require('express-session');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());
app.use(session({
  secret: 'session-secret', // Signing key for session cookies; store in env
  resave: false,
  saveUninitialized: false,
  cookie: { secure: false } // Set secure: true with HTTPS
}));

const users = [
  { id: 1, username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Login to create session
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

// Logout to destroy session
// Input: POST /logout (with session cookie)
// Output: { "message": "Logout successful" }
app.post('/logout', (req, res) => {
  req.session.destroy(err => {
    if (err) return res.status(500).json({ message: 'Logout failed' });
    res.json({ message: 'Logout successful' });
  });
});

app.listen(3001, () => console.log('Session server on port 3001'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>Session Authentication</title>
</head>
<body>
  <h1>Session Authentication</h1>
  <input type="text" id="username" placeholder="Username">
  <input type="password" id="password" placeholder="Password">
  <button onclick="login()">Login</button>
  <button onclick="profile()">Profile</button>
  <button onclick="logout()">Logout</button>
  <pre id="result"></pre>

  <script>
    async function login() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      const response = await fetch('http://localhost:3001/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password }),
        credentials: 'include'
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }

    async function profile() {
      const response = await fetch('http://localhost:3001/profile', { credentials: 'include' });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }

    async function logout() {
      const response = await fetch('http://localhost:3001/logout', {
        method: 'POST',
        credentials: 'include'
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 3. API Key Authentication
**Concept**: Clients include a unique API key in requests (header or query). It’s simple and stateless but less secure for client-side apps.

**Why It’s Used**: Popular in AWS and other API services for quick access control to server-to-server communication or low-security APIs.

**Why Learn It**: API keys are common in microservices and third-party integrations, teaching you about stateless authentication and key management. They’re a lightweight alternative to tokens.

#### Backend Implementation
```javascript
// backend-api-key.js
const express = require('express');
const app = express();

app.use(express.json());

const API_KEYS = ['api-key-123']; // Valid API keys; store securely

// Middleware to validate API key
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

app.listen(3002, () => console.log('API Key server on port 3002'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>API Key Access</title>
</head>
<body>
  <h1>API Key Access</h1>
  <input type="text" id="apiKey" placeholder="API Key">
  <button onclick="getData()">Get Data</button>
  <pre id="result"></pre>

  <script>
    async function getData() {
      const apiKey = document.getElementById('apiKey').value;
      const response = await fetch('http://localhost:3002/data', {
        headers: { 'x-api-key': apiKey }
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 4. JSON Web Token (JWT) Authentication
**Concept**: The server issues a signed JWT upon login, which the client sends in the `Authorization` header. It’s stateless, encoding user data in the token.

**Why It’s Used**: Adopted by Amazon for API services due to its statelessness, scalability, and suitability for microservices and mobile apps.

**Why Learn It**: JWTs are a cornerstone of modern API authentication, teaching token-based security, stateless systems, and token validation. They’re widely used in distributed architectures.

#### Backend Implementation
```javascript
// backend-jwt.js
const express = require('express');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());

const SECRET_KEY = 'jwt-secret'; // Signing key; store securely

const users = [
  { id: 1, username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Login to issue JWT
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

app.listen(3003, () => console.log('JWT server on port 3003'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>JWT Authentication</title>
</head>
<body>
  <h1>JWT Authentication</h1>
  <input type="text" id="username" placeholder="Username">
  <input type="password" id="password" placeholder="Password">
  <button onclick="login()">Login</button>
  <button onclick="profile()">Profile</button>
  <pre id="result"></pre>

  <script>
    let token = '';

    async function login() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      const response = await fetch('http://localhost:3003/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password })
      });
      const result = await response.json();
      token = result.token;
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }

    async function profile() {
      const response = await fetch('http://localhost:3003/profile', {
        headers: { 'Authorization': `Bearer ${token}` }
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 5. Token-Based Authentication with Refresh Tokens
**Concept**: Issues a short-lived access token and a long-lived refresh token. The client uses the refresh token to obtain new access tokens without re-authentication.

**Why It’s Used**: Employed by Meta for persistent API sessions, balancing security (short-lived tokens) and user experience (fewer logins).

**Why Learn It**: Refresh tokens enhance JWT security by reducing token lifespan and enabling token rotation, critical for long-lived sessions in mobile and web apps.

#### Backend Implementation
```javascript
// backend-refresh-token.js
const express = require('express');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');
const app = express();

app.use(express.json());

const ACCESS_SECRET = 'access-secret'; // Access token signing key; store securely
const REFRESH_SECRET = 'refresh-secret'; // Refresh token signing key; store securely
const refreshTokens = []; // Store refresh tokens; use database in production

const users = [
  { id: 1, username: 'user1', password: bcrypt.hashSync('pass1', 10) }
];

// Login to issue tokens
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

// Refresh access token
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

app.listen(3004, () => console.log('Refresh Token server on port 3004'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>Refresh Token Login</title>
</head>
<body>
  <h1>Refresh Token Login</h1>
  <input type="text" id="username" placeholder="Username">
  <input type="password" id="password" placeholder="Password">
  <button onclick="login()">Login</button>
  <button onclick="refresh()">Refresh</button>
  <button onclick="profile()">Profile</button>
  <pre id="result"></pre>

  <script>
    let accessToken = '';
    let refreshToken = '';

    async function login() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      const response = await fetch('http://localhost:3004/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password })
      });
      const result = await response.json();
      accessToken = result.accessToken;
      refreshToken = result.refreshToken;
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }

    async function refresh() {
      const response = await fetch('http://localhost:3004/refresh', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ refreshToken })
      });
      const result = await response.json();
      accessToken = result.accessToken;
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }

    async function profile() {
      const response = await fetch('http://localhost:3004/profile', {
        headers: { 'Authorization': `Bearer ${accessToken}` }
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 6. OAuth 2.0
**Concept**: A third-party provider (e.g., GitHub) issues an access token after user consent, allowing access to protected resources without sharing credentials.

**Why It’s Used**: Used by Google for social logins and API access, enabling secure, user-friendly authentication across services.

**Why Learn It**: OAuth 2.0 is the standard for third-party authentication, teaching you about authorization flows, token exchange, and integrating with providers like Google, GitHub, or Facebook.

#### Backend Implementation
```javascript
// backend-oauth.js
const express = require('express');
const axios = require('axios');
const app = express();

const CLIENT_ID = 'your-github-client-id'; // GitHub OAuth app client ID
const CLIENT_SECRET = 'your-github-client-secret'; // GitHub OAuth app client secret
const REDIRECT_URI = 'http://localhost:3005/callback'; // Registered redirect URI

// Redirect to GitHub for authorization
// Input: GET /login
// Output: Redirects to GitHub authorization page
app.get('/login', (req, res) => {
  res.redirect(`https://github.com/login/oauth/authorize?client_id=${CLIENT_ID}&redirect_uri=${REDIRECT_URI}`);
});

// Callback to exchange code for token
// Input: GET /callback?code=abc123
// Output: { "message": "Welcome, <username>", "accessToken": "gho_..." }
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

app.listen(3005, () => console.log('OAuth server on port 3005'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>OAuth Login</title>
</head>
<body>
  <h1>OAuth Login</h1>
  <button onclick="login()">Login with GitHub</button>
  <pre id="result"></pre>

  <script>
    function login() {
      window.location.href = 'http://localhost:3005/login';
    }

    // Handle callback in production by parsing URL or server response
  </script>
</body>
</html>
```

### 7. Single Sign-On (SSO) with Google
**Concept**: Users authenticate once via a provider (e.g., Google) and access multiple services without re-authenticating.

**Why It’s Used**: Adopted by Google Workspace and enterprises for seamless access across applications, improving user experience and centralizing identity management.

**Why Learn It**: SSO is critical for enterprise environments, teaching you about identity federation, session management, and integration with identity providers.

#### Backend Implementation
```javascript
// backend-sso.js
const express = require('express');
const session = require('express-session');
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;
const app = express();

app.use(session({ secret: 'sso-secret', resave: false, saveUninitialized: false }));
app.use(passport.initialize());
app.use(passport.session());

passport.use(new GoogleStrategy({
  clientID: 'your-google-client-id',
  clientSecret: 'your-google-client-secret',
  callbackURL: 'http://localhost:3006/auth/google/callback'
}, (accessToken, refreshToken, profile, done) => {
  return done(null, profile);
}));

passport.serializeUser((user, done) => done(null, user));
passport.deserializeUser((obj, done) => done(null, obj));

// SSO login
// Input: GET /auth/google
// Output: Redirects to Google authentication
app.get('/auth/google', passport.authenticate('google', { scope: ['profile', 'email'] }));

// Callback route
// Input: GET /auth/google/callback
// Output: { "message": "Welcome, <displayName>" }
app.get('/auth/google/callback', passport.authenticate('google', { failureRedirect: '/' }), (req, res) => {
  res.json({ message: `Welcome, ${req.user.displayName}` });
});

// Protected profile route
// Input: GET /profile
// Output: { "message": "Welcome, <displayName>" }
app.get('/profile', (req, res) => {
  if (req.isAuthenticated()) {
    res.json({ message: `Welcome, ${req.user.displayName}` });
  } else {
    res.status(401).json({ message: 'Unauthorized' });
  }
});

app.listen(3006, () => console.log('SSO server on port 3006'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>SSO Login</title>
</head>
<body>
  <h1>SSO Login</h1>
  <button onclick="login()">Login with Google</button>
  <button onclick="profile()">Profile</button>
  <pre id="result"></pre>

  <script>
    function login() {
      window.location.href = 'http://localhost:3006/auth/google';
    }

    async function profile() {
      const response = await fetch('http://localhost:3006/profile', { credentials: 'include' });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 8. Multi-Factor Authentication (MFA) with TOTP
**Concept**: Requires multiple verification factors (password + TOTP from an authenticator app) for enhanced security.

**Why It’s Used**: Implemented by Google to protect sensitive accounts, reducing risks from compromised credentials.

**Why Learn It**: MFA is a standard for securing high-value accounts, teaching you about layered security, TOTP generation, and user experience considerations.

#### Backend Implementation
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

// Setup MFA
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

// MFA login
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

app.listen(3007, () => console.log('MFA server on port 3007'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>MFA Login</title>
</head>
<body>
  <h1>MFA Login</h1>
  <input type="text" id="username" placeholder="Username">
  <input type="password" id="password" placeholder="Password">
  <input type="text" id="token" placeholder="MFA Token">
  <button onclick="setupMFA()">Setup MFA</button>
  <button onclick="login()">Login</button>
  <pre id="result"></pre>

  <script>
    async function setupMFA() {
      const username = document.getElementById('username').value;
      const response = await fetch('http://localhost:3007/setup-mfa', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username })
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }

    async function login() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      const token = document.getElementById('token').value;
      const response = await fetch('http://localhost:3007/login-mfa', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password, token })
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 9. Passkeys (FIDO2/WebAuthn)
**Concept**: Uses public-key cryptography for passwordless authentication via biometrics or hardware keys, offering phishing-resistant security.

**Why It’s Used**: Supported by Google, Apple, and Microsoft to replace passwords, providing a secure, user-friendly alternative.

**Why Learn It**: Passkeys represent the future of authentication, teaching you about passwordless systems, public-key cryptography, and modern browser APIs.

#### Backend Implementation
```javascript
// backend-passkey.js
const express = require('express');
const crypto = require('crypto');
const app = express();

app.use(express.json());

const users = [
  { id: 1, username: 'user1', credentialId: null, publicKey: null }
];

// Start registration
// Input: POST /register { "username": "user1" }
// Output: { "challenge": "..." }
app.post('/register', (req, res) => {
  const { username } = req.body;
  const user = users.find(u => u.username === username);
  if (!user) return res.status(404).json({ message: 'User not found' });
  const challenge = Buffer.from(crypto.randomBytes(32)).toString('base64');
  user.challenge = challenge;
  res.json({ challenge });
});

// Complete registration
// Input: POST /register-complete { "username": "user1", "credentialId": "...", "publicKey": "..." }
// Output: { "message": "Registration successful" }
app.post('/register-complete', (req, res) => {
  const { username, credentialId, publicKey } = req.body;
  const user = users.find(u => u.username === username);
  if (!user || !user.challenge) return res.status(400).json({ message: 'Invalid registration' });
  user.credentialId = credentialId;
  user.publicKey = publicKey;
  user.challenge = null;
  res.json({ message: 'Registration successful' });
});

// Authenticate with passkey
// Input: POST /login-passkey { "username": "user1", "credentialId": "...", "signature": "..." }
// Output: { "message": "Passkey login successful" }
app.post('/login-passkey', (req, res) => {
  const { username, credentialId, signature } = req.body;
  const user = users.find(u => u.username === username && u.credentialId === credentialId);
  if (!user) return res.status(401).json({ message: 'Invalid credentials' });
  // Verify signature with publicKey using WebAuthn library in production
  res.json({ message: 'Passkey login successful' });
});

app.listen(3008, () => console.log('Passkey server on port 3008'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>Passkey Login</title>
</head>
<body>
  <h1>Passkey Login</h1>
  <input type="text" id="username" placeholder="Username">
  <button onclick="register()">Register</button>
  <button onclick="login()">Login</button>
  <pre id="result"></pre>

  <script>
    async function register() {
      const username = document.getElementById('username').value;
      const response = await fetch('http://localhost:3008/register', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username })
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
      // In production, use WebAuthn navigator.credentials.create()
    }

    async function login() {
      const username = document.getElementById('username').value;
      const response = await fetch('http://localhost:3008/login-passkey', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, credentialId: 'mock', signature: 'mock' })
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
      // In production, use WebAuthn navigator.credentials.get()
    }
  </script>
</body>
</html>
```

### 10. Certificate-Based Authentication (CBA)
**Concept**: Uses X.509 certificates for authentication, verifying client identity via cryptographic certificates.

**Why It’s Used**: Employed by AWS for device authentication in IoT and secure systems, offering strong security for non-human clients.

**Why Learn It**: CBA teaches you about certificate management, public-key infrastructure (PKI), and secure device authentication, critical for enterprise and IoT applications.

#### Backend Implementation
```javascript
// backend-cba.js
const express = require('express');
const https = require('https');
const fs = require('fs');
const app = express();

// Validate client certificate
// Input: GET /secure (with client certificate)
// Output: { "message": "Access granted", "subject": "<CN>" }
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
  key: fs.readFileSync('server.key'),
  cert: fs.readFileSync('server.crt'),
  requestCert: true,
  rejectUnauthorized: false // Set true in production
}, app).listen(3009, () => console.log('CBA server on port 3009'));
```

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>Certificate Auth</title>
</head>
<body>
  <h1>Certificate Auth</h1>
  <button onclick="access()">Access</button>
  <pre id="result"></pre>

  <script>
    async function access() {
      // Requires browser configured with client certificate
      const response = await fetch('https://localhost:3009/secure');
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

### 11. Behavioral Biometrics
**Concept**: Authenticates based on user behavior patterns (e.g., typing speed), enhancing security through continuous verification.

**Why It’s Used**: Used by Amazon for fraud detection, providing an additional security layer without user friction.

**Why Learn It**: Behavioral biometrics introduces you to advanced, AI-driven authentication, teaching you about user behavior analysis and fraud prevention.

#### Backend Implementation
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

#### Frontend Implementation
```html
<!DOCTYPE html>
<html>
<head>
  <title>Behavioral Login</title>
</head>
<body>
  <h1>Behavioral Login</h1>
  <input type="text" id="username" placeholder="Username">
  <input type="text" id="typingSpeed" placeholder="Typing Speed">
  <button onclick="login()">Login</button>
  <pre id="result"></pre>

  <script>
    async function login() {
      const username = document.getElementById('username').value;
      const typingSpeed = parseInt(document.getElementById('typingSpeed').value);
      const response = await fetch('http://localhost:3010/login-behavioral', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, typingSpeed })
      });
      const result = await response.json();
      document.getElementById('result').textContent = JSON.stringify(result, null, 2);
    }
  </script>
</body>
</html>
```

## Modern Security Practices
To complement authentication methods, modern security practices ensure robust protection against evolving threats. These practices, adopted by Meta, Amazon, Google, and others, are critical for production systems.

### Zero Trust Architecture
**Concept**: Never trust any request; always verify identity, device, and context.

```javascript
// Security middleware for Zero Trust
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const crypto = require('crypto');

const securityMiddleware = [
  helmet({ contentSecurityPolicy: { directives: { defaultSrc: ["'self'"] } } }),
  rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }),
  (req, res, next) => {
    const fingerprint = crypto.createHash('sha256')
      .update(req.headers['user-agent'] + req.ip)
      .digest('hex');
    req.deviceId = fingerprint;
    next();
  }
];
```

### AI-Powered Risk Assessment
**Concept**: Uses machine learning to analyze user behavior and assign risk scores.

```javascript
// Risk scoring based on behavior
const calculateRiskScore = (req) => {
  let score = 0;
  if (req.geoLocation && req.user.lastLocation) {
    const distance = calculateDistance(req.geoLocation, req.user.lastLocation);
    if (distance > 1000) score += 30;
  }
  const hour = new Date().getHours();
  if (hour < 6 || hour > 22) score += 10;
  if (req.deviceId !== req.user.knownDevices.find(d => d.id === req.deviceId)?.id) {
    score += 20;
  }
  return score;
};
```

### Modern Token Management with Rotation
**Concept**: Securely issues and rotates tokens to minimize exposure.

```javascript
// Token management with rotation
const tokenManager = {
  async issueTokens(userId, req) {
    const accessToken = jwt.sign(
      { userId, type: 'access' },
      process.env.ACCESS_SECRET,
      { expiresIn: '15m' }
    );
    const refreshToken = jwt.sign(
      { userId, type: 'refresh', jti: crypto.randomUUID() },
      process.env.REFRESH_SECRET,
      { expiresIn: '7d' }
    );
    await redis.setex(`refresh:${refreshToken}`, 604800, JSON.stringify({
      userId,
      deviceId: req.deviceId,
      issuedAt: Date.now(),
      ipAddress: req.ip
    }));
    return { accessToken, refreshToken };
  },

  async rotateTokens(refreshToken) {
    const tokenData = await redis.get(`refresh:${refreshToken}`);
    if (!tokenData) throw new Error('Invalid refresh token');
    await redis.del(`refresh:${refreshToken}`);
    return this.issueTokens(JSON.parse(tokenData).userId);
  }
};
```

### Adaptive Authentication Flow
**Concept**: Adjusts authentication requirements based on risk scores.

```javascript
// Risk-based authentication
app.post('/adaptive-login', async (req, res) => {
  const { username, password } = req.body;
  const user = await User.findOne({ username });
  if (!user || !(await bcrypt.compare(password, user.password))) {
    return res.status(401).json({ message: 'Invalid credentials' });
  }
  const riskScore = calculateRiskScore(req);
  if (riskScore < 20) {
    const tokens = await tokenManager.issueTokens(user.id, req);
    res.json({ ...tokens, stepUp: false });
  } else if (riskScore < 50) {
    res.json({ requireMFA: true, challenge: generateMFAChallenge(user) });
  } else {
    res.status(403).json({ message: 'Additional verification required', requireVerification: true });
  }
});
```

### Privacy-Preserving Analytics
**Concept**: Analyzes user behavior while protecting privacy using homomorphic encryption.

```javascript
// Privacy-preserving analytics
const privacyAnalytics = {
  async trackLoginPattern(userId, success) {
    const encryptedUserId = await homomorphicEncrypt(userId);
    const encryptedSuccess = await homomorphicEncrypt(success ? 1 : 0);
    await analytics.track({
      userId: encryptedUserId,
      success: encryptedSuccess,
      timestamp: Date.now()
    });
  }
};
```

### Decentralized Identity (DID) Integration
**Concept**: Uses blockchain-based DIDs for self-sovereign identity verification.

```javascript
// Web3 identity verification
const didAuth = {
  async verifyDID(presentation) {
    const { did, proof } = presentation;
    const didDocument = await resolveDID(did);
    const publicKey = didDocument.authentication[0].publicKeyJwk;
    const isValid = await verifyJWS(proof, publicKey);
    if (isValid) {
      return { authenticated: true, did };
    }
    throw new Error('Invalid DID presentation');
  }
};
```

### Rate Limiting Implementation
**Concept**: Limits request frequency to prevent abuse.

```javascript
// backend-rate-limiting.js
const express = require('express');
const rateLimit = require('express-rate-limit');
const app = express();

app.use(express.json());

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // 100 requests per window
  message: { error: 'Too many requests from this IP' },
  standardHeaders: true,
  legacyHeaders: false
});

// Apply rate limiting
app.post('/login', limiter, (req, res) => {
  res.json({ message: 'Login successful' });
});

app.listen(3011, () => console.log('Rate Limiting server on port 3011'));
```

### Key Security Principles
- **Zero Trust**: Verify every request (Google’s BeyondCorp).
- **Risk-Based Authentication**: Use AI for adaptive security (Amazon GuardDuty).
- **Privacy-First**: Employ differential privacy and homomorphic encryption.
- **Passwordless Future**: Adopt WebAuthn and biometrics (Google, Apple).
- **Decentralized Identity**: Use DIDs for self-sovereign identity.
- **Continuous Authentication**: Monitor behavior in real-time (Amazon).
- **Quantum-Ready**: Prepare for post-quantum cryptography.
- **Edge Security**: Distribute authentication via CDNs.
- **Contextual Access**: Enforce location, device, and time-based policies.
- **Token Binding**: Bind tokens cryptographically to devices.

### Production Deployment Checklist
- ✅ HTTPS with HSTS headers
- ✅ Rate limiting and DDoS protection
- ✅ Secrets management (AWS KMS, HashiCorp Vault)
- ✅ Multi-region deployment
- ✅ Real-time monitoring and alerting
- ✅ GDPR/CCPA compliance
- ✅ Regular penetration testing
- ✅ Incident response plan
- ✅ Audit logging with SIEM
- ✅ Automated security updates

## Additional Notes
- **Security**: Use random, long secrets stored in environment variables. Implement HTTPS, token revocation, and secure hardware for CBA.
- **Scalability**: Use Redis for sessions and token storage. Register OAuth/SSO apps with providers.
- **MFA**: Consider QR codes for TOTP setup or SMS OTP.
- **Passkeys**: Requires modern browsers; use WebAuthn libraries in production.
- **CBA**: Generate certificates with OpenSSL; use CA verification.
- **Behavioral Biometrics**: Integrate services like BioCatch for production.
- **Extensibility**: Add OpenID Connect or SAML for enterprise use cases.
- **Compliance**: Adhere to GDPR, SOC 2, and ISO 27001.
