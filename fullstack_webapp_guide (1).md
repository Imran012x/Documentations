# Complete Full-Stack Web Development Project

A comprehensive full-stack web application using React, Node.js, Express.js, MongoDB, MySQL, and complete authentication system with MVC architecture.

## Complete Project File Structure

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
│   │   ├── 001_create_users_table.sql
│   │   ├── 002_create_sessions_table.sql
│   │   └── 003_create_refresh_tokens_table.sql
│   ├── seeders/
│   │   ├── users_seeder.sql
│   │   └── default_data.sql
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

## Technology Stack

### Frontend
- **React 18** with TypeScript support
- **Redux Toolkit** for state management
- **React Router** for navigation
- **Tailwind CSS** + **Bootstrap** + **Sass** for styling
- **Axios** for API calls

### Backend
- **Node.js** with **Express.js**
- **MongoDB** with **Mongoose**
- **MySQL** with **Sequelize**
- **Passport.js** for authentication strategies
- **JWT** for token-based auth
- **bcrypt** for password hashing
- **express-rate-limit** for rate limiting
- **helmet** for security headers
- **cors** for cross-origin requests
- **express-validator** for input validation

### Authentication & Security
- Local strategy with email/password
- Google OAuth 2.0
- JWT access & refresh tokens
- Session-based authentication
- Password hashing with bcrypt + salt
- Rate limiting
- Input sanitization
- CSRF protection

---

# Complete Code Implementation

## Frontend Implementation

---

## client/public/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta name="description" content="Full-stack web application with authentication" />
    <title>Full-Stack Web App</title>
    <!-- Bootstrap CSS for responsive design -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Google Fonts for typography -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
</head>
<body>
    <!-- Fallback for non-JavaScript users -->
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <!-- React app root element -->
    <div id="root"></div>
    <!-- Bootstrap JS for interactive components -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

---

## client/public/favicon.ico

```
# Favicon file (binary) - In production, this would be an actual .ico file
# This is a placeholder comment for the favicon
```

---

## client/src/components/Auth/Login.jsx

```jsx
import React, { useState } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { useNavigate } from 'react-router-dom';
import { loginUser } from '../../store/authSlice';
import Button from '../Common/Button';
import Input from '../Common/Input';
import GoogleAuth from './GoogleAuth';

const Login = () => {
    // ES6+ destructuring and useState hook for form state
    const [formData, setFormData] = useState({
        email: '',
        password: ''
    });
    
    // Redux hooks for state management
    const dispatch = useDispatch();
    const navigate = useNavigate();
    const { loading, error } = useSelector(state => state.auth);

    // ES6+ arrow function for handling input changes
    const handleChange = (e) => {
        const { name, value } = e.target; // ES6+ destructuring
        setFormData(prev => ({ ...prev, [name]: value })); // ES6+ spread operator
    };

    // Async function for form submission (ES6+ async/await)
    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            const result = await dispatch(loginUser(formData)).unwrap();
            if (result.success) {
                navigate('/dashboard');
            }
        } catch (error) {
            console.error('Login failed:', error);
        }
    };

    return (
        <div className="container mt-5">
            <div className="row justify-content-center">
                <div className="col-md-6">
                    <div className="card shadow">
                        <div className="card-body">
                            <h2 className="card-title text-center mb-4">Login</h2>
                            
                            {/* Conditional rendering for error display */}
                            {error && (
                                <div className="alert alert-danger" role="alert">
                                    {error}
                                </div>
                            )}

                            <form onSubmit={handleSubmit}>
                                <Input
                                    type="email"
                                    name="email"
                                    placeholder="Email"
                                    value={formData.email}
                                    onChange={handleChange}
                                    required
                                />
                                
                                <Input
                                    type="password"
                                    name="password"
                                    placeholder="Password"
                                    value={formData.password}
                                    onChange={handleChange}
                                    required
                                />

                                <Button
                                    type="submit"
                                    className="btn-primary w-100"
                                    disabled={loading}
                                >
                                    {loading ? 'Logging in...' : 'Login'}
                                </Button>
                            </form>

                            <hr />
                            <GoogleAuth />
                            
                            <div className="text-center mt-3">
                                <p>Don't have an account? <a href="/register">Register here</a></p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Login;
```

---

## client/src/components/Auth/Register.jsx

```jsx
import React, { useState } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { useNavigate } from 'react-router-dom';
import { registerUser } from '../../store/authSlice';
import Button from '../Common/Button';
import Input from '../Common/Input';

const Register = () => {
    // Form state with ES6+ object shorthand
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        password: '',
        confirmPassword: ''
    });
    
    const [errors, setErrors] = useState({});
    const dispatch = useDispatch();
    const navigate = useNavigate();
    const { loading, error } = useSelector(state => state.auth);

    // Form validation with ES6+ features
    const validateForm = () => {
        const newErrors = {};
        
        // Name validation
        if (!formData.name.trim()) {
            newErrors.name = 'Name is required';
        }
        
        // Email validation with ES6+ regex
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(formData.email)) {
            newErrors.email = 'Valid email is required';
        }
        
        // Password validation
        if (formData.password.length < 6) {
            newErrors.password = 'Password must be at least 6 characters';
        }
        
        // Confirm password validation
        if (formData.password !== formData.confirmPassword) {
            newErrors.confirmPassword = 'Passwords do not match';
        }
        
        return newErrors;
    };

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData(prev => ({ ...prev, [name]: value }));
        // Clear specific error when user starts typing
        if (errors[name]) {
            setErrors(prev => ({ ...prev, [name]: '' }));
        }
    };

    const handleSubmit = async (e) => {
        e.preventDefault();
        const validationErrors = validateForm();
        
        if (Object.keys(validationErrors).length > 0) {
            setErrors(validationErrors);
            return;
        }
        
        try {
            const result = await dispatch(registerUser(formData)).unwrap();
            if (result.success) {
                navigate('/login');
            }
        } catch (error) {
            console.error('Registration failed:', error);
        }
    };

    return (
        <div className="container mt-5">
            <div className="row justify-content-center">
                <div className="col-md-6">
                    <div className="card shadow">
                        <div className="card-body">
                            <h2 className="card-title text-center mb-4">Register</h2>
                            
                            {error && (
                                <div className="alert alert-danger">{error}</div>
                            )}

                            <form onSubmit={handleSubmit}>
                                <Input
                                    type="text"
                                    name="name"
                                    placeholder="Full Name"
                                    value={formData.name}
                                    onChange={handleChange}
                                    error={errors.name}
                                    required
                                />
                                
                                <Input
                                    type="email"
                                    name="email"
                                    placeholder="Email"
                                    value={formData.email}
                                    onChange={handleChange}
                                    error={errors.email}
                                    required
                                />
                                
                                <Input
                                    type="password"
                                    name="password"
                                    placeholder="Password"
                                    value={formData.password}
                                    onChange={handleChange}
                                    error={errors.password}
                                    required
                                />
                                
                                <Input
                                    type="password"
                                    name="confirmPassword"
                                    placeholder="Confirm Password"
                                    value={formData.confirmPassword}
                                    onChange={handleChange}
                                    error={errors.confirmPassword}
                                    required
                                />

                                <Button
                                    type="submit"
                                    className="btn-primary w-100"
                                    disabled={loading}
                                >
                                    {loading ? 'Registering...' : 'Register'}
                                </Button>
                            </form>
                            
                            <div className="text-center mt-3">
                                <p>Already have an account? <a href="/login">Login here</a></p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Register;
```

---

## client/src/components/Auth/GoogleAuth.jsx

```jsx
import React from 'react';
import { useDispatch } from 'react-redux';
import { googleLogin } from '../../store/authSlice';
import Button from '../Common/Button';

const GoogleAuth = () => {
    const dispatch = useDispatch();

    // Handle Google OAuth login
    const handleGoogleLogin = () => {
        // Redirect to backend Google OAuth route
        window.location.href = `${process.env.REACT_APP_API_URL}/auth/google`;
    };

    // Handle Google OAuth callback (called from backend)
    React.useEffect(() => {
        // Check for token in URL params (from OAuth callback)
        const urlParams = new URLSearchParams(window.location.search);
        const token = urlParams.get('token');
        const user = urlParams.get('user');
        
        if (token && user) {
            // Parse user data and dispatch login action
            const userData = JSON.parse(decodeURIComponent(user));
            dispatch(googleLogin({ token, user: userData }));
            // Clean up URL
            window.history.replaceState({}, document.title, window.location.pathname);
        }
    }, [dispatch]);

    return (
        <div className="text-center">
            <Button
                type="button"
                className="btn-outline-danger w-100"
                onClick={handleGoogleLogin}
            >
                <i className="fab fa-google me-2"></i>
                Continue with Google
            </Button>
        </div>
    );
};

export default GoogleAuth;
```

---

## client/src/components/Dashboard/Dashboard.jsx

```jsx
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchUserData } from '../../store/userSlice';
import { useAuth } from '../../hooks/useAuth';
import UserProfile from './UserProfile';

const Dashboard = () => {
    const dispatch = useDispatch();
    const { user } = useAuth();
    const { userData, loading } = useSelector(state => state.user);

    // Fetch user data on component mount
    useEffect(() => {
        if (user?.id) {
            dispatch(fetchUserData(user.id));
        }
    }, [dispatch, user?.id]);

    if (loading) {
        return (
            <div className="d-flex justify-content-center mt-5">
                <div className="spinner-border" role="status">
                    <span className="visually-hidden">Loading...</span>
                </div>
            </div>
        );
    }

    return (
        <div className="container mt-4">
            <div className="row">
                <div className="col-12">
                    <h1 className="mb-4">Welcome to Dashboard, {user?.name}!</h1>
                </div>
            </div>
            
            <div className="row">
                {/* Statistics Cards */}
                <div className="col-md-4 mb-3">
                    <div className="card bg-primary text-white">
                        <div className="card-body">
                            <h5 className="card-title">Total Users</h5>
                            <h2>{userData?.stats?.totalUsers || 0}</h2>
                        </div>
                    </div>
                </div>
                
                <div className="col-md-4 mb-3">
                    <div className="card bg-success text-white">
                        <div className="card-body">
                            <h5 className="card-title">Active Sessions</h5>
                            <h2>{userData?.stats?.activeSessions || 0}</h2>
                        </div>
                    </div>
                </div>
                
                <div className="col-md-4 mb-3">
                    <div className="card bg-info text-white">
                        <div className="card-body">
                            <h5 className="card-title">Login Count</h5>
                            <h2>{userData?.loginCount || 0}</h2>
                        </div>
                    </div>
                </div>
            </div>
            
            <div className="row mt-4">
                <div className="col-md-8">
                    <div className="card">
                        <div className="card-header">
                            <h5>Recent Activity</h5>
                        </div>
                        <div className="card-body">
                            <p>Last login: {user?.lastLogin ? new Date(user.lastLogin).toLocaleString() : 'Never'}</p>
                            <p>Account created: {user?.createdAt ? new Date(user.createdAt).toLocaleDateString() : 'Unknown'}</p>
                        </div>
                    </div>
                </div>
                
                <div className="col-md-4">
                    <UserProfile />
                </div>
            </div>
        </div>
    );
};

export default Dashboard;
```

---

## client/src/components/Dashboard/UserProfile.jsx

```jsx
import React, { useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { updateUserProfile } from '../../store/userSlice';
import Button from '../Common/Button';
import Input from '../Common/Input';
import Modal from '../Common/Modal';

const UserProfile = () => {
    const dispatch = useDispatch();
    const { user } = useSelector(state => state.auth);
    const { loading } = useSelector(state => state.user);
    
    // Local state for profile editing
    const [isEditing, setIsEditing] = useState(false);
    const [profileData, setProfileData] = useState({
        name: user?.name || '',
        email: user?.email || '',
        phone: user?.phone || '',
        bio: user?.bio || ''
    });

    const handleInputChange = (e) => {
        const { name, value } = e.target;
        setProfileData(prev => ({ ...prev, [name]: value }));
    };

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            await dispatch(updateUserProfile({ id: user.id, ...profileData })).unwrap();
            setIsEditing(false);
        } catch (error) {
            console.error('Profile update failed:', error);
        }
    };

    const handleCancel = () => {
        setProfileData({
            name: user?.name || '',
            email: user?.email || '',
            phone: user?.phone || '',
            bio: user?.bio || ''
        });
        setIsEditing(false);
    };

    return (
        <div className="card">
            <div className="card-header d-flex justify-content-between align-items-center">
                <h5>User Profile</h5>
                <Button
                    className="btn-sm btn-outline-primary"
                    onClick={() => setIsEditing(true)}
                    disabled={isEditing}
                >
                    Edit
                </Button>
            </div>
            <div className="card-body">
                {!isEditing ? (
                    // Display mode
                    <div>
                        <p><strong>Name:</strong> {user?.name}</p>
                        <p><strong>Email:</strong> {user?.email}</p>
                        <p><strong>Phone:</strong> {user?.phone || 'Not provided'}</p>
                        <p><strong>Bio:</strong> {user?.bio || 'No bio available'}</p>
                    </div>
                ) : (
                    // Edit mode
                    <form onSubmit={handleSubmit}>
                        <Input
                            type="text"
                            name="name"
                            label="Name"
                            value={profileData.name}
                            onChange={handleInputChange}
                            required
                        />
                        
                        <Input
                            type="email"
                            name="email"
                            label="Email"
                            value={profileData.email}
                            onChange={handleInputChange}
                            required
                        />
                        
                        <Input
                            type="tel"
                            name="phone"
                            label="Phone"
                            value={profileData.phone}
                            onChange={handleInputChange}
                        />
                        
                        <div className="mb-3">
                            <label className="form-label">Bio</label>
                            <textarea
                                className="form-control"
                                name="bio"
                                rows="3"
                                value={profileData.bio}
                                onChange={handleInputChange}
                                placeholder="Tell us about yourself..."
                            />
                        </div>
                        
                        <div className="d-flex gap-2">
                            <Button
                                type="submit"
                                className="btn-primary btn-sm"
                                disabled={loading}
                            >
                                {loading ? 'Saving...' : 'Save'}
                            </Button>
                            <Button
                                type="button"
                                className="btn-secondary btn-sm"
                                onClick={handleCancel}
                            >
                                Cancel
                            </Button>
                        </div>
                    </form>
                )}
            </div>
        </div>
    );
};

export default UserProfile;
```

---

## client/src/components/Layout/Header.jsx

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { useNavigate } from 'react-router-dom';
import { logout } from '../../store/authSlice';
import Button from '../Common/Button';

const Header = () => {
    const dispatch = useDispatch();
    const navigate = useNavigate();
    const { user, isAuthenticated } = useSelector(state => state.auth);

    const handleLogout = async () => {
        try {
            await dispatch(logout()).unwrap();
            navigate('/');
        } catch (error) {
            console.error('Logout failed:', error);
        }
    };

    return (
        <nav className="navbar navbar-expand-lg navbar-dark bg-dark">
            <div className="container">
                <a className="navbar-brand" href="/">
                    <strong>FullStack App</strong>
                </a>
                
                <button
                    className="navbar-toggler"
                    type="button"
                    data-bs-toggle="collapse"
                    data-bs-target="#navbarNav"
                >
                    <span className="navbar-toggler-icon"></span>
                </button>
                
                <div className="collapse navbar-collapse" id="navbarNav">
                    <ul className="navbar-nav me-auto">
                        <li className="nav-item">
                            <a className="nav-link" href="/">Home</a>
                        </li>
                        <li className="nav-item">
                            <a className="nav-link" href="/about">About</a>
                        </li>
                        <li className="nav-item">
                            <a className="nav-link" href="/contact">Contact</a>
                        </li>
                        {isAuthenticated && (
                            <li className="nav-item">
                                <a className="nav-link" href="/dashboard">Dashboard</a>
                            </li>
                        )}
                    </ul>
                    
                    <div className="d-flex align-items-center">
                        {isAuthenticated ? (
                            // User is logged in
                            <div className="d-flex align-items-center">
                                <span className="navbar-text me-3">
                                    Welcome, {user?.name}!
                                </span>
                                <Button
                                    className="btn-outline-light btn-sm"
                                    onClick={handleLogout}
                                >
                                    Logout
                                </Button>
                            </div>
                        ) : (
                            // User is not logged in
                            <div className="d-flex gap-2">
                                <a href="/login" className="btn btn-outline-light btn-sm">
                                    Login
                                </a>
                                <a href="/register" className="btn btn-primary btn-sm">
                                    Register
                                </a>
                            </div>
                        )}
                    </div>
                </div>
            </div>
        </nav>
    );
};

export default Header;
```

---

## client/src/components/Layout/Footer.jsx

```jsx
import React from 'react';

const Footer = () => {
    const currentYear = new Date().getFullYear(); // ES6+ date handling

    return (
        <footer className="bg-dark text-light mt-5 py-4">
            <div className="container">
                <div className="row">
                    <div className="col-md-6">
                        <h5>FullStack Web App</h5>
                        <p className="mb-0">
                            A complete full-stack application with authentication,
                            built with modern web technologies.
                        </p>
                    </div>
                    
                    <div className="col-md-3">
                        <h6>Quick Links</h6>
                        <ul className="list-unstyled">
                            <li><a href="/" className="text-light text-decoration-none">Home</a></li>
                            <li><a href="/about" className="text-light text-decoration-none">About</a></li>
                            <li><a href="/contact" className="text-light text-decoration-none">Contact</a></li>
                            <li><a href="/dashboard" className="text-light text-decoration-none">Dashboard</a></li>
                        </ul>
                    </div>
                    
                    <div className="col-md-3">
                        <h6>Technologies</h6>
                        <ul className="list-unstyled small">
                            <li>React & Redux</li>
                            <li>Node.js & Express</li>
                            <li>MongoDB & MySQL</li>
                            <li>JWT & OAuth</li>
                        </ul>
                    </div>
                </div>
                
                <hr className="my-3" />
                
                <div className="row align-items-center">
                    <div className="col-md-6">
                        <p className="mb-0">
                            &copy; {currentYear} FullStack Web App. All rights reserved.
                        </p>
                    </div>
                    <div className="col-md-6 text-md-end">
                        <small className="text-muted">
                            Built with React, Node.js, and modern web technologies
                        </small>
                    </div>
                </div>
            </div>
        </footer>
    );
};

export default Footer;
```

---

## client/src/components/Layout/Sidebar.jsx

```jsx
import React, { useState } from 'react';
import { useSelector } from 'react-redux';
import { useLocation } from 'react-router-dom';

const Sidebar = () => {
    const [isCollapsed, setIsCollapsed] = useState(false);
    const { user, isAuthenticated } = useSelector(state => state.auth);
    const location = useLocation();

    // Navigation items with ES6+ array methods
    const navItems = [
        { path: '/dashboard', label: 'Dashboard', icon: 'fas fa-tachometer-alt' },
        { path: '/profile', label: 'Profile', icon: 'fas fa-user' },
        { path: '/settings', label: 'Settings', icon: 'fas fa-cog' },
        { path: '/users', label: 'Users', icon: 'fas fa-users', adminOnly: true },
        { path: '/reports', label: 'Reports', icon: 'fas fa-chart-bar', adminOnly: true }
    ];

    // Filter items based on user role
    const filteredItems = navItems.filter(item => 
        !item.adminOnly || (user?.role === 'admin')
    );

    const isActive = (path) => location.pathname === path;

    if (!isAuthenticated) {
        return null; // Don't show sidebar if not authenticated
    }

    return (
        <div className={`sidebar bg-dark text-light ${isCollapsed ? 'collapsed' : ''}`}>
            <div className="sidebar-header p-3 d-flex justify-content-between align-items-center">
                <h6 className={`mb-0 ${isCollapsed ? 'd-none' : ''}`}>Navigation</h6>
                <button
                    className="btn btn-sm btn-outline-light"
                    onClick={() => setIsCollapsed(!isCollapsed)}
                >
                    <i className={`fas ${isCollapsed ? 'fa-chevron-right' : 'fa-chevron-left'}`}></i>
                </button>
            </div>
            
            <nav className="sidebar-nav">
                <ul className="list-unstyled">
                    {filteredItems.map((item, index) => (
                        <li key={index} className="nav-item">
                            <a
                                href={item.path}
                                className={`nav-link d-flex align-items-center p-3 text-decoration-none ${
                                    isActive(item.path) ? 'active bg-primary' : 'text-light'
                                }`}
                            >
                                <i className={`${item.icon} me-3`}></i>
                                <span className={isCollapsed ? 'd-none' : ''}>{item.label}</span>
                            </a>
                        </li>
                    ))}
                </ul>
            </nav>
            
            {/* User info at bottom */}
            <div className="sidebar-footer mt-auto p-3 border-top">
                <div className="d-flex align-items-center">
                    <div className="avatar me-2">
                        <img
                            src={user?.avatar || '/default-avatar.png'}
                            alt="User Avatar"
                            className="rounded-circle"
                            width="32"
                            height="32"
                        />
                    </div>
                    <div className={isCollapsed ? 'd-none' : ''}>
                        <small className="d-block text-truncate">{user?.name}</small>
                        <small className="text-muted">{user?.role}</small>
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Sidebar;

---

## client/src/components/Common/Button.jsx

```jsx
import React from 'react';
import PropTypes from 'prop-types';

const Button = ({ 
    children, 
    type = 'button', 
    className = '', 
    onClick, 
    disabled = false,
    loading = false,
    variant = 'primary',
    size = 'md',
    ...props 
}) => {
    // ES6+ template literals for dynamic class names
    const baseClasses = 'btn';
    const variantClass = `btn-${variant}`;
    const sizeClass = size !== 'md' ? `btn-${size}` : '';
    
    // Combine classes using ES6+ template literals
    const combinedClasses = `${baseClasses} ${variantClass} ${sizeClass} ${className}`.trim();

    return (
        <button
            type={type}
            className={combinedClasses}
            onClick={onClick}
            disabled={disabled || loading}
            {...props}
        >
            {loading && (
                <span className="spinner-border spinner-border-sm me-2" role="status">
                    <span className="visually-hidden">Loading...</span>
                </span>
            )}
            {children}
        </button>
    );
};

// PropTypes for type checking (legacy: React.PropTypes)
Button.propTypes = {
    children: PropTypes.node.isRequired,
    type: PropTypes.oneOf(['button', 'submit', 'reset']),
    className: PropTypes.string,
    onClick: PropTypes.func,
    disabled: PropTypes.bool,
    loading: PropTypes.bool,
    variant: PropTypes.oneOf(['primary', 'secondary', 'success', 'danger', 'warning', 'info', 'light', 'dark', 'outline-primary', 'outline-secondary']),
    size: PropTypes.oneOf(['sm', 'md', 'lg'])
};

export default Button;
```

---

## client/src/components/Common/Input.jsx

```jsx
import React from 'react';
import PropTypes from 'prop-types';

const Input = ({
    type = 'text',
    name,
    label,
    placeholder,
    value,
    onChange,
    onBlur,
    error,
    required = false,
    disabled = false,
    className = '',
    ...props
}) => {
    const inputId = `input-${name}`; // Unique ID for accessibility

    return (
        <div className={`mb-3 ${className}`}>
            {label && (
                <label htmlFor={inputId} className="form-label">
                    {label}
                    {required && <span className="text-danger ms-1">*</span>}
                </label>
            )}
            
            <input
                type={type}
                id={inputId}
                name={name}
                className={`form-control ${error ? 'is-invalid' : ''}`}
                placeholder={placeholder}
                value={value}
                onChange={onChange}
                onBlur={onBlur}
                required={required}
                disabled={disabled}
                {...props}
            />
            
            {error && (
                <div className="invalid-feedback">
                    {error}
                </div>
            )}
        </div>
    );
};

Input.propTypes = {
    type: PropTypes.string,
    name: PropTypes.string.isRequired,
    label: PropTypes.string,
    placeholder: PropTypes.string,
    value: PropTypes.oneOfType([PropTypes.string, PropTypes.number]),
    onChange: PropTypes.func.isRequired,
    onBlur: PropTypes.func,
    error: PropTypes.string,
    required: PropTypes.bool,
    disabled: PropTypes.bool,
    className: PropTypes.string
};

export default Input;
```

---

## client/src/components/Common/Modal.jsx

```jsx
import React, { useEffect } from 'react';
import PropTypes from 'prop-types';

const Modal = ({
    isOpen,
    onClose,
    title,
    children,
    size = 'md',
    showCloseButton = true,
    backdrop = true,
    keyboard = true
}) => {
    // ES6+ useEffect for handling escape key
    useEffect(() => {
        const handleEscape = (event) => {
            if (keyboard && event.key === 'Escape' && isOpen) {
                onClose();
            }
        };

        if (isOpen) {
            document.addEventListener('keydown', handleEscape);
            document.body.style.overflow = 'hidden'; // Prevent background scroll
        }

        // Cleanup function (ES6+ arrow function)
        return () => {
            document.removeEventListener('keydown', handleEscape);
            document.body.style.overflow = 'unset';
        };
    }, [isOpen, onClose, keyboard]);

    const handleBackdropClick = (event) => {
        if (backdrop && event.target === event.currentTarget) {
            onClose();
        }
    };

    if (!isOpen) return null;

    return (
        <div
            className="modal fade show d-block"
            tabIndex="-1"
            style={{ backgroundColor: 'rgba(0,0,0,0.5)' }}
            onClick={handleBackdropClick}
        >
            <div className={`modal-dialog modal-${size}`}>
                <div className="modal-content">
                    <div className="modal-header">
                        <h5 className="modal-title">{title}</h5>
                        {showCloseButton && (
                            <button
                                type="button"
                                className="btn-close"
                                onClick={onClose}
                                aria-label="Close"
                            />
                        )}
                    </div>
                    <div className="modal-body">
                        {children}
                    </div>
                </div>
            </div>
        </div>
    );
};

Modal.propTypes = {
    isOpen: PropTypes.bool.isRequired,
    onClose: PropTypes.func.isRequired,
    title: PropTypes.string.isRequired,
    children: PropTypes.node.isRequired,
    size: PropTypes.oneOf(['sm', 'md', 'lg', 'xl']),
    showCloseButton: PropTypes.bool,
    backdrop: PropTypes.bool,
    keyboard: PropTypes.bool
};

export default Modal;
```

---

## client/src/pages/Home.jsx

```jsx
import React from 'react';
import { useSelector } from 'react-redux';
import { Link } from 'react-router-dom';
import Button from '../components/Common/Button';

const Home = () => {
    const { isAuthenticated, user } = useSelector(state => state.auth);

    return (
        <div className="container mt-5">
            {/* Hero Section */}
            <div className="row align-items-center min-vh-50">
                <div className="col-lg-6">
                    <h1 className="display-4 fw-bold mb-4">
                        Welcome to FullStack Web App
                    </h1>
                    <p className="lead mb-4">
                        A complete full-stack application featuring modern authentication,
                        responsive design, and robust backend architecture built with
                        React, Node.js, and MongoDB/MySQL.
                    </p>
                    
                    {!isAuthenticated ? (
                        <div className="d-flex gap-3">
                            <Link to="/register">
                                <Button variant="primary" size="lg">
                                    Get Started
                                </Button>
                            </Link>
                            <Link to="/login">
                                <Button variant="outline-primary" size="lg">
                                    Login
                                </Button>
                            </Link>
                        </div>
                    ) : (
                        <div>
                            <p className="text-success mb-3">
                                Welcome back, {user?.name}!
                            </p>
                            <Link to="/dashboard">
                                <Button variant="primary" size="lg">
                                    Go to Dashboard
                                </Button>
                            </Link>
                        </div>
                    )}
                </div>
                
                <div className="col-lg-6">
                    <div className="text-center">
                        <img
                            src="/api/placeholder/500/400"
                            alt="FullStack Application"
                            className="img-fluid rounded shadow"
                        />
                    </div>
                </div>
            </div>
            
            {/* Features Section */}
            <div className="row mt-5 pt-5">
                <div className="col-12 text-center mb-5">
                    <h2 className="display-5 fw-bold">Features</h2>
                    <p className="lead">Everything you need for a modern web application</p>
                </div>
            </div>
            
            <div className="row g-4">
                {/* Feature cards using ES6+ array methods */}
                {[
                    {
                        icon: 'fas fa-shield-alt',
                        title: 'Secure Authentication',
                        description: 'JWT tokens, OAuth, password hashing, and session management'
                    },
                    {
                        icon: 'fas fa-mobile-alt',
                        title: 'Responsive Design',
                        description: 'Mobile-first design with Bootstrap and Tailwind CSS'
                    },
                    {
                        icon: 'fas fa-database',
                        title: 'Dual Database Support',
                        description: 'MongoDB for NoSQL and MySQL for relational data'
                    },
                    {
                        icon: 'fas fa-rocket',
                        title: 'Modern Stack',
                        description: 'React, Node.js, Express, Redux, and TypeScript'
                    },
                    {
                        icon: 'fas fa-cogs',
                        title: 'MVC Architecture',
                        description: 'Clean separation of concerns with MVC pattern'
                    },
                    {
                        icon: 'fas fa-lock',
                        title: 'Security First',
                        description: 'Rate limiting, input sanitization, and CSRF protection'
                    }
                ].map((feature, index) => (
                    <div key={index} className="col-md-4">
                        <div className="card h-100 shadow-sm">
                            <div className="card-body text-center">
                                <i className={`${feature.icon} fa-3x text-primary mb-3`}></i>
                                <h5 className="card-title">{feature.title}</h5>
                                <p className="card-text">{feature.description}</p>
                            </div>
                        </div>
                    </div>
                ))}
            </div>
            
            {/* Call to Action */}
            <div className="row mt-5 pt-5">
                <div className="col-12 text-center">
                    <div className="bg-primary text-white rounded p-5">
                        <h3 className="mb-3">Ready to get started?</h3>
                        <p className="mb-4">Join thousands of developers building amazing applications</p>
                        {!isAuthenticated && (
                            <Link to="/register">
                                <Button variant="light" size="lg">
                                    Create Account
                                </Button>
                            </Link>
                        )}
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Home;
```

---

## client/src/pages/About.jsx

```jsx
import React from 'react';

const About = () => {
    return (
        <div className="container mt-5">
            <div className="row">
                <div className="col-lg-8 mx-auto">
                    <h1 className="display-4 text-center mb-5">About Our Project</h1>
                    
                    <div className="card shadow-lg">
                        <div className="card-body p-5">
                            <h2 className="mb-4">Full-Stack Web Development</h2>
                            <p className="lead">
                                This project demonstrates a complete full-stack web application
                                using modern technologies and best practices in web development.
                            </p>
                            
                            <h3 className="mt-4 mb-3">Technologies Used</h3>
                            
                            <div className="row">
                                <div className="col-md-6">
                                    <h5>Frontend</h5>
                                    <ul className="list-unstyled">
                                        <li><i className="fas fa-check text-success me-2"></i>React 18</li>
                                        <li><i className="fas fa-check text-success me-2"></i>Redux Toolkit</li>
                                        <li><i className="fas fa-check text-success me-2"></i>TypeScript</li>
                                        <li><i className="fas fa-check text-success me-2"></i>Tailwind CSS</li>
                                        <li><i className="fas fa-check text-success me-2"></i>Bootstrap 5</li>
                                        <li><i className="fas fa-check text-success me-2"></i>Sass/SCSS</li>
                                    </ul>
                                </div>
                                
                                <div className="col-md-6">
                                    <h5>Backend</h5>
                                    <ul className="list-unstyled">
                                        <li><i className="fas fa-check text-success me-2"></i>Node.js</li>
                                        <li><i className="fas fa-check text-success me-2"></i>Express.js</li>
                                        <li><i className="fas fa-check text-success me-2"></i>MongoDB</li>
                                        <li><i className="fas fa-check text-success me-2"></i>MySQL</li>
                                        <li><i className="fas fa-check text-success me-2"></i>JWT Authentication</li>
                                        <li><i className="fas fa-check text-success me-2"></i>Passport.js</li>
                                    </ul>
                                </div>
                            </div>
                            
                            <h3 className="mt-4 mb-3">Key Features</h3>
                            <div className="row">
                                <div className="col-md-6">
                                    <ul className="list-unstyled">
                                        <li><i className="fas fa-star text-warning me-2"></i>User Authentication</li>
                                        <li><i className="fas fa-star text-warning me-2"></i>Google OAuth Integration</li>
                                        <li><i className="fas fa-star text-warning me-2"></i>Password Hashing & Salting</li>
                                        <li><i className="fas fa-star text-warning me-2"></i>JWT & Session Management</li>
                                    </ul>
                                </div>
                                <div className="col-md-6">
                                    <ul className="list-unstyled">
                                        <li><i className="fas fa-star text-warning me-2"></i>Rate Limiting</li>
                                        <li><i className="fas fa-star text-warning me-2"></i>Input Sanitization</li>
                                        <li><i className="fas fa-star text-warning me-2"></i>MVC Architecture</li>
                                        <li><i className="fas fa-star text-warning me-2"></i>Responsive Design</li>
                                    </ul>
                                </div>
                            </div>
                            
                            <h3 className="mt-4 mb-3">Architecture</h3>
                            <p>
                                The application follows the Model-View-Controller (MVC) pattern
                                for clean separation of concerns. The frontend is built with React
                                and Redux for state management, while the backend uses Express.js
                                with both MongoDB and MySQL database support.
                            </p>
                            
                            <div className="alert alert-info mt-4">
                                <h5 className="alert-heading">Security First</h5>
                                <p className="mb-0">
                                    Security is a top priority with features like password hashing,
                                    JWT tokens, rate limiting, input validation, and sanitization
                                    to protect against common web vulnerabilities.
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    );
};

export default About;
```

---

## client/src/pages/Contact.jsx

```jsx
import React, { useState } from 'react';
import Button from '../components/Common/Button';
import Input from '../components/Common/Input';

const Contact = () => {
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        subject: '',
        message: ''
    });
    
    const [isSubmitting, setIsSubmitting] = useState(false);
    const [submitted, setSubmitted] = useState(false);

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData(prev => ({ ...prev, [name]: value }));
    };

    const handleSubmit = async (e) => {
        e.preventDefault();
        setIsSubmitting(true);
        
        try {
            // Simulate API call
            await new Promise(resolve => setTimeout(resolve, 1000));
            
            // In real app, make API call here
            console.log('Contact form submitted:', formData);
            
            setSubmitted(true);
            setFormData({ name: '', email: '', subject: '', message: '' });
        } catch (error) {
            console.error('Error submitting form:', error);
        } finally {
            setIsSubmitting(false);
        }
    };

    return (
        <div className="container mt-5">
            <div className="row">
                <div className="col-lg-8 mx-auto">
                    <h1 className="display-4 text-center mb-5">Contact Us</h1>
                    
                    {submitted && (
                        <div className="alert alert-success alert-dismissible fade show" role="alert">
                            Thank you for your message! We'll get back to you soon.
                            <button
                                type="button"
                                className="btn-close"
                                onClick={() => setSubmitted(false)}
                            ></button>
                        </div>
                    )}
                    
                    <div className="row">
                        <div className="col-md-8">
                            <div className="card shadow">
                                <div className="card-body p-4">
                                    <form onSubmit={handleSubmit}>
                                        <div className="row">
                                            <div className="col-md-6">
                                                <Input
                                                    type="text"
                                                    name="name"
                                                    label="Full Name"
                                                    value={formData.name}
                                                    onChange={handleChange}
                                                    required
                                                />
                                            </div>
                                            <div className="col-md-6">
                                                <Input
                                                    type="email"
                                                    name="email"
                                                    label="Email Address"
                                                    value={formData.email}
                                                    onChange={handleChange}
                                                    required
                                                />
                                            </div>
                                        </div>
                                        
                                        <Input
                                            type="text"
                                            name="subject"
                                            label="Subject"
                                            value={formData.subject}
                                            onChange={handleChange}
                                            required
                                        />
                                        
                                        <div className="mb-3">
                                            <label htmlFor="message" className="form-label">
                                                Message <span className="text-danger">*</span>
                                            </label>
                                            <textarea
                                                id="message"
                                                name="message"
                                                className="form-control"
                                                rows="5"
                                                value={formData.message}
                                                onChange={handleChange}
                                                required
                                                placeholder="Your message here..."
                                            />
                                        </div>
                                        
                                        <Button
                                            type="submit"
                                            variant="primary"
                                            size="lg"
                                            loading={isSubmitting}
                                            className="w-100"
                                        >
                                            Send Message
                                        </Button>
                                    </form>
                                </div>
                            </div>
                        </div>
                        
                        <div className="col-md-4">
                            <div className="card shadow">
                                <div className="card-body">
                                    <h5 className="card-title">Get in Touch</h5>
                                    <p className="card-text">
                                        Have questions about our full-stack application?
                                        We'd love to hear from you.
                                    </p>
                                    
                                    <div className="mb-3">
                                        <i className="fas fa-envelope text-primary me-2"></i>
                                        <strong>Email:</strong><br />
                                        <a href="mailto:info@fullstackapp.com">info@fullstackapp.com</a>
                                    </div>
                                    
                                    <div className="mb-3">
                                        <i className="fas fa-phone text-primary me-2"></i>
                                        <strong>Phone:</strong><br />
                                        <a href="tel:+1234567890">+1 (234) 567-8900</a>
                                    </div>
                                    
                                    <div className="mb-3">
                                        <i className="fas fa-map-marker-alt text-primary me-2"></i>
                                        <strong>Address:</strong><br />
                                        123 Developer Street<br />
                                        Tech City, TC 12345
                                    </div>
                                    
                                    <div className="mt-4">
                                        <h6>Follow Us</h6>
                                        <div className="d-flex gap-2">
                                            <a href="#" className="btn btn-outline-primary btn-sm">
                                                <i className="fab fa-twitter"></i>
                                            </a>
                                            <a href="#" className="btn btn-outline-primary btn-sm">
                                                <i className="fab fa-github"></i>
                                            </a>
                                            <a href="#" className="btn btn-outline-primary btn-sm">
                                                <i className="fab fa-linkedin"></i>
                                            </a>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Contact;


# Complete Full-Stack Web Application

## client/src/pages/Contact.jsx

```jsx
import React, { useState } from 'react';
import Button from '../components/Common/Button';
import Input from '../components/Common/Input';

const Contact = () => {
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        subject: '',
        message: ''
    });
    const [loading, setLoading] = useState(false);
    const [success, setSuccess] = useState(false);

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData(prev => ({ ...prev, [name]: value }));
    };

    const handleSubmit = async (e) => {
        e.preventDefault();
        setLoading(true);
        
        try {
            // Simulate API call
            await new Promise(resolve => setTimeout(resolve, 1000));
            setSuccess(true);
            setFormData({ name: '', email: '', subject: '', message: '' });
        } catch (error) {
            console.error('Contact form submission failed:', error);
        } finally {
            setLoading(false);
        }
    };

    if (success) {
        return (
            <div className="container mt-5">
                <div className="alert alert-success text-center">
                    <h4>Thank you for your message!</h4>
                    <p>We'll get back to you soon.</p>
                    <Button onClick={() => setSuccess(false)}>Send Another Message</Button>
                </div>
            </div>
        );
    }

    return (
        <div className="container mt-5">
            <div className="row">
                <div className="col-md-8 mx-auto">
                    <h2 className="text-center mb-4">Contact Us</h2>
                    <form onSubmit={handleSubmit}>
                        <div className="row">
                            <div className="col-md-6">
                                <Input
                                    name="name"
                                    label="Name"
                                    value={formData.name}
                                    onChange={handleChange}
                                    required
                                />
                            </div>
                            <div className="col-md-6">
                                <Input
                                    type="email"
                                    name="email"
                                    label="Email"
                                    value={formData.email}
                                    onChange={handleChange}
                                    required
                                />
                            </div>
                        </div>
                        
                        <Input
                            name="subject"
                            label="Subject"
                            value={formData.subject}
                            onChange={handleChange}
                            required
                        />
                        
                        <div className="mb-3">
                            <label className="form-label">Message *</label>
                            <textarea
                                className="form-control"
                                name="message"
                                rows="5"
                                value={formData.message}
                                onChange={handleChange}
                                required
                            />
                        </div>
                        
                        <Button
                            type="submit"
                            className="w-100"
                            loading={loading}
                        >
                            Send Message
                        </Button>
                    </form>
                </div>
            </div>
        </div>
    );
};

export default Contact;
```

---

## client/src/hooks/useAuth.js

```javascript
import { useSelector, useDispatch } from 'react-redux';
import { useCallback } from 'react';
import { logout, refreshToken } from '../store/authSlice';

// Custom hook for authentication logic
export const useAuth = () => {
    const dispatch = useDispatch();
    const authState = useSelector(state => state.auth);
    
    // ES6+ destructuring with default values
    const {
        user = null,
        token = null,
        isAuthenticated = false,
        loading = false,
        error = null
    } = authState;

    // Memoized logout function using useCallback
    const handleLogout = useCallback(async () => {
        try {
            await dispatch(logout()).unwrap();
        } catch (error) {
            console.error('Logout failed:', error);
        }
    }, [dispatch]);

    // Memoized token refresh function
    const handleRefreshToken = useCallback(async () => {
        try {
            const result = await dispatch(refreshToken()).unwrap();
            return result.token;
        } catch (error) {
            console.error('Token refresh failed:', error);
            throw error;
        }
    }, [dispatch]);

    // Check if user has specific role
    const hasRole = useCallback((role) => {
        return user?.role === role;
    }, [user?.role]);

    // Check if user has any of the specified roles
    const hasAnyRole = useCallback((roles) => {
        return roles.includes(user?.role);
    }, [user?.role]);

    return {
        user,
        token,
        isAuthenticated,
        loading,
        error,
        logout: handleLogout,
        refreshToken: handleRefreshToken,
        hasRole,
        hasAnyRole
    };
};
```

---

## client/src/hooks/useApi.js

```javascript
import { useState, useCallback } from 'react';
import { useAuth } from './useAuth';
import api from '../services/api';

// Custom hook for API calls with authentication
export const useApi = () => {
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState(null);
    const { token, refreshToken } = useAuth();

    // Generic API call function with error handling
    const apiCall = useCallback(async (apiFunction, ...args) => {
        setLoading(true);
        setError(null);

        try {
            const result = await apiFunction(...args);
            return result;
        } catch (err) {
            // Handle token expiration
            if (err.response?.status === 401 && token) {
                try {
                    await refreshToken();
                    // Retry the original request
                    const retryResult = await apiFunction(...args);
                    return retryResult;
                } catch (refreshError) {
                    setError('Session expired. Please login again.');
                    throw refreshError;
                }
            }
            
            const errorMessage = err.response?.data?.message || err.message || 'An error occurred';
            setError(errorMessage);
            throw err;
        } finally {
            setLoading(false);
        }
    }, [token, refreshToken]);

    // Specific API methods
    const get = useCallback((url, config = {}) => {
        return apiCall(api.get, url, config);
    }, [apiCall]);

    const post = useCallback((url, data, config = {}) => {
        return apiCall(api.post, url, data, config);
    }, [apiCall]);

    const put = useCallback((url, data, config = {}) => {
        return apiCall(api.put, url, data, config);
    }, [apiCall]);

    const del = useCallback((url, config = {}) => {
        return apiCall(api.delete, url, config);
    }, [apiCall]);

    return {
        loading,
        error,
        get,
        post,
        put,
        delete: del,
        clearError: () => setError(null)
    };
};
```

---

## client/src/services/api.js

```javascript
import axios from 'axios';

// Create axios instance with base configuration
const api = axios.create({
    baseURL: process.env.REACT_APP_API_URL || 'http://localhost:5000/api',
    timeout: 10000,
    headers: {
        'Content-Type': 'application/json'
    }
});

// Request interceptor to add auth token
api.interceptors.request.use(
    (config) => {
        // Get token from localStorage (ES6+ template literals)
        const token = localStorage.getItem('authToken');
        if (token) {
            config.headers.Authorization = `Bearer ${token}`;
        }
        return config;
    },
    (error) => {
        return Promise.reject(error);
    }
);

// Response interceptor for global error handling
api.interceptors.response.use(
    (response) => response,
    (error) => {
        // Handle common errors
        if (error.response?.status === 401) {
            // Token expired or invalid
            localStorage.removeItem('authToken');
            localStorage.removeItem('user');
            window.location.href = '/login';
        }
        
        return Promise.reject(error);
    }
);

export default api;
```

---

## client/src/services/auth.js

```javascript
import api from './api';

// Authentication service with ES6+ class syntax
class AuthService {
    // Login user
    async login(credentials) {
        const response = await api.post('/auth/login', credentials);
        const { token, user } = response.data;
        
        // Store in localStorage
        localStorage.setItem('authToken', token);
        localStorage.setItem('user', JSON.stringify(user));
        
        return response.data;
    }

    // Register user
    async register(userData) {
        const response = await api.post('/auth/register', userData);
        return response.data;
    }

    // Logout user
    async logout() {
        try {
            await api.post('/auth/logout');
        } finally {
            // Clear local storage regardless of API response
            localStorage.removeItem('authToken');
            localStorage.removeItem('user');
        }
    }

    // Refresh token
    async refreshToken() {
        const response = await api.post('/auth/refresh');
        const { token } = response.data;
        
        localStorage.setItem('authToken', token);
        return response.data;
    }

    // Get current user from localStorage
    getCurrentUser() {
        const user = localStorage.getItem('user');
        return user ? JSON.parse(user) : null;
    }

    // Check if user is authenticated
    isAuthenticated() {
        return !!localStorage.getItem('authToken');
    }

    // Google OAuth login
    googleLogin() {
        window.location.href = `${process.env.REACT_APP_API_URL}/auth/google`;
    }
}

// Export singleton instance
export default new AuthService();
```

---

## client/src/store/index.js

```javascript
import { configureStore } from '@reduxjs/toolkit';
import authSlice from './authSlice';
import userSlice from './userSlice';

// Configure Redux store with Redux Toolkit
const store = configureStore({
    reducer: {
        auth: authSlice,
        user: userSlice
    },
    middleware: (getDefaultMiddleware) =>
        getDefaultMiddleware({
            serializableCheck: {
                // Ignore these action types for serialization checks
                ignoredActions: ['persist/PERSIST', 'persist/REHYDRATE']
            }
        }),
    devTools: process.env.NODE_ENV !== 'production'
});

export default store;
```

---

## client/src/store/authSlice.js

```javascript
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
import authService from '../services/auth';

// Async thunks for authentication actions
export const loginUser = createAsyncThunk(
    'auth/login',
    async (credentials, { rejectWithValue }) => {
        try {
            const response = await authService.login(credentials);
            return response;
        } catch (error) {
            return rejectWithValue(error.response?.data?.message || 'Login failed');
        }
    }
);

export const registerUser = createAsyncThunk(
    'auth/register',
    async (userData, { rejectWithValue }) => {
        try {
            const response = await authService.register(userData);
            return response;
        } catch (error) {
            return rejectWithValue(error.response?.data?.message || 'Registration failed');
        }
    }
);

export const logout = createAsyncThunk(
    'auth/logout',
    async (_, { rejectWithValue }) => {
        try {
            await authService.logout();
            return {};
        } catch (error) {
            return rejectWithValue(error.response?.data?.message || 'Logout failed');
        }
    }
);

export const refreshToken = createAsyncThunk(
    'auth/refreshToken',
    async (_, { rejectWithValue }) => {
        try {
            const response = await authService.refreshToken();
            return response;
        } catch (error) {
            return rejectWithValue(error.response?.data?.message || 'Token refresh failed');
        }
    }
);

// Initial state
const initialState = {
    user: authService.getCurrentUser(),
    token: localStorage.getItem('authToken'),
    isAuthenticated: authService.isAuthenticated(),
    loading: false,
    error: null
};

// Auth slice
const authSlice = createSlice({
    name: 'auth',
    initialState,
    reducers: {
        clearError: (state) => {
            state.error = null;
        },
        googleLogin: (state, action) => {
            const { token, user } = action.payload;
            state.user = user;
            state.token = token;
            state.isAuthenticated = true;
            state.loading = false;
            state.error = null;
        }
    },
    extraReducers: (builder) => {
        builder
            // Login cases
            .addCase(loginUser.pending, (state) => {
                state.loading = true;
                state.error = null;
            })
            .addCase(loginUser.fulfilled, (state, action) => {
                state.loading = false;
                state.user = action.payload.user;
                state.token = action.payload.token;
                state.isAuthenticated = true;
                state.error = null;
            })
            .addCase(loginUser.rejected, (state, action) => {
                state.loading = false;
                state.error = action.payload;
                state.isAuthenticated = false;
            })
            // Register cases
            .addCase(registerUser.pending, (state) => {
                state.loading = true;
                state.error = null;
            })
            .addCase(registerUser.fulfilled, (state) => {
                state.loading = false;
                state.error = null;
            })
            .addCase(registerUser.rejected, (state, action) => {
                state.loading = false;
                state.error = action.payload;
            })
            // Logout cases
            .addCase(logout.fulfilled, (state) => {
                state.user = null;
                state.token = null;
                state.isAuthenticated = false;
                state.loading = false;
                state.error = null;
            })
            // Refresh token cases
            .addCase(refreshToken.fulfilled, (state, action) => {
                state.token = action.payload.token;
                state.loading = false;
            });
    }
});

export const { clearError, googleLogin } = authSlice.actions;
export default authSlice.reducer;
```

---

## client/src/store/userSlice.js

```javascript
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
import api from '../services/api';

// Async thunks for user actions
export const fetchUserData = createAsyncThunk(
    'user/fetchUserData',
    async (userId, { rejectWithValue }) => {
        try {
            const response = await api.get(`/users/${userId}`);
            return response.data;
        } catch (error) {
            return rejectWithValue(error.response?.data?.message || 'Failed to fetch user data');
        }
    }
);

export const updateUserProfile = createAsyncThunk(
    'user/updateProfile',
    async (userData, { rejectWithValue }) => {
        try {
            const response = await api.put(`/users/${userData.id}`, userData);
            return response.data;
        } catch (error) {
            return rejectWithValue(error.response?.data?.message || 'Failed to update profile');
        }
    }
);

const initialState = {
    userData: null,
    loading: false,
    error: null
};

const userSlice = createSlice({
    name: 'user',
    initialState,
    reducers: {
        clearUserError: (state) => {
            state.error = null;
        }
    },
    extraReducers: (builder) => {
        builder
            .addCase(fetchUserData.pending, (state) => {
                state.loading = true;
                state.error = null;
            })
            .addCase(fetchUserData.fulfilled, (state, action) => {
                state.loading = false;
                state.userData = action.payload;
                state.error = null;
            })
            .addCase(fetchUserData.rejected, (state, action) => {
                state.loading = false;
                state.error = action.payload;
            })
            .addCase(updateUserProfile.pending, (state) => {
                state.loading = true;
                state.error = null;
            })
            .addCase(updateUserProfile.fulfilled, (state, action) => {
                state.loading = false;
                state.userData = { ...state.userData, ...action.payload };
                state.error = null;
            })
            .addCase(updateUserProfile.rejected, (state, action) => {
                state.loading = false;
                state.error = action.payload;
            });
    }
});

export const { clearUserError } = userSlice.actions;
export default userSlice.reducer;
```

---

## client/src/styles/globals.css

```css
/* Global styles with modern CSS features */

/* CSS Custom Properties (Variables) */
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  --danger-color: #dc3545;
  --warning-color: #ffc107;
  --info-color: #17a2b8;
  --light-color: #f8f9fa;
  --dark-color: #343a40;
  
  --font-family-sans-serif: 'Roboto', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --border-radius: 0.375rem;
  --box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
  --transition: all 0.15s ease-in-out;
}

/* Reset and base styles */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: var(--font-family-sans-serif);
  line-height: 1.6;
  color: var(--dark-color);
  background-color: #ffffff;
}

/* Utility classes */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}

/* Custom button styles */
.btn {
  transition: var(--transition);
  border-radius: var(--border-radius);
  font-weight: 500;
}

.btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
}

/* Card styles */
.card {
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
  border: 1px solid rgba(0, 0, 0, 0.125);
  transition: var(--transition);
}

.card:hover {
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
}

/* Form styles */
.form-control {
  border-radius: var(--border-radius);
  transition: var(--transition);
}

.form-control:focus {
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

/* Loading spinner */
.spinner-border {
  width: 1rem;
  height: 1rem;
}

/* Sidebar styles */
.sidebar {
  width: 250px;
  min-height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  transition: var(--transition);
  z-index: 1000;
}

.sidebar.collapsed {
  width: 80px;
}

.sidebar .nav-link:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.sidebar .nav-link.active {
  background-color: var(--primary-color);
}

/* Responsive design */
@media (max-width: 768px) {
  .sidebar {
    transform: translateX(-100%);
  }
  
  .sidebar.show {
    transform: translateX(0);
  }
}

/* Animation keyframes */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-in {
  animation: fadeIn 0.5s ease-in-out;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #555;
}
```

---

## client/src/styles/components.scss

```scss
// SCSS variables
$primary-color: #007bff;
$secondary-color: #6c757d;
$success-color: #28a745;
$danger-color: #dc3545;
$warning-color: #ffc107;

$border-radius: 0.375rem;
$box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
$transition: all 0.15s ease-in-out;

// Mixins
@mixin button-variant($color) {
  background-color: $color;
  border-color: $color;
  
  &:hover {
    background-color: darken($color, 7.5%);
    border-color: darken($color, 10%);
  }
  
  &:focus {
    box-shadow: 0 0 0 0.2rem rgba($color, 0.5);
  }
}

@mixin card-style {
  border-radius: $border-radius;
  box-shadow: $box-shadow;
  border: 1px solid rgba(0, 0, 0, 0.125);
  transition: $transition;
  
  &:hover {
    box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
  }
}

// Component styles
.auth-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  
  .auth-card {
    @include card-style;
    width: 100%;
    max-width: 400px;
    padding: 2rem;
    background: white;
  }
}

.dashboard-container {
  padding: 2rem 0;
  
  .stats-card {
    @include card-style;
    text-align: center;
    padding: 1.5rem;
    
    .stat-number {
      font-size: 2rem;
      font-weight: bold;
      margin-bottom: 0.5rem;
    }
    
    .stat-label {
      color: $secondary-color;
      font-size: 0.875rem;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }
  }
}

.header-nav {
  background: linear-gradient(90deg, #2c3e50 0%, #3498db 100%);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  
  .navbar-brand {
    font-weight: bold;
    font-size: 1.25rem;
  }
  
  .nav-link {
    transition: $transition;
    
    &:hover {
      color: rgba(255, 255, 255, 0.8);
    }
  }
}

.footer {
  background-color: $dark-color;
  color: white;
  padding: 3rem 0 1rem;
  margin-top: auto;
  
  .footer-links {
    list-style: none;
    padding: 0;
    
    li {
      margin-bottom: 0.5rem;
      
      a {
        color: rgba(255, 255, 255, 0.7);
        text-decoration: none;
        transition: $transition;
        
        &:hover {
          color: white;
        }
      }
    }
  }
}

// Form components
.form-group {
  margin-bottom: 1rem;
  
  label {
    font-weight: 500;
    margin-bottom: 0.5rem;
    display: block;
  }
  
  .form-control {
    border-radius: $border-radius;
    border: 1px solid #ced4da;
    transition: $transition;
    
    &:focus {
      border-color: $primary-color;
      box-shadow: 0 0 0 0.2rem rgba($primary-color, 0.25);
    }
    
    &.is-invalid {
      border-color: $danger-color;
      
      &:focus {
        box-shadow: 0 0 0 0.2rem rgba($danger-color, 0.25);
      }
    }
  }
  
  .invalid-feedback {
    color: $danger-color;
    font-size: 0.875rem;
    margin-top: 0.25rem;
  }
}

// Button variants
.btn-primary {
  @include button-variant($primary-color);
}

.btn-success {
  @include button-variant($success-color);
}

.btn-danger {
  @include button-variant($danger-color);
}

.btn-warning {
  @include button-variant($warning-color);
}

// Responsive utilities
@media (max-width: 768px) {
  .auth-container .auth-card {
    margin: 1rem;
    padding: 1.5rem;
  }
  
  .dashboard-container {
    padding: 1rem 0;
  }
  
  .stats-card .stat-number {
    font-size: 1.5rem;
  }
}
```

---

## server/controllers/authController.js

```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const { validationResult } = require('express-validator');
const User = require('../models/User');
const authService = require('../services/authService');
const tokenService = require('../services/tokenService');

class AuthController {
    // Register new user
    async register(req, res) {
        try {
            // Check validation errors
            const errors = validationResult(req);
            if (!errors.isEmpty()) {
                return res.status(400).json({
                    success: false,
                    message: 'Validation failed',
                    errors: errors.array()
                });
            }

            const { name, email, password } = req.body;

            // Check if user already exists
            const existingUser = await User.findOne({ email });
            if (existingUser) {
                return res.status(409).json({
                    success: false,
                    message: 'User already exists with this email'
                });
            }

            // Hash password with salt
            const saltRounds = 12;
            const hashedPassword = await bcrypt.hash(password, saltRounds);

            // Create new user
            const user = new User({
                name,
                email,
                password: hashedPassword,
                createdAt: new Date()
            });

            await user.save();

            // Remove password from response
            const userResponse = user.toObject();
            delete userResponse.password;

            res.status(201).json({
                success: true,
                message: 'User registered successfully',
                user: userResponse
            });

        } catch (error) {
            console.error('Registration error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Login user
    async login(req, res) {
        try {
            const errors = validationResult(req);
            if (!errors.isEmpty()) {
                return res.status(400).json({
                    success: false,
                    message: 'Validation failed',
                    errors: errors.array()
                });
            }

            const { email, password } = req.body;

            // Find user by email
            const user = await User.findOne({ email });
            if (!user) {
                return res.status(401).json({
                    success: false,
                    message: 'Invalid credentials'
                });
            }

            // Verify password
            const isPasswordValid = await bcrypt.compare(password, user.password);
            if (!isPasswordValid) {
                return res.status(401).json({
                    success: false,
                    message: 'Invalid credentials'
                });
            }

            // Generate tokens
            const tokens = await tokenService.generateTokens({
                id: user._id,
                email: user.email,
                role: user.role
            });

            // Update last login
            user.lastLogin = new Date();
            await user.save();

            // Set HTTP-only cookie for refresh token
            res.cookie('refreshToken', tokens.refreshToken, {
                httpOnly: true,
                secure: process.env.NODE_ENV === 'production',
                sameSite: 'strict',
                maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
            });

            // Remove sensitive data
            const userResponse = user.toObject();
            delete userResponse.password;

            res.json({
                success: true,
                message: 'Login successful',
                token: tokens.accessToken,
                user: userResponse
            });

        } catch (error) {
            console.error('Login error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Logout user
    async logout(req, res) {
        try {
            const refreshToken = req.cookies.refreshToken;
            
            if (refreshToken) {
                await tokenService.revokeRefreshToken(refreshToken);
            }

            res.clearCookie('refreshToken');
            res.json({
                success: true,
                message: 'Logout successful'
            });

        } catch (error) {
            console.error('Logout error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    //
