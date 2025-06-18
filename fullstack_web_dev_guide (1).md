# Complete Full-Stack Web Development Guide

## Project Overview
This is a comprehensive full-stack web application demonstrating modern web development technologies including HTML5, CSS3, JavaScript ES6+, React, Node.js, Express.js with complete authentication and security implementation.

## File Structure

```
fullstack-webapp/
├── client/                          # React Frontend
│   ├── public/
│   │   ├── index.html
│   │   └── favicon.ico
│   ├── src/
│   │   ├── components/
│   │   │   ├── Auth/
│   │   │   │   ├── Login.jsx
│   │   │   │   ├── Register.jsx
│   │   │   │   └── GoogleAuth.jsx
│   │   │   ├── Dashboard/
│   │   │   │   ├── Dashboard.jsx
│   │   │   │   └── UserProfile.jsx
│   │   │   ├── Layout/
│   │   │   │   ├── Header.jsx
│   │   │   │   ├── Footer.jsx
│   │   │   │   └── Sidebar.jsx
│   │   │   └── Common/
│   │   │       ├── Button.jsx
│   │   │       ├── Input.jsx
│   │   │       └── Modal.jsx
│   │   ├── pages/
│   │   │   ├── Home.jsx
│   │   │   ├── About.jsx
│   │   │   └── Contact.jsx
│   │   ├── hooks/
│   │   │   ├── useAuth.js
│   │   │   └── useApi.js
│   │   ├── services/
│   │   │   ├── api.js
│   │   │   └── auth.js
│   │   ├── store/                   # Redux Store
│   │   │   ├── index.js
│   │   │   ├── authSlice.js
│   │   │   └── userSlice.js
│   │   ├── styles/
│   │   │   ├── globals.css
│   │   │   ├── components.scss
│   │   │   └── variables.scss
│   │   ├── utils/
│   │   │   ├── constants.js
│   │   │   └── helpers.js
│   │   ├── App.jsx
│   │   └── index.js
│   ├── package.json
│   └── tailwind.config.js
├── server/                          # Node.js Backend
│   ├── controllers/                 # MVC Controllers
│   │   ├── authController.js
│   │   ├── userController.js
│   │   └── dashboardController.js
│   ├── models/                      # MVC Models
│   │   ├── User.js
│   │   ├── Session.js
│   │   └── RefreshToken.js
│   ├── views/                       # MVC Views (EJS Templates)
│   │   ├── layouts/
│   │   │   ├── main.ejs
│   │   │   └── auth.ejs
│   │   ├── auth/
│   │   │   ├── login.ejs
│   │   │   ├── register.ejs
│   │   │   └── forgot-password.ejs
│   │   ├── dashboard/
│   │   │   ├── index.ejs
│   │   │   └── profile.ejs
│   │   ├── partials/
│   │   │   ├── header.ejs
│   │   │   ├── footer.ejs
│   │   │   └── navbar.ejs
│   │   └── errors/
│   │       ├── 404.ejs
│   │       └── 500.ejs
│   ├── routes/                      # Route Handlers
│   │   ├── authRoutes.js
│   │   ├── userRoutes.js
│   │   ├── apiRoutes.js
│   │   └── viewRoutes.js
│   ├── middleware/                  # Custom Middleware
│   │   ├── auth.js
│   │   ├── validation.js
│   │   ├── rateLimit.js
│   │   ├── cors.js
│   │   ├── errorHandler.js
│   │   └── logger.js
│   ├── config/                      # Configuration Files
│   │   ├── database.js
│   │   ├── passport.js
│   │   ├── jwt.js
│   │   └── oauth.js
│   ├── services/                    # Business Logic Services
│   │   ├── authService.js
│   │   ├── emailService.js
│   │   ├── tokenService.js
│   │   └── userService.js
│   ├── utils/                       # Utility Functions
│   │   ├── encryption.js
│   │   ├── validation.js
│   │   ├── logger.js
│   │   └── helpers.js
│   ├── security/                    # Security Related Files
│   │   ├── rateLimiter.js
│   │   ├── sanitizer.js
│   │   └── encryption.js
│   ├── app.js                       # Express App Configuration
│   ├── server.js                    # Server Entry Point
│   └── package.json
├── database/                        # Database Scripts
│   ├── migrations/
│   ├── seeders/
│   └── init.sql
├── docs/                           # Documentation
│   ├── API.md
│   ├── SETUP.md
│   └── DEPLOYMENT.md
├── .env.example
├── .gitignore
├── docker-compose.yml
├── Dockerfile
└── README.md
```

## Why This Structure?

### Frontend Structure Reasoning:
- **`components/`**: Reusable UI components following atomic design
- **`pages/`**: Route-level components representing full views
- **`hooks/`**: Custom React hooks for logic reuse
- **`store/`**: Redux store for global state management
- **`services/`**: API calls and external service integrations
- **`styles/`**: SCSS/CSS organization with variables and mixins

### Backend Structure Reasoning:
- **MVC Pattern**: Separates concerns (Model-View-Controller)
- **`middleware/`**: Reusable functions that process requests
- **`config/`**: Environment and service configurations
- **`services/`**: Business logic separated from controllers
- **`security/`**: Centralized security implementations

---

## Package.json Files

### Client package.json
```json
{
  "name": "fullstack-client",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "@reduxjs/toolkit": "^1.9.5",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-redux": "^8.1.1",
    "react-router-dom": "^6.14.1",
    "axios": "^1.4.0",
    "tailwindcss": "^3.3.3",
    "sass": "^1.63.6",
    "bootstrap": "^5.3.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "devDependencies": {
    "react-scripts": "5.0.1"
  }
}
```

### Server package.json
```json
{
  "name": "fullstack-server",
  "version": "1.0.0",
  "main": "server.js",
  "type": "module",
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.3.1",
    "mysql2": "^3.5.2",
    "bcryptjs": "^2.4.3",
    "jsonwebtoken": "^9.0.1",
    "passport": "^0.6.0",
    "passport-local": "^1.0.0",
    "passport-google-oauth20": "^2.0.0",
    "passport-jwt": "^4.0.1",
    "express-session": "^1.17.3",
    "express-rate-limit": "^6.8.1",
    "helmet": "^7.0.0",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "ejs": "^3.1.9",
    "express-validator": "^7.0.1",
    "nodemailer": "^6.9.3",
    "crypto": "^1.0.1",
    "compression": "^1.7.4",
    "morgan": "^1.10.0"
  },
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "jest"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.6.1"
  }
}
```

---

## Environment Configuration

### .env.example
```env
# Server Configuration
NODE_ENV=development
PORT=5000
CLIENT_URL=http://localhost:3000

# Database Configuration - Choose one
# MongoDB
MONGODB_URI=mongodb://localhost:27017/fullstack_app
# MySQL
MYSQL_HOST=localhost
MYSQL_USER=root
MYSQL_PASSWORD=password
MYSQL_DATABASE=fullstack_app

# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key_here_change_in_production
JWT_REFRESH_SECRET=your_refresh_token_secret_here
JWT_EXPIRE=1h
JWT_REFRESH_EXPIRE=7d

# Session Configuration
SESSION_SECRET=your_session_secret_here_change_in_production

# Google OAuth Configuration
GOOGLE_CLIENT_ID=your_google_client_id_here
GOOGLE_CLIENT_SECRET=your_google_client_secret_here

# Email Configuration
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587

# Security
BCRYPT_SALT_ROUNDS=12
```

---

## Backend Implementation

### 1. Server Entry Point (server.js)
```javascript
// ES6 Import syntax (Modern)
import express from 'express';
import dotenv from 'dotenv';
import cors from 'cors';
import helmet from 'helmet';
import compression from 'compression';
import morgan from 'morgan';

// Traditional CommonJS syntax (commented)
// const express = require('express');
// const dotenv = require('dotenv');

import app from './app.js';
import { connectMongoDB, connectMySQL } from './config/database.js';
import logger from './utils/logger.js';

// Load environment variables
dotenv.config();

const PORT = process.env.PORT || 5000;

// Database Connection
const initializeDatabase = async () => {
    try {
        // Choose database (MongoDB or MySQL)
        if (process.env.MONGODB_URI) {
            await connectMongoDB();
            logger.info('MongoDB connected successfully');
        } else {
            await connectMySQL();
            logger.info('MySQL connected successfully');
        }
    } catch (error) {
        logger.error('Database connection failed:', error);
        process.exit(1);
    }
};

// Server startup
const startServer = async () => {
    await initializeDatabase();
    
    app.listen(PORT, () => {
        logger.info(`Server running on port ${PORT} in ${process.env.NODE_ENV} mode`);
        logger.info(`Client URL: ${process.env.CLIENT_URL}`);
    });
};

// Graceful shutdown
process.on('SIGTERM', () => {
    logger.info('SIGTERM received. Shutting down gracefully...');
    process.exit(0);
});

startServer().catch(error => {
    logger.error('Failed to start server:', error);
    process.exit(1);
});
```

### 2. Express App Configuration (app.js)
```javascript
import express from 'express';
import session from 'express-session';
import passport from 'passport';
import path from 'path';
import { fileURLToPath } from 'url';

// Import middleware
import corsMiddleware from './middleware/cors.js';
import rateLimitMiddleware from './middleware/rateLimit.js';
import authMiddleware from './middleware/auth.js';
import errorHandler from './middleware/errorHandler.js';
import loggerMiddleware from './middleware/logger.js';

// Import routes
import authRoutes from './routes/authRoutes.js';
import userRoutes from './routes/userRoutes.js';
import apiRoutes from './routes/apiRoutes.js';
import viewRoutes from './routes/viewRoutes.js';

// Import configurations
import './config/passport.js';

// ES6 way to get __dirname
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const app = express();

// Trust proxy for rate limiting behind reverse proxy
app.set('trust proxy', 1);

// View engine setup (EJS)
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// Static files
app.use(express.static(path.join(__dirname, 'public')));

// Security middleware
app.use(helmet({
    contentSecurityPolicy: {
        directives: {
            defaultSrc: ["'self'"],
            styleSrc: ["'self'", "'unsafe-inline'", "https://cdn.jsdelivr.net"],
            scriptSrc: ["'self'", "https://apis.google.com"],
            imgSrc: ["'self'", "data:", "https:"],
        },
    },
}));

// CORS middleware
app.use(corsMiddleware);

// Compression middleware
app.use(compression());

// Logging middleware
app.use(loggerMiddleware);

// Body parsing middleware
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true, limit: '10mb' }));

// Session configuration
app.use(session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false,
    cookie: {
        secure: process.env.NODE_ENV === 'production', // HTTPS only in production
        httpOnly: true, // Prevent XSS
        maxAge: 24 * 60 * 60 * 1000, // 24 hours
        sameSite: 'strict' // CSRF protection
    }
}));

// Passport middleware
app.use(passport.initialize());
app.use(passport.session());

// Rate limiting
app.use(rateLimitMiddleware);

// Routes
app.use('/auth', authRoutes);
app.use('/api/users', userRoutes);
app.use('/api', apiRoutes);
app.use('/', viewRoutes);

// Health check endpoint
app.get('/health', (req, res) => {
    res.status(200).json({
        status: 'OK',
        timestamp: new Date().toISOString(),
        uptime: process.uptime(),
        environment: process.env.NODE_ENV
    });
});

// 404 handler
app.use('*', (req, res) => {
    if (req.accepts('html')) {
        res.status(404).render('errors/404', { 
            title: 'Page Not Found',
            url: req.originalUrl 
        });
    } else if (req.accepts('json')) {
        res.status(404).json({ error: 'Not Found' });
    } else {
        res.status(404).type('txt').send('Not Found');
    }
});

// Error handling middleware (must be last)
app.use(errorHandler);

export default app;
```

### 3. Database Configuration (config/database.js)
```javascript
import mongoose from 'mongoose';
import mysql from 'mysql2/promise';
import logger from '../utils/logger.js';

// MongoDB Connection
export const connectMongoDB = async () => {
    try {
        const options = {
            useNewUrlParser: true,
            useUnifiedTopology: true,
            maxPoolSize: 10, // Maximum number of connections
            serverSelectionTimeoutMS: 5000, // Keep trying to send operations for 5 seconds
            socketTimeoutMS: 45000, // Close sockets after 45 seconds of inactivity
        };

        await mongoose.connect(process.env.MONGODB_URI, options);
        
        // Connection event listeners
        mongoose.connection.on('connected', () => {
            logger.info('MongoDB connected');
        });

        mongoose.connection.on('error', (err) => {
            logger.error('MongoDB connection error:', err);
        });

        mongoose.connection.on('disconnected', () => {
            logger.warn('MongoDB disconnected');
        });

    } catch (error) {
        logger.error('MongoDB connection failed:', error);
        throw error;
    }
};

// MySQL Connection
let mysqlPool;

export const connectMySQL = async () => {
    try {
        mysqlPool = mysql.createPool({
            host: process.env.MYSQL_HOST,
            user: process.env.MYSQL_USER,
            password: process.env.MYSQL_PASSWORD,
            database: process.env.MYSQL_DATABASE,
            connectionLimit: 10,
            acquireTimeout: 60000,
            timeout: 60000,
            reconnect: true
        });

        // Test the connection
        const connection = await mysqlPool.getConnection();
        await connection.ping();
        connection.release();
        
        logger.info('MySQL connected successfully');
    } catch (error) {
        logger.error('MySQL connection failed:', error);
        throw error;
    }
};

export const getMySQLPool = () => mysqlPool;

// Graceful shutdown
process.on('SIGINT', async () => {
    try {
        if (mongoose.connection.readyState === 1) {
            await mongoose.connection.close();
            logger.info('MongoDB connection closed');
        }
        
        if (mysqlPool) {
            await mysqlPool.end();
            logger.info('MySQL connection pool closed');
        }
    } catch (error) {
        logger.error('Error during database cleanup:', error);
    }
    process.exit(0);
});
```

### 4. Models

#### MongoDB User Model (models/User.js)
```javascript
import mongoose from 'mongoose';
import bcrypt from 'bcryptjs';

// Traditional function syntax vs Arrow function
// Traditional: function(next) { ... }
// ES6 Arrow: (next) => { ... }

const userSchema = new mongoose.Schema({
    username: {
        type: String,
        required: [true, 'Username is required'],
        unique: true,
        trim: true,
        minlength: [3, 'Username must be at least 3 characters'],
        maxlength: [30, 'Username cannot exceed 30 characters']
    },
    email: {
        type: String,
        required: [true, 'Email is required'],
        unique: true,
        lowercase: true,
        trim: true,
        match: [/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/, 'Please enter a valid email']
    },
    password: {
        type: String,
        required: function() {
            return !this.googleId; // Password not required for Google OAuth users
        },
        minlength: [6, 'Password must be at least 6 characters']
    },
    // Google OAuth fields
    googleId: {
        type: String,
        sparse: true // Allows multiple null values
    },
    googleProfile: {
        name: String,
        picture: String
    },
    // Profile fields
    profile: {
        firstName: String,
        lastName: String,
        avatar: String,
        bio: String,
        phone: String,
        dateOfBirth: Date
    },
    // Security fields
    isEmailVerified: {
        type: Boolean,
        default: false
    },
    emailVerificationToken: String,
    passwordResetToken: String,
    passwordResetExpires: Date,
    loginAttempts: {
        type: Number,
        default: 0
    },
    lockUntil: Date,
    // Audit fields
    lastLogin: Date,
    isActive: {
        type: Boolean,
        default: true
    },
    role: {
        type: String,
        enum: ['user', 'admin', 'moderator'],
        default: 'user'
    }
}, {
    timestamps: true, // Adds createdAt and updatedAt
    toJSON: {
        transform: function(doc, ret) {
            delete ret.password;
            delete ret.emailVerificationToken;
            delete ret.passwordResetToken;
            delete ret.__v;
            return ret;
        }
    }
});

// Indexes for performance
userSchema.index({ email: 1 });
userSchema.index({ username: 1 });
userSchema.index({ googleId: 1 });

// Virtual for account lock status
userSchema.virtual('isLocked').get(function() {
    return !!(this.lockUntil && this.lockUntil > Date.now());
});

// Pre-save middleware for password hashing
userSchema.pre('save', async function(next) {
    // Traditional function syntax needed for 'this' binding
    // Arrow functions don't have their own 'this'
    
    if (!this.isModified('password')) return next();
    
    try {
        // Generate salt and hash password
        const salt = await bcrypt.genSalt(parseInt(process.env.BCRYPT_SALT_ROUNDS) || 12);
        this.password = await bcrypt.hash(this.password, salt);
        next();
    } catch (error) {
        next(error);
    }
});

// Instance method to compare password
userSchema.methods.comparePassword = async function(candidatePassword) {
    if (!this.password) return false;
    return bcrypt.compare(candidatePassword, this.password);
};

// Instance method to increment login attempts
userSchema.methods.incLoginAttempts = function() {
    // If we have a previous lock that has expired, restart at 1
    if (this.lockUntil && this.lockUntil < Date.now()) {
        return this.updateOne({
            $unset: { lockUntil: 1 },
            $set: { loginAttempts: 1 }
        });
    }
    
    const updates = { $inc: { loginAttempts: 1 } };
    
    // If we're at max attempts and not locked, lock the account
    if (this.loginAttempts + 1 >= 5 && !this.isLocked) {
        updates.$set = { lockUntil: Date.now() + 2 * 60 * 60 * 1000 }; // 2 hours
    }
    
    return this.updateOne(updates);
};

// Static method to find by credentials
userSchema.statics.findByCredentials = async function(email, password) {
    const user = await this.findOne({ email, isActive: true });
    
    if (!user) {
        throw new Error('Invalid credentials');
    }
    
    if (user.isLocked) {
        throw new Error('Account temporarily locked due to too many failed login attempts');
    }
    
    const isMatch = await user.comparePassword(password);
    
    if (!isMatch) {
        await user.incLoginAttempts();
        throw new Error('Invalid credentials');
    }
    
    // Reset login attempts on successful login
    if (user.loginAttempts > 0) {
        await user.updateOne({
            $unset: { loginAttempts: 1, lockUntil: 1 },
            $set: { lastLogin: new Date() }
        });
    }
    
    return user;
};

export default mongoose.model('User', userSchema);
```

#### MySQL User Model Alternative (commented)
```javascript
/*
// MySQL User Model Alternative
import { getMySQLPool } from '../config/database.js';
import bcrypt from 'bcryptjs';

class User {
    constructor(userData) {
        this.username = userData.username;
        this.email = userData.email;
        this.password = userData.password;
        this.googleId = userData.googleId;
        this.profile = userData.profile || {};
        this.isEmailVerified = userData.isEmailVerified || false;
        this.isActive = userData.isActive || true;
        this.role = userData.role || 'user';
    }

    async save() {
        const pool = getMySQLPool();
        
        // Hash password if provided
        if (this.password) {
            const salt = await bcrypt.genSalt(12);
            this.password = await bcrypt.hash(this.password, salt);
        }

        const query = `
            INSERT INTO users (username, email, password, google_id, first_name, last_name, 
                             is_email_verified, is_active, role, created_at, updated_at)
            VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, NOW(), NOW())
        `;
        
        const values = [
            this.username,
            this.email,
            this.password,
            this.googleId,
            this.profile.firstName,
            this.profile.lastName,
            this.isEmailVerified,
            this.isActive,
            this.role
        ];

        const [result] = await pool.execute(query, values);
        this.id = result.insertId;
        return this;
    }

    static async findByEmail(email) {
        const pool = getMySQLPool();
        const query = 'SELECT * FROM users WHERE email = ? AND is_active = 1';
        const [rows] = await pool.execute(query, [email]);
        return rows[0] ? new User(rows[0]) : null;
    }

    async comparePassword(candidatePassword) {
        if (!this.password) return false;
        return bcrypt.compare(candidatePassword, this.password);
    }
}

export default User;
*/
```

### 5. Security & Middleware

#### Authentication Middleware (middleware/auth.js)
```javascript
import jwt from 'jsonwebtoken';
import User from '../models/User.js';
import logger from '../utils/logger.js';

// JWT Authentication Middleware
export const authenticateJWT = async (req, res, next) => {
    try {
        // Get token from header
        const authHeader = req.headers.authorization;
        
        if (!authHeader || !authHeader.startsWith('Bearer ')) {
            return res.status(401).json({ 
                error: 'Access denied. No token provided.' 
            });
        }

        const token = authHeader.substring(7); // Remove 'Bearer ' prefix

        // Verify token
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        
        // Find user
        const user = await User.findById(decoded.userId).select('-password');
        
        if (!user || !user.isActive) {
            return res.status(401).json({ 
                error: 'Invalid token. User not found or inactive.' 
            });
        }

        // Add user to request object
        req.user = user;
        next();

    } catch (error) {
        logger.error('JWT Authentication error:', error);
        
        if (error.name === 'TokenExpiredError') {
            return res.status(401).json({ 
                error: 'Token expired.',
                code: 'TOKEN_EXPIRED'
            });
        }
        
        if (error.name === 'JsonWebTokenError') {
            return res.status(401).json({ 
                error: 'Invalid token.',
                code: 'INVALID_TOKEN'
            });
        }

        res.status(500).json({ 
            error: 'Internal server error during authentication.' 
        });
    }
};

// Session Authentication Middleware
export const authenticateSession = (req, res, next) => {
    if (req.isAuthenticated()) {
        return next();
    }
    
    // For API requests, return JSON error
    if (req.xhr || req.headers.accept?.indexOf('json') > -1) {
        return res.status(401).json({ 
            error: 'Authentication required' 
        });
    }
    
    // For web requests, redirect to login
    req.session.returnTo = req.originalUrl;
    res.redirect('/auth/login');
};

// Role-based Authorization Middleware
export const authorize = (...roles) => {
    return (req, res, next) => {
        if (!req.user) {
            return res.status(401).json({ 
                error: 'Authentication required' 
            });
        }

        if (!roles.includes(req.user.role)) {
            return res.status(403).json({ 
                error: 'Insufficient permissions' 
            });
        }

        next();
    };
};

// Optional Authentication (doesn't fail if no token)
export const optionalAuth = async (req, res, next) => {
    try {
        const authHeader = req.headers.authorization;
        
        if (authHeader && authHeader.startsWith('Bearer ')) {
            const token = authHeader.substring(7);
            const decoded = jwt.verify(token, process.env.JWT_SECRET);
            const user = await User.findById(decoded.userId).select('-password');
            
            if (user && user.isActive) {
                req.user = user;
            }
        }
    } catch (error) {
        // Silently fail for optional auth
        logger.debug('Optional auth failed:', error.message);
    }
    
    next();
};

// Admin Only Middleware
export const adminOnly = authorize('admin');

// User or Admin Middleware
export const userOrAdmin = authorize('user', 'admin', 'moderator');
```

#### Rate Limiting Middleware (middleware/rateLimit.js)
```javascript
import rateLimit from 'express-rate-limit';
import logger from '../utils/logger.js';

// General rate limiting
export const generalLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // Limit each IP to 100 requests per windowMs
    message: {
        error: 'Too many requests from this IP, please try again later.',
        retryAfter: Math.ceil(15 * 60 / 60) // in minutes
    },
    standardHeaders: true, // Return rate limit info in headers
    legacyHeaders: false, // Disable X-RateLimit-* headers
    handler: (req, res) => {
        logger.warn(`Rate limit exceeded for IP: ${req.ip}`);
        res.status(429).json({
            error: 'Too many requests from this IP, please try again later.',
            retryAfter: Math.ceil(15 * 60 / 60)
        });
    }
});

// Strict rate limiting for authentication endpoints
export const authLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 5, // Limit each IP to 5 requests per windowMs
    message: {
        error: 'Too many authentication attempts, please try again later.',
        retryAfter: Math.ceil(15 * 60 / 60)
    },
    standardHeaders: true,
    legacyHeaders: false,
    skipSuccessfulRequests: true, // Don't count successful requests
    handler: (req, res) => {
        logger.warn(`Auth rate limit exceeded for IP: ${req.ip}`);
        res.status(429).json({
            error: 'Too many authentication attempts, please try again later.',
            retryAfter: Math.ceil(15 * 60 / 60)
        });
    }
});

// API rate limiting
export const apiLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 1000, // Higher limit for API endpoints
    message: {
        error: 'API rate limit exceeded, please try again later.',
        retryAfter: Math.ceil(15 * 60 / 60)
    }
});

export default generalLimiter;

#### Validation Middleware (middleware/validation.js)
```javascript
import { body, validationResult } from 'express-validator';
import logger from '../utils/logger.js';

// Handle validation errors
export const handleValidationErrors = (req, res, next) => {
    const errors = validationResult(req);
    
    if (!errors.isEmpty()) {
        logger.warn('Validation errors:', errors.array());
        return res.status(400).json({
            error: 'Validation failed',
            details: errors.array()
        });
    }
    
    next();
};

// User registration validation
export const validateRegistration = [
    body('username')
        .trim()
        .isLength({ min: 3, max: 30 })
        .withMessage('Username must be between 3 and 30 characters')
        .matches(/^[a-zA-Z0-9_]+$/)
        .withMessage('Username can only contain letters, numbers, and underscores'),
    
    body('email')
        .trim()
        .isEmail()
        .normalizeEmail()
        .withMessage('Please provide a valid email address'),
    
    body('password')
        .isLength({ min: 6 })
        .withMessage('Password must be at least 6 characters long')
        .matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)
        .withMessage('Password must contain at least one lowercase letter, one uppercase letter, and one number'),
    
    body('confirmPassword')
        .custom((value, { req }) => {
            if (value !== req.body.password) {
                throw new Error('Password confirmation does not match password');
            }
            return true;
        }),
    
    handleValidationErrors
];

// User login validation
export const validateLogin = [
    body('email')
        .trim()
        .isEmail()
        .normalizeEmail()
        .withMessage('Please provide a valid email address'),
    
    body('password')
        .notEmpty()
        .withMessage('Password is required'),
    
    handleValidationErrors
];

// Profile update validation
export const validateProfileUpdate = [
    body('firstName')
        .optional()
        .trim()
        .isLength({ min: 1, max: 50 })
        .withMessage('First name must be between 1 and 50 characters'),
    
    body('lastName')
        .optional()
        .trim()
        .isLength({ min: 1, max: 50 })
        .withMessage('Last name must be between 1 and 50 characters'),
    
    body('bio')
        .optional()
        .trim()
        .isLength({ max: 500 })
        .withMessage('Bio cannot exceed 500 characters'),
    
    body('phone')
        .optional()
        .isMobilePhone()
        .withMessage('Please provide a valid phone number'),
    
    handleValidationErrors
];

// Password change validation
export const validatePasswordChange = [
    body('currentPassword')
        .notEmpty()
        .withMessage('Current password is required'),
    
    body('newPassword')
        .isLength({ min: 6 })
        .withMessage('New password must be at least 6 characters long')
        .matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)
        .withMessage('New password must contain at least one lowercase letter, one uppercase letter, and one number'),
    
    body('confirmNewPassword')
        .custom((value, { req }) => {
            if (value !== req.body.newPassword) {
                throw new Error('New password confirmation does not match new password');
            }
            return true;
        }),
    
    handleValidationErrors
];
```

#### CORS Middleware (middleware/cors.js)
```javascript
import cors from 'cors';

const corsOptions = {
    origin: function (origin, callback) {
        // Allow requests with no origin (mobile apps, curl, etc.)
        if (!origin) return callback(null, true);
        
        const allowedOrigins = [
            process.env.CLIENT_URL,
            'http://localhost:3000', // React dev server
            'http://localhost:3001', // Alternative React port
            'http://127.0.0.1:3000',
        ];
        
        if (process.env.NODE_ENV === 'development') {
            // Allow any localhost in development
            if (origin.includes('localhost') || origin.includes('127.0.0.1')) {
                return callback(null, true);
            }
        }
        
        if (allowedOrigins.indexOf(origin) === -1) {
            const msg = 'The CORS policy for this site does not allow access from the specified Origin.';
            return callback(new Error(msg), false);
        }
        
        return callback(null, true);
    },
    credentials: true, // Allow cookies to be sent
    methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS'],
    allowedHeaders: [
        'Origin',
        'X-Requested-With',
        'Content-Type',
        'Accept',
        'Authorization',
        'Cache-Control',
        'X-Access-Token'
    ],
    exposedHeaders: ['X-Total-Count'], // For pagination info
    optionsSuccessStatus: 200 // For legacy browser support
};

export default cors(corsOptions);
```

#### Error Handler Middleware (middleware/errorHandler.js)
```javascript
import logger from '../utils/logger.js';

// Error handling middleware
const errorHandler = (err, req, res, next) => {
    logger.error('Error:', {
        message: err.message,
        stack: err.stack,
        url: req.url,
        method: req.method,
        ip: req.ip,
        userAgent: req.get('User-Agent')
    });

    // Mongoose validation error
    if (err.name === 'ValidationError') {
        const errors = Object.values(err.errors).map(val => val.message);
        return res.status(400).json({
            error: 'Validation Error',
            details: errors
        });
    }

    // Mongoose duplicate key error
    if (err.code === 11000) {
        const field = Object.keys(err.keyValue)[0];
        return res.status(400).json({
            error: `${field} already exists`,
            field
        });
    }

    // JWT errors
    if (err.name === 'JsonWebTokenError') {
        return res.status(401).json({
            error: 'Invalid token'
        });
    }

    if (err.name === 'TokenExpiredError') {
        return res.status(401).json({
            error: 'Token expired'
        });
    }

    // CORS errors
    if (err.message && err.message.includes('CORS')) {
        return res.status(403).json({
            error: 'CORS policy violation'
        });
    }

    // Default error
    const statusCode = err.statusCode || 500;
    const message = process.env.NODE_ENV === 'production' 
        ? 'Internal Server Error' 
        : err.message;

    res.status(statusCode).json({
        error: message,
        ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    });
};

export default errorHandler;
```

### 6. Passport Configuration (config/passport.js)
```javascript
import passport from 'passport';
import { Strategy as LocalStrategy } from 'passport-local';
import { Strategy as GoogleStrategy } from 'passport-google-oauth20';
import { Strategy as JwtStrategy, ExtractJwt } from 'passport-jwt';
import User from '../models/User.js';
import logger from '../utils/logger.js';

// Local Strategy (Username/Password)
passport.use(new LocalStrategy({
    usernameField: 'email', // Use email instead of username
    passwordField: 'password'
}, async (email, password, done) => {
    try {
        const user = await User.findByCredentials(email, password);
        return done(null, user);
    } catch (error) {
        logger.error('Local strategy error:', error);
        return done(null, false, { message: error.message });
    }
}));

// Google OAuth Strategy
passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "/auth/google/callback"
}, async (accessToken, refreshToken, profile, done) => {
    try {
        // Check if user already exists with this Google ID
        let user = await User.findOne({ googleId: profile.id });
        
        if (user) {
            return done(null, user);
        }
        
        // Check if user exists with the same email
        user = await User.findOne({ email: profile.emails[0].value });
        
        if (user) {
            // Link Google account to existing user
            user.googleId = profile.id;
            user.googleProfile = {
                name: profile.displayName,
                picture: profile.photos[0]?.value
            };
            user.isEmailVerified = true; // Google emails are verified
            await user.save();
            return done(null, user);
        }
        
        // Create new user
        user = new User({
            googleId: profile.id,
            email: profile.emails[0].value,
            username: profile.emails[0].value.split('@')[0], // Use email prefix as username
            googleProfile: {
                name: profile.displayName,
                picture: profile.photos[0]?.value
            },
            profile: {
                firstName: profile.name?.givenName,
                lastName: profile.name?.familyName,
                avatar: profile.photos[0]?.value
            },
            isEmailVerified: true
        });
        
        await user.save();
        return done(null, user);
        
    } catch (error) {
        logger.error('Google strategy error:', error);
        return done(error, null);
    }
}));

// JWT Strategy
passport.use(new JwtStrategy({
    jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
    secretOrKey: process.env.JWT_SECRET
}, async (payload, done) => {
    try {
        const user = await User.findById(payload.userId).select('-password');
        
        if (user && user.isActive) {
            return done(null, user);
        }
        
        return done(null, false);
    } catch (error) {
        logger.error('JWT strategy error:', error);
        return done(error, false);
    }
}));

// Serialize user for session
passport.serializeUser((user, done) => {
    done(null, user._id);
});

// Deserialize user from session
passport.deserializeUser(async (id, done) => {
    try {
        const user = await User.findById(id).select('-password');
        done(null, user);
    } catch (error) {
        logger.error('Deserialize user error:', error);
        done(error, null);
    }
});

export default passport;
```

### 7. Controllers

#### Auth Controller (controllers/authController.js)
```javascript
import jwt from 'jsonwebtoken';
import crypto from 'crypto';
import User from '../models/User.js';
import { sendVerificationEmail, sendpasswordResetEmail } from '../services/emailService.js';
import logger from '../utils/logger.js';

// Generate JWT tokens
const generateTokens = (userId) => {
    const accessToken = jwt.sign(
        { userId },
        process.env.JWT_SECRET,
        { expiresIn: process.env.JWT_EXPIRE || '1h' }
    );
    
    const refreshToken = jwt.sign(
        { userId },
        process.env.JWT_REFRESH_SECRET,
        { expiresIn: process.env.JWT_REFRESH_EXPIRE || '7d' }
    );
    
    return { accessToken, refreshToken };
};

// Register new user
export const register = async (req, res) => {
    try {
        const { username, email, password } = req.body;
        
        // Check if user already exists
        const existingUser = await User.findOne({
            $or: [{ email }, { username }]
        });
        
        if (existingUser) {
            return res.status(400).json({
                error: existingUser.email === email 
                    ? 'Email already registered' 
                    : 'Username already taken'
            });
        }
        
        // Generate email verification token
        const emailVerificationToken = crypto.randomBytes(32).toString('hex');
        
        // Create new user
        const user = new User({
            username,
            email,
            password,
            emailVerificationToken
        });
        
        await user.save();
        
        // Send verification email
        try {
            await sendVerificationEmail(user.email, emailVerificationToken);
        } catch (emailError) {
            logger.error('Failed to send verification email:', emailError);
            // Don't fail registration if email fails
        }
        
        // Generate tokens
        const tokens = generateTokens(user._id);
        
        logger.info(`New user registered: ${user.email}`);
        
        res.status(201).json({
            message: 'User registered successfully. Please check your email for verification.',
            user: {
                id: user._id,
                username: user.username,
                email: user.email,
                isEmailVerified: user.isEmailVerified
            },
            tokens
        });
        
    } catch (error) {
        logger.error('Registration error:', error);
        res.status(500).json({
            error: 'Registration failed. Please try again.'
        });
    }
};

// Login user
export const login = async (req, res) => {
    try {
        const { email, password, rememberMe } = req.body;
        
        // Find user and verify password
        const user = await User.findByCredentials(email, password);
        
        // Update last login
        user.lastLogin = new Date();
        await user.save();
        
        // Generate tokens with different expiry for "remember me"
        const accessTokenExpiry = rememberMe ? '30d' : '1h';
        const accessToken = jwt.sign(
            { userId: user._id },
            process.env.JWT_SECRET,
            { expiresIn: accessTokenExpiry }
        );
        
        const refreshToken = jwt.sign(
            { userId: user._id },
            process.env.JWT_REFRESH_SECRET,
            { expiresIn: '30d' }
        );
        
        // Set session for traditional web auth
        req.login(user, (err) => {
            if (err) {
                logger.error('Session login error:', err);
            }
        });
        
        logger.info(`User logged in: ${user.email}`);
        
        res.json({
            message: 'Login successful',
            user: {
                id: user._id,
                username: user.username,
                email: user.email,
                isEmailVerified: user.isEmailVerified,
                profile: user.profile,
                role: user.role
            },
            tokens: {
                accessToken,
                refreshToken
            }
        });
        
    } catch (error) {
        logger.error('Login error:', error);
        res.status(401).json({
            error: error.message || 'Login failed'
        });
    }
};

// Refresh JWT token
export const refreshToken = async (req, res) => {
    try {
        const { refreshToken } = req.body;
        
        if (!refreshToken) {
            return res.status(401).json({
                error: 'Refresh token required'
            });
        }
        
        // Verify refresh token
        const decoded = jwt.verify(refreshToken, process.env.JWT_REFRESH_SECRET);
        
        // Find user
        const user = await User.findById(decoded.userId);
        
        if (!user || !user.isActive) {
            return res.status(401).json({
                error: 'Invalid refresh token'
            });
        }
        
        // Generate new tokens
        const tokens = generateTokens(user._id);
        
        res.json({
            message: 'Token refreshed successfully',
            tokens
        });
        
    } catch (error) {
        logger.error('Token refresh error:', error);
        res.status(401).json({
            error: 'Invalid refresh token'
        });
    }
};

// Logout user
export const logout = (req, res) => {
    req.logout((err) => {
        if (err) {
            logger.error('Logout error:', err);
        }
    });
    
    req.session.destroy((err) => {
        if (err) {
            logger.error('Session destroy error:', err);
        }
    });
    
    res.json({
        message: 'Logged out successfully'
    });
};

// Google OAuth callback
export const googleCallback = (req, res) => {
    // Generate tokens for Google OAuth user
    const tokens = generateTokens(req.user._id);
    
    // Redirect to frontend with tokens
    const redirectUrl = `${process.env.CLIENT_URL}/auth/callback?` +
        `token=${tokens.accessToken}&refresh=${tokens.refreshToken}`;
    
    res.redirect(redirectUrl);
};

// Verify email
export const verifyEmail = async (req, res) => {
    try {
        const { token } = req.params;
        
        const user = await User.findOne({
            emailVerificationToken: token
        });
        
        if (!user) {
            return res.status(400).json({
                error: 'Invalid or expired verification token'
            });
        }
        
        user.isEmailVerified = true;
        user.emailVerificationToken = undefined;
        await user.save();
        
        logger.info(`Email verified for user: ${user.email}`);
        
        res.json({
            message: 'Email verified successfully'
        });
        
    } catch (error) {
        logger.error('Email verification error:', error);
        res.status(500).json({
            error: 'Email verification failed'
        });
    }
};

// Forgot password
export const forgotPassword = async (req, res) => {
    try {
        const { email } = req.body;
        
        const user = await User.findOne({ email });
        
        if (!user) {
            // Don't reveal if email exists or not
            return res.json({
                message: 'If the email exists, a password reset link has been sent.'
            });
        }
        
        // Generate reset token
        const resetToken = crypto.randomBytes(32).toString('hex');
        user.passwordResetToken = resetToken;
        user.passwordResetExpires = Date.now() + 10 * 60 * 1000; // 10 minutes
        
        await user.save();
        
        // Send reset email
        try {
            await sendPasswordResetEmail(user.email, resetToken);
        } catch (emailError) {
            logger.error('Failed to send password reset email:', emailError);
            user.passwordResetToken = undefined;
            user.passwordResetExpires = undefined;
            await user.save();
            
            return res.status(500).json({
                error: 'Failed to send password reset email'
            });
        }
        
        res.json({
            message: 'If the email exists, a password reset link has been sent.'
        });
        
    } catch (error) {
        logger.error('Forgot password error:', error);
        res.status(500).json({
            error: 'Password reset request failed'
        });
    }
};

// Reset password
export const resetPassword = async (req, res) => {
    try {
        const { token } = req.params;
        const { password } = req.body;
        
        const user = await User.findOne({
            passwordResetToken: token,
            passwordResetExpires: { $gt: Date.now() }
        });
        
        if (!user) {
            return res.status(400).json({
                error: 'Invalid or expired reset token'
            });
        }
        
        user.password = password;
        user.passwordResetToken = undefined;
        user.passwordResetExpires = undefined;
        user.loginAttempts = 0; // Reset login attempts
        user.lockUntil = undefined;
        
        await user.save();
        
        logger.info(`Password reset for user: ${user.email}`);
        
        res.json({
            message: 'Password reset successfully'
        });
        
    } catch (error) {
        logger.error('Password reset error:', error);
        res.status(500).json({
            error: 'Password reset failed'
        });
    }
};

// Change password (for authenticated users)
export const changePassword = async (req, res) => {
    try {
        const { currentPassword, newPassword } = req.body;
        const userId = req.user._id;
        
        const user = await User.findById(userId);
        
        // Verify current password
        const isCurrentPasswordValid = await user.comparePassword(currentPassword);
        
        if (!isCurrentPasswordValid) {
            return res.status(400).json({
                error: 'Current password is incorrect'
            });
        }
        
        user.password = newPassword;
        await user.save();
        
        logger.info(`Password changed for user: ${user.email}`);
        
        res.json({
            message: 'Password changed successfully'
        });
        
    } catch (error) {
        logger.error('Change password error:', error);
        res.status(500).json({
            error: 'Password change failed'
        });
    }
};

// Get current user profile
export const getProfile = async (req, res) => {
    try {
        const user = await User.findById(req.user._id).select('-password');
        
        if (!user) {
            return res.status(404).json({
                error: 'User not found'
            });
        }
        
        res.json({
            user
        });
        
    } catch (error) {
        logger.error('Get profile error:', error);
        res.status(500).json({
            error: 'Failed to fetch profile'
        });
    }
};
```

### 8. Routes

#### Auth Routes (routes/authRoutes.js)
```javascript
import express from 'express';
import passport from 'passport';
import {
    register,
    login,
    logout,
    refreshToken,
    googleCallback,
    verifyEmail,
    forgotPassword,
    resetPassword,
    changePassword,
    getProfile
} from '../controllers/authController.js';
import {
    validateRegistration,
    validateLogin,
    validatePasswordChange
} from '../middleware/validation.js';
import { authenticateJWT } from '../middleware/auth.js';
import { authLimiter } from '../middleware/rateLimit.js';

const router = express.Router();

// Apply rate limiting to auth routes
router.use(authLimiter);

// Registration and Login
router.post('/register', validateRegistration, register);
router.post('/login', validateLogin, login);
router.post('/logout', logout);

// Token refresh
router.post('/refresh-token', refreshToken);

// Google OAuth
router.get('/google',
    passport.authenticate('google', { scope: ['profile', 'email'] })
);

router.get('/google/callback',
    passport.authenticate('google', { failureRedirect: '/auth/login' }),
    googleCallback
);

// Email verification
router.get('/verify-email/:token', verifyEmail);

// Password reset
router.post('/forgot-password', forgotPassword);
router.post('/reset-password/:token', resetPassword);

// Protected routes (require authentication)
router.use(authenticateJWT); // Apply JWT auth to all routes below

router.post('/change-password', validatePasswordChange, changePassword);
router.get('/profile', getProfile);

export default router;
```

### 9. Frontend Implementation

#### Tailwind Configuration (client/tailwind.config.js)
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
    "./public/index.html"
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
        },
        secondary: {
          50: '#f8fafc',
          500: '#64748b',
          600: '#475569',
        }
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.3s ease-out',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        slideUp: {
          '0%': { transform: 'translateY(10px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        }
      }
    },
  },
  plugins: [],
}
```

#### React App Component (client/src/App.jsx)
```javascript
import React, { useEffect } from 'react';
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';
import { useDispatch, useSelector } from 'react-redux';

// Components
import Header from './components/Layout/Header';
import Footer from './components/Layout/Footer';
import Login from './components/Auth/Login';
import Register from './components/Auth/Register';
import Dashboard from './components/Dashboard/Dashboard';
import Home from './pages/Home';
import About from './pages/About';
import Contact from './pages/Contact';

// Redux
import { checkAuthStatus } from './store/authSlice';

// Styles
import './styles/globals.css';

// Protected Route Component
const ProtectedRoute = ({ children }) => {
  const { isAuthenticated, loading } = useSelector(state => state.auth);
  
  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center">
        <div className="animate-spin rounded-full h-32 w-32 border-b-2 border-primary-500"></div>
      </div>
    );
  }
  
  return isAuthenticated ? children : <Navigate to="/login" replace />;
};

// Public Route Component (redirect if authenticated)
const PublicRoute = ({ children }) => {
  const { isAuthenticated, loading } = useSelector(state => state.auth);
  
  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center">
        <div className="animate-spin rounded-full h-32 w-32 border-b-2 border-primary-500"></div>
      </div>
    );
  }
  
  return !isAuthenticated ? children : <Navigate to="/dashboard" replace />;
};

function App() {
  const dispatch = useDispatch();
  const { loading } = useSelector(state => state.auth);
  
  // Check authentication status on app load
  useEffect(() => {
    dispatch(checkAuthStatus());
  }, [dispatch]);
  
  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center bg-gray-50">
        <div className="text-center">
          <div className="animate-spin rounded-full h-32 w-32 border-b-2 border-primary-500 mx-auto"></div>
          <p className="mt-4 text-gray-600">Loading...</p>
        </div>
      </div>
    );
  }
  
  return (
    <Router>
      <div className="min-h-screen bg-gray-50 flex flex-col">
        <Header />
        
        <main className="flex-grow">
          <Routes>
            {/* Public Routes */}
            <Route path="/" element={<Home />} />
            <Route path="/about" element={<About />} />
            <Route path="/contact" element={<Contact />} />
            
            {/* Auth Routes */}
            <Route 
              path="/login" 
              element={
                <PublicRoute>
                  <Login />
                </PublicRoute>
              } 
            />
            <Route 
              path="/register" 
              element={
                <PublicRoute>
                  <Register />
                </PublicRoute>
              } 
            />
            
            {/* Protected Routes */}
            <Route 
              path="/dashboard" 
              element={
                <ProtectedRoute>
                  <Dashboard />
                </ProtectedRoute>
              } 
            />
            
            {/* 404 Route */}
            <Route path="*" element={
              <div className="min-h-screen flex items-center justify-center">
                <div className="text-center">
                  <h1 className="text-6xl font-bold text-gray-300">404</h1>
                  <p className="text-xl text-gray-600 mt-4">Page not found</p>
                  <a 
                    href="/" 
                    className="mt-6 inline-block bg-primary-500 text-white px-6 py-3 rounded-lg hover:bg-primary-600 transition-colors"
                  >
                    Go Home
                  </a>
                </div>
              </div>
            } />
          </Routes>
        </main>
        
        <Footer />
      </div>
    </Router>
  );