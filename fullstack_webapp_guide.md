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
```

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

## client/src/styles/variables.scss

```scss
// Color Variables
:root {
  // Primary Colors
  --color-primary: #3b82f6;
  --color-primary-light: #60a5fa;
  --color-primary-dark: #1d4ed8;
  --color-primary-50: #eff6ff;
  --color-primary-100: #dbeafe;
  --color-primary-200: #bfdbfe;
  --color-primary-300: #93c5fd;
  --color-primary-400: #60a5fa;
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-700: #1d4ed8;
  --color-primary-800: #1e40af;
  --color-primary-900: #1e3a8a;

  // Secondary Colors
  --color-secondary: #64748b;
  --color-secondary-light: #94a3b8;
  --color-secondary-dark: #475569;

  // Accent Colors
  --color-accent: #f59e0b;
  --color-accent-light: #fbbf24;
  --color-accent-dark: #d97706;

  // Status Colors
  --color-success: #10b981;
  --color-success-light: #34d399;
  --color-success-dark: #059669;
  --color-warning: #f59e0b;
  --color-warning-light: #fbbf24;
  --color-warning-dark: #d97706;
  --color-error: #ef4444;
  --color-error-light: #f87171;
  --color-error-dark: #dc2626;
  --color-info: #06b6d4;
  --color-info-light: #22d3ee;
  --color-info-dark: #0891b2;

  // Neutral Colors
  --color-white: #ffffff;
  --color-black: #000000;
  --color-gray-50: #f9fafb;
  --color-gray-100: #f3f4f6;
  --color-gray-200: #e5e7eb;
  --color-gray-300: #d1d5db;
  --color-gray-400: #9ca3af;
  --color-gray-500: #6b7280;
  --color-gray-600: #4b5563;
  --color-gray-700: #374151;
  --color-gray-800: #1f2937;
  --color-gray-900: #111827;

  // Background Colors
  --bg-primary: var(--color-white);
  --bg-secondary: var(--color-gray-50);
  --bg-tertiary: var(--color-gray-100);
  --bg-dark: var(--color-gray-900);
  --bg-card: var(--color-white);
  --bg-overlay: rgba(0, 0, 0, 0.5);

  // Text Colors
  --text-primary: var(--color-gray-900);
  --text-secondary: var(--color-gray-600);
  --text-tertiary: var(--color-gray-500);
  --text-inverse: var(--color-white);
  --text-muted: var(--color-gray-400);

  // Border Colors
  --border-primary: var(--color-gray-200);
  --border-secondary: var(--color-gray-300);
  --border-focus: var(--color-primary);
  --border-error: var(--color-error);

  // Shadow Variables
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  --shadow-2xl: 0 25px 50px -12px rgba(0, 0, 0, 0.25);

  // Spacing Variables
  --spacing-xs: 0.25rem;   // 4px
  --spacing-sm: 0.5rem;    // 8px
  --spacing-md: 1rem;      // 16px
  --spacing-lg: 1.5rem;    // 24px
  --spacing-xl: 2rem;      // 32px
  --spacing-2xl: 3rem;     // 48px
  --spacing-3xl: 4rem;     // 64px

  // Typography Variables
  --font-family-sans: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-family-mono: 'Fira Code', 'Monaco', 'Consolas', monospace;
  
  --font-size-xs: 0.75rem;     // 12px
  --font-size-sm: 0.875rem;    // 14px
  --font-size-base: 1rem;      // 16px
  --font-size-lg: 1.125rem;    // 18px
  --font-size-xl: 1.25rem;     // 20px
  --font-size-2xl: 1.5rem;     // 24px
  --font-size-3xl: 1.875rem;   // 30px
  --font-size-4xl: 2.25rem;    // 36px
  --font-size-5xl: 3rem;       // 48px

  --font-weight-light: 300;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;

  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;

  // Border Radius Variables
  --radius-sm: 0.25rem;    // 4px
  --radius-md: 0.375rem;   // 6px
  --radius-lg: 0.5rem;     // 8px
  --radius-xl: 0.75rem;    // 12px
  --radius-2xl: 1rem;      // 16px
  --radius-full: 9999px;

  // Transition Variables
  --transition-fast: 150ms ease-in-out;
  --transition-normal: 300ms ease-in-out;
  --transition-slow: 500ms ease-in-out;

  // Z-Index Variables
  --z-dropdown: 1000;
  --z-sticky: 1010;
  --z-fixed: 1020;
  --z-modal-backdrop: 1030;
  --z-modal: 1040;
  --z-popover: 1050;
  --z-tooltip: 1060;
  --z-toast: 1070;

  // Layout Variables
  --container-max-width: 1200px;
  --sidebar-width: 280px;
  --header-height: 64px;
  --footer-height: 80px;
}

// Dark Mode Variables
[data-theme="dark"] {
  --bg-primary: var(--color-gray-900);
  --bg-secondary: var(--color-gray-800);
  --bg-tertiary: var(--color-gray-700);
  --bg-card: var(--color-gray-800);
  
  --text-primary: var(--color-gray-100);
  --text-secondary: var(--color-gray-300);
  --text-tertiary: var(--color-gray-400);
  
  --border-primary: var(--color-gray-700);
  --border-secondary: var(--color-gray-600);
}

// Component-specific Variables
:root {
  // Button Variables
  --btn-padding-x: 1rem;
  --btn-padding-y: 0.5rem;
  --btn-font-size: var(--font-size-sm);
  --btn-border-radius: var(--radius-md);
  --btn-transition: var(--transition-fast);

  // Input Variables
  --input-padding-x: 0.75rem;
  --input-padding-y: 0.5rem;
  --input-font-size: var(--font-size-sm);
  --input-border-radius: var(--radius-md);
  --input-border-width: 1px;
  --input-transition: var(--transition-fast);

  // Card Variables
  --card-padding: 1.5rem;
  --card-border-radius: var(--radius-lg);
  --card-shadow: var(--shadow-md);

  // Modal Variables
  --modal-max-width: 32rem;
  --modal-padding: 1.5rem;
  --modal-border-radius: var(--radius-lg);
}

// Breakpoint Variables (for use in JavaScript)
:root {
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
  --breakpoint-2xl: 1536px;
}

// Animation Variables
:root {
  --animation-fade-in: fadeIn 0.3s ease-in-out;
  --animation-slide-up: slideUp 0.3s ease-out;
  --animation-slide-down: slideDown 0.3s ease-out;
  --animation-scale-in: scaleIn 0.2s ease-out;
  --animation-bounce: bounce 0.5s ease-in-out;
}

// Keyframe Animations
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { 
    opacity: 0;
    transform: translateY(20px);
  }
  to { 
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideDown {
  from { 
    opacity: 0;
    transform: translateY(-20px);
  }
  to { 
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes scaleIn {
  from { 
    opacity: 0;
    transform: scale(0.95);
  }
  to { 
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes bounce {
  0%, 20%, 53%, 80%, 100% {
    transform: translate3d(0, 0, 0);
  }
  40%, 43% {
    transform: translate3d(0, -30px, 0);
  }
  70% {
    transform: translate3d(0, -15px, 0);
  }
  90% {
    transform: translate3d(0, -4px, 0);
  }
}
```

---

## client/src/utils/constants.js

```javascript
// API Configuration
export const API_CONFIG = {
  BASE_URL: process.env.REACT_APP_API_URL || 'http://localhost:5000/api',
  TIMEOUT: 30000, // 30 seconds
  RETRY_ATTEMPTS: 3,
  RETRY_DELAY: 1000, // 1 second
};

// Authentication Constants
export const AUTH_CONFIG = {
  TOKEN_KEY: 'accessToken',
  REFRESH_TOKEN_KEY: 'refreshToken',
  USER_KEY: 'user',
  TOKEN_EXPIRY_KEY: 'tokenExpiry',
  REMEMBER_ME_KEY: 'rememberMe',
  LOGIN_REDIRECT_KEY: 'loginRedirect',
};

// Application Routes
export const ROUTES = {
  HOME: '/',
  LOGIN: '/login',
  REGISTER: '/register',
  FORGOT_PASSWORD: '/forgot-password',
  RESET_PASSWORD: '/reset-password',
  VERIFY_EMAIL: '/verify-email',
  DASHBOARD: '/dashboard',
  PROFILE: '/profile',
  SETTINGS: '/settings',
  USERS: '/users',
  ADMIN: '/admin',
  NOT_FOUND: '/404',
  UNAUTHORIZED: '/unauthorized',
  SERVER_ERROR: '/500',
};

// User Roles and Permissions
export const USER_ROLES = {
  ADMIN: 'admin',
  MODERATOR: 'moderator',
  USER: 'user',
  GUEST: 'guest',
};

export const PERMISSIONS = {
  CREATE_USER: 'create:user',
  READ_USER: 'read:user',
  UPDATE_USER: 'update:user',
  DELETE_USER: 'delete:user',
  MANAGE_USERS: 'manage:users',
  ADMIN_ACCESS: 'admin:access',
};

// HTTP Status Codes
export const HTTP_STATUS = {
  OK: 200,
  CREATED: 201,
  NO_CONTENT: 204,
  BAD_REQUEST: 400,
  UNAUTHORIZED: 401,
  FORBIDDEN: 403,
  NOT_FOUND: 404,
  CONFLICT: 409,
  UNPROCESSABLE_ENTITY: 422,
  TOO_MANY_REQUESTS: 429,
  INTERNAL_SERVER_ERROR: 500,
  BAD_GATEWAY: 502,
  SERVICE_UNAVAILABLE: 503,
};

// Form Validation Constants
export const VALIDATION_RULES = {
  EMAIL: {
    PATTERN: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
    MAX_LENGTH: 255,
  },
  PASSWORD: {
    MIN_LENGTH: 8,
    MAX_LENGTH: 128,
    PATTERN: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/,
  },
  NAME: {
    MIN_LENGTH: 2,
    MAX_LENGTH: 100,
    PATTERN: /^[a-zA-Z\s'-]+$/,
  },
  PHONE: {
    PATTERN: /^[\+]?[1-9][\d]{0,15}$/,
  },
  USERNAME: {
    MIN_LENGTH: 3,
    MAX_LENGTH: 30,
    PATTERN: /^[a-zA-Z0-9_-]+$/,
  },
};

// File Upload Constants
export const FILE_UPLOAD = {
  MAX_SIZE: 5 * 1024 * 1024, // 5MB
  ALLOWED_TYPES: {
    IMAGES: ['image/jpeg', 'image/jpg', 'image/png', 'image/gif', 'image/webp'],
    DOCUMENTS: ['application/pdf', 'application/msword', 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'],
    EXCEL: ['application/vnd.ms-excel', 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'],
  },
  AVATAR_MAX_SIZE: 2 * 1024 * 1024, // 2MB
};

// Pagination Constants
export const PAGINATION = {
  DEFAULT_PAGE: 1,
  DEFAULT_LIMIT: 10,
  MAX_LIMIT: 100,
  PAGE_SIZE_OPTIONS: [10, 25, 50, 100],
};

// Date/Time Constants
export const DATE_FORMATS = {
  SHORT: 'MM/DD/YYYY',
  LONG: 'MMMM DD, YYYY',
  WITH_TIME: 'MM/DD/YYYY HH:mm',
  TIME_ONLY: 'HH:mm',
  ISO: 'YYYY-MM-DDTHH:mm:ss.SSSZ',
  API: 'YYYY-MM-DD',
};

export const TIME_ZONES = {
  UTC: 'UTC',
  EST: 'America/New_York',
  PST: 'America/Los_Angeles',
  GMT: 'Europe/London',
};

// UI Constants
export const UI_CONFIG = {
  TOAST_DURATION: 5000, // 5 seconds
  DEBOUNCE_DELAY: 300, // 300ms
  ANIMATION_DURATION: 300, // 300ms
  MOBILE_BREAKPOINT: 768, // px
  TABLET_BREAKPOINT: 1024, // px
  DESKTOP_BREAKPOINT: 1280, // px
};

// Theme Constants
export const THEMES = {
  LIGHT: 'light',
  DARK: 'dark',
  SYSTEM: 'system',
};

export const THEME_COLORS = {
  PRIMARY: '#3b82f6',
  SECONDARY: '#64748b',
  SUCCESS: '#10b981',
  WARNING: '#f59e0b',
  ERROR: '#ef4444',
  INFO: '#06b6d4',
};

// Storage Keys (LocalStorage/SessionStorage)
export const STORAGE_KEYS = {
  THEME: 'app_theme',
  LANGUAGE: 'app_language',
  SIDEBAR_COLLAPSED: 'sidebar_collapsed',
  TABLE_SETTINGS: 'table_settings',
  FORM_DRAFTS: 'form_drafts',
  RECENT_SEARCHES: 'recent_searches',
  USER_PREFERENCES: 'user_preferences',
};

// Error Messages
export const ERROR_MESSAGES = {
  NETWORK_ERROR: 'Network error. Please check your connection.',
  SERVER_ERROR: 'Server error. Please try again later.',
  VALIDATION_ERROR: 'Please fix the validation errors.',
  UNAUTHORIZED: 'You are not authorized to perform this action.',
  FORBIDDEN: 'Access denied.',
  NOT_FOUND: 'The requested resource was not found.',
  TIMEOUT: 'Request timeout. Please try again.',
  FILE_TOO_LARGE: 'File size exceeds the maximum limit.',
  INVALID_FILE_TYPE: 'Invalid file type.',
  GENERIC_ERROR: 'An error occurred. Please try again.',
};

// Success Messages
export const SUCCESS_MESSAGES = {
  SAVE_SUCCESS: 'Changes saved successfully.',
  CREATE_SUCCESS: 'Created successfully.',
  UPDATE_SUCCESS: 'Updated successfully.',
  DELETE_SUCCESS: 'Deleted successfully.',
  LOGIN_SUCCESS: 'Logged in successfully.',
  LOGOUT_SUCCESS: 'Logged out successfully.',
  EMAIL_SENT: 'Email sent successfully.',
  PASSWORD_RESET: 'Password reset successfully.',
  PROFILE_UPDATED: 'Profile updated successfully.',
};

// Loading States
export const LOADING_STATES = {
  IDLE: 'idle',
  LOADING: 'loading',
  SUCCESS: 'success',
  ERROR: 'error',
};

// Modal Types
export const MODAL_TYPES = {
  CONFIRM: 'confirm',
  ALERT: 'alert',
  FORM: 'form',
  INFO: 'info',
  CUSTOM: 'custom',
};

// Table Constants
export const TABLE_CONFIG = {
  DEFAULT_SORT: 'createdAt',
  DEFAULT_ORDER: 'desc',
  ROW_ACTIONS: {
    VIEW: 'view',
    EDIT: 'edit',
    DELETE: 'delete',
    DUPLICATE: 'duplicate',
  },
};

// Chart/Graph Constants
export const CHART_COLORS = [
  '#3b82f6', '#ef4444', '#10b981', '#f59e0b',
  '#8b5cf6', '#06b6d4', '#f97316', '#84cc16',
  '#ec4899', '#6366f1', '#14b8a6', '#f43f5e',
];

// Social Media Constants
export const SOCIAL_MEDIA = {
  FACEBOOK: 'facebook',
  TWITTER: 'twitter',
  LINKEDIN: 'linkedin',
  INSTAGRAM: 'instagram',
  GITHUB: 'github',
  GOOGLE: 'google',
};

// Notification Types
export const NOTIFICATION_TYPES = {
  INFO: 'info',
  SUCCESS: 'success',
  WARNING: 'warning',
  ERROR: 'error',
};

// Language Constants
export const LANGUAGES = {
  EN: 'en',
  ES: 'es',
  FR: 'fr',
  DE: 'de',
  IT: 'it',
  PT: 'pt',
  RU: 'ru',
  ZH: 'zh',
  JA: 'ja',
  KO: 'ko',
};

// Export all constants as default
export default {
  API_CONFIG,
  AUTH_CONFIG,
  ROUTES,
  USER_ROLES,
  PERMISSIONS,
  HTTP_STATUS,
  VALIDATION_RULES,
  FILE_UPLOAD,
  PAGINATION,
  DATE_FORMATS,
  TIME_ZONES,
  UI_CONFIG,
  THEMES,
  THEME_COLORS,
  STORAGE_KEYS,
  ERROR_MESSAGES,
  SUCCESS_MESSAGES,
  LOADING_STATES,
  MODAL_TYPES,
  TABLE_CONFIG,
  CHART_COLORS,
  SOCIAL_MEDIA,
  NOTIFICATION_TYPES,
  LANGUAGES,
};
```

## client/src/utils/helpers.js

```javascript
// ES6+ utility functions for common operations

// Format date with ES6+ template literals
export const formatDate = (date, format = 'short') => {
    const dateObj = new Date(date);
    
    const formats = {
        short: dateObj.toLocaleDateString(),
        long: dateObj.toLocaleDateString('en-US', { 
            year: 'numeric', 
            month: 'long', 
            day: 'numeric' 
        }),
        time: dateObj.toLocaleTimeString(),
        datetime: dateObj.toLocaleString()
    };
    
    return formats[format] || formats.short;
};

// Debounce function using ES6+ arrow functions and closures
export const debounce = (func, delay) => {
    let timeoutId;
    return (...args) => {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(null, args), delay);
    };
};

// Throttle function for performance optimization
export const throttle = (func, limit) => {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
};

// Validate email with ES6+ regex
export const isValidEmail = (email) => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
};

// Validate password strength
export const validatePassword = (password) => {
    const minLength = 8;
    const hasUpperCase = /[A-Z]/.test(password);
    const hasLowerCase = /[a-z]/.test(password);
    const hasNumbers = /\d/.test(password);
    const hasSpecialChar = /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]/.test(password);
    
    return {
        isValid: password.length >= minLength && hasUpperCase && hasLowerCase && hasNumbers,
        requirements: {
            length: password.length >= minLength,
            uppercase: hasUpperCase,
            lowercase: hasLowerCase,
            numbers: hasNumbers,
            special: hasSpecialChar
        }
    };
};

// Generate random string for tokens/IDs
export const generateRandomString = (length = 16) => {
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    return Array.from({ length }, () => chars.charAt(Math.floor(Math.random() * chars.length))).join('');
};

// Format file size in human readable format
export const formatFileSize = (bytes) => {
    if (bytes === 0) return '0 Bytes';
    
    const k = 1024;
    const sizes = ['Bytes', 'KB', 'MB', 'GB'];
    const i = Math.floor(Math.log(bytes) / Math.log(k));
    
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
};

// Capitalize first letter of each word
export const capitalizeWords = (str) => {
    return str.replace(/\w\S*/g, (txt) => 
        txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase()
    );
};

// Deep clone object using ES6+ spread and recursion
export const deepClone = (obj) => {
    if (obj === null || typeof obj !== 'object') return obj;
    if (obj instanceof Date) return new Date(obj.getTime());
    if (obj instanceof Array) return obj.map(item => deepClone(item));
    if (typeof obj === 'object') {
        const clonedObj = {};
        Object.keys(obj).forEach(key => {
            clonedObj[key] = deepClone(obj[key]);
        });
        return clonedObj;
    }
};

// Check if object is empty
export const isEmpty = (obj) => {
    return obj === null || obj === undefined || 
           (typeof obj === 'object' && Object.keys(obj).length === 0) ||
           (typeof obj === 'string' && obj.trim().length === 0);
};

// Format currency
export const formatCurrency = (amount, currency = 'USD') => {
    return new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: currency
    }).format(amount);
};

// Get initials from name
export const getInitials = (name) => {
    return name.split(' ')
        .map(word => word.charAt(0).toUpperCase())
        .join('')
        .substring(0, 2);
};

// Storage helpers (with fallback for server-side rendering)
export const storage = {
    get: (key) => {
        try {
            if (typeof window !== 'undefined') {
                const item = localStorage.getItem(key);
                return item ? JSON.parse(item) : null;
            }
        } catch (error) {
            console.error('Error getting from localStorage:', error);
        }
        return null;
    },
    
    set: (key, value) => {
        try {
            if (typeof window !== 'undefined') {
                localStorage.setItem(key, JSON.stringify(value));
            }
        } catch (error) {
            console.error('Error setting to localStorage:', error);
        }
    },
    
    remove: (key) => {
        try {
            if (typeof window !== 'undefined') {
                localStorage.removeItem(key);
            }
        } catch (error) {
            console.error('Error removing from localStorage:', error);
        }
    }
};

// URL helpers
export const getQueryParams = () => {
    if (typeof window === 'undefined') return {};
    
    const params = new URLSearchParams(window.location.search);
    const result = {};
    
    for (let [key, value] of params) {
        result[key] = value;
    }
    
    return result;
};

export const updateQueryParams = (params) => {
    if (typeof window === 'undefined') return;
    
    const url = new URL(window.location);
    Object.keys(params).forEach(key => {
        if (params[key]) {
            url.searchParams.set(key, params[key]);
        } else {
            url.searchParams.delete(key);
        }
    });
    
    window.history.replaceState({}, '', url);
};
```

---

## client/src/App.jsx

```jsx
import React, { useEffect } from 'react';
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';
import { useDispatch, useSelector } from 'react-redux';
import { checkAuthStatus } from './store/authSlice';

// Layout Components
import Header from './components/Layout/Header';
import Footer from './components/Layout/Footer';
import Sidebar from './components/Layout/Sidebar';

// Page Components
import Home from './pages/Home';
import About from './pages/About';
import Contact from './pages/Contact';

// Auth Components
import Login from './components/Auth/Login';
import Register from './components/Auth/Register';

// Dashboard Components
import Dashboard from './components/Dashboard/Dashboard';
import UserProfile from './components/Dashboard/UserProfile';

// Hooks
import { useAuth } from './hooks/useAuth';

// Styles
import './styles/globals.css';

// Protected Route Component
const ProtectedRoute = ({ children }) => {
    const { isAuthenticated, loading } = useAuth();
    
    if (loading) {
        return (
            <div className="d-flex justify-content-center align-items-center min-vh-100">
                <div className="spinner-border text-primary" role="status">
                    <span className="visually-hidden">Loading...</span>
                </div>
            </div>
        );
    }
    
    return isAuthenticated ? children : <Navigate to="/login" replace />;
};

// Public Route Component (redirect if authenticated)
const PublicRoute = ({ children }) => {
    const { isAuthenticated, loading } = useAuth();
    
    if (loading) {
        return (
            <div className="d-flex justify-content-center align-items-center min-vh-100">
                <div className="spinner-border text-primary" role="status">
                    <span className="visually-hidden">Loading...</span>
                </div>
            </div>
        );
    }
    
    return !isAuthenticated ? children : <Navigate to="/dashboard" replace />;
};

// Main App Component
const App = () => {
    const dispatch = useDispatch();
    const { isAuthenticated, loading } = useSelector(state => state.auth);

    // Check authentication status on app load
    useEffect(() => {
        dispatch(checkAuthStatus());
    }, [dispatch]);

    // Show loading spinner during initial auth check
    if (loading) {
        return (
            <div className="d-flex justify-content-center align-items-center min-vh-100">
                <div className="spinner-border text-primary" role="status">
                    <span className="visually-hidden">Loading...</span>
                </div>
            </div>
        );
    }

    return (
        <Router>
            <div className="App d-flex flex-column min-vh-100">
                <Header />
                
                <div className={`main-content flex-grow-1 ${isAuthenticated ? 'd-flex' : ''}`}>
                    {/* Sidebar for authenticated users */}
                    {isAuthenticated && <Sidebar />}
                    
                    {/* Main content area */}
                    <div className={`content ${isAuthenticated ? 'flex-grow-1' : 'container-fluid'}`}>
                        <Routes>
                            {/* Public Routes */}
                            <Route path="/" element={<Home />} />
                            <Route path="/about" element={<About />} />
                            <Route path="/contact" element={<Contact />} />
                            
                            {/* Auth Routes (only for non-authenticated users) */}
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
                            
                            {/* Protected Routes (only for authenticated users) */}
                            <Route 
                                path="/dashboard" 
                                element={
                                    <ProtectedRoute>
                                        <Dashboard />
                                    </ProtectedRoute>
                                } 
                            />
                            <Route 
                                path="/profile" 
                                element={
                                    <ProtectedRoute>
                                        <UserProfile />
                                    </ProtectedRoute>
                                } 
                            />
                            
                            {/* Catch all route - 404 */}
                            <Route 
                                path="*" 
                                element={
                                    <div className="container mt-5 text-center">
                                        <h1>404 - Page Not Found</h1>
                                        <p>The page you're looking for doesn't exist.</p>
                                        <a href="/" className="btn btn-primary">Go Home</a>
                                    </div>
                                } 
                            />
                        </Routes>
                    </div>
                </div>
                
                <Footer />
            </div>
        </Router>
    );
};

export default App;
```

---

## client/src/index.js

```javascript
import React from 'react';
import { createRoot } from 'react-dom/client'; // ES6+ React 18 syntax
import { Provider } from 'react-redux';
import { store } from './store';
import App from './App';

// Import global styles
import 'bootstrap/dist/css/bootstrap.min.css'; // Bootstrap CSS
import './styles/globals.css'; // Custom global styles

// Error Boundary Component
class ErrorBoundary extends React.Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false, error: null };
    }

    static getDerivedStateFromError(error) {
        // Update state so the next render will show the fallback UI
        return { hasError: true, error };
    }

    componentDidCatch(error, errorInfo) {
        // Log error to console and external service
        console.error('Error caught by boundary:', error, errorInfo);
        
        // In production, you might want to log this to an error reporting service
        // like Sentry, LogRocket, etc.
    }

    render() {
        if (this.state.hasError) {
            return (
                <div className="container mt-5 text-center">
                    <div className="alert alert-danger">
                        <h4>Something went wrong!</h4>
                        <p>We're sorry, but something unexpected happened.</p>
                        <button 
                            className="btn btn-primary"
                            onClick={() => window.location.reload()}
                        >
                            Reload Page
                        </button>
                    </div>
                </div>
            );
        }

        return this.props.children;
    }
}

// Get root element
const container = document.getElementById('root');

// Create root using React 18's createRoot API (ES6+ syntax)
const root = createRoot(container);

// Render app with error boundary and Redux provider
root.render(
    <React.StrictMode>
        <ErrorBoundary>
            <Provider store={store}>
                <App />
            </Provider>
        </ErrorBoundary>
    </React.StrictMode>
);

// Service Worker registration (optional)
// Uncomment below if you want to enable service worker for PWA features
/*
if ('serviceWorker' in navigator && process.env.NODE_ENV === 'production') {
    window.addEventListener('load', () => {
        navigator.serviceWorker.register('/sw.js')
            .then((registration) => {
                console.log('SW registered: ', registration);
            })
            .catch((registrationError) => {
                console.log('SW registration failed: ', registrationError);
            });
    });
}
*/

// Hot Module Replacement (HMR) for development
if (process.env.NODE_ENV === 'development' && module.hot) {
    module.hot.accept('./App', () => {
        const NextApp = require('./App').default;
        root.render(
            <React.StrictMode>
                <ErrorBoundary>
                    <Provider store={store}>
                        <NextApp />
                    </Provider>
                </ErrorBoundary>
            </React.StrictMode>
        );
    });
}

// Performance monitoring (optional)
// Uncomment and modify as needed for performance tracking
/*
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

function sendToAnalytics(metric) {
    // Send metrics to your analytics service
    console.log(metric);
}

getCLS(sendToAnalytics);
getFID(sendToAnalytics);
getFCP(sendToAnalytics);
getLCP(sendToAnalytics);
getTTFB(sendToAnalytics);
*/
```

---

## client/package.json

```json
{
  "name": "fullstack-client",
  "version": "1.0.0",
  "description": "Full-stack web application frontend built with React, Redux, and modern web technologies",
  "private": true,
  "homepage": ".",
  "dependencies": {
    "@reduxjs/toolkit": "^1.9.7",
    "@testing-library/jest-dom": "^5.17.0",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^14.5.2",
    "axios": "^1.6.0",
    "bootstrap": "^5.3.2",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-redux": "^8.1.3",
    "react-router-dom": "^6.17.0",
    "react-scripts": "5.0.1",
    "redux": "^4.2.1",
    "redux-persist": "^6.0.0",
    "sass": "^1.69.5",
    "web-vitals": "^3.5.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.37",
    "@types/react-dom": "^18.2.15",
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.31",
    "prop-types": "^15.8.1",
    "tailwindcss": "^3.3.5"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "lint": "eslint src --ext .js,.jsx",
    "lint:fix": "eslint src --ext .js,.jsx --fix",
    "format": "prettier --write src/**/*.{js,jsx,css,scss}",
    "analyze": "npm run build && npx webpack-bundle-analyzer build/static/js/*.js"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ],
    "rules": {
      "no-unused-vars": "warn",
      "no-console": "off",
      "react-hooks/exhaustive-deps": "warn"
    }
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "proxy": "http://localhost:5000",
  "engines": {
    "node": ">=16.0.0",
    "npm": ">=8.0.0"
  },
  "keywords": [
    "react",
    "redux",
    "fullstack",
    "authentication",
    "jwt",
    "oauth",
    "bootstrap",
    "tailwindcss",
    "typescript"
  ],
  "author": "Your Name",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/fullstack-webapp.git"
  },
  "bugs": {
    "url": "https://github.com/yourusername/fullstack-webapp/issues"
  }
}
```

---

## client/tailwind.config.js

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  // Content paths for Tailwind to scan for classes
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
    "./public/index.html"
  ],
  
  // Dark mode configuration
  darkMode: 'class', // Enable class-based dark mode
  
  theme: {
    extend: {
      // Custom colors with ES6+ object shorthand
      colors: {
        primary: {
          50: '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6', // Main primary color
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
        },
        secondary: {
          50: '#f8fafc',
          100: '#f1f5f9',
          200: '#e2e8f0',
          300: '#cbd5e1',
          400: '#94a3b8',
          500: '#64748b', // Main secondary color
          600: '#475569',
          700: '#334155',
          800: '#1e293b',
          900: '#0f172a',
        },
        success: '#10b981',
        warning: '#f59e0b',
        error: '#ef4444',
        info: '#06b6d4'
      },
      
      // Custom fonts
      fontFamily: {
        sans: ['Roboto', 'system-ui', 'sans-serif'],
        serif: ['Georgia', 'serif'],
        mono: ['Monaco', 'monospace']
      },
      
      // Custom spacing
      spacing: {
        '18': '4.5rem',
        '88': '22rem',
        '128': '32rem'
      },
      
      // Custom border radius
      borderRadius: {
        'xl': '1rem',
        '2xl': '1.5rem',
        '3xl': '2rem'
      },
      
      // Custom box shadows
      boxShadow: {
        'custom': '0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)',
        'custom-lg': '0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05)',
        'inner-custom': 'inset 0 2px 4px 0 rgba(0, 0, 0, 0.06)'
      },
      
      // Custom animations
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.3s ease-out',
        'slide-down': 'slideDown 0.3s ease-out',
        'pulse-slow': 'pulse 3s infinite'
      },
      
      // Custom keyframes
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' }
        },
        slideUp: {
          '0%': { transform: 'translateY(100%)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' }
        },
        slideDown: {
          '0%': { transform: 'translateY(-100%)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' }
        }
      },
      
      // Custom breakpoints
      screens: {
        'xs': '475px',
        '3xl': '1600px'
      },
      
      // Custom typography
      fontSize: {
        'xs': ['0.75rem', { lineHeight: '1rem' }],
        'sm': ['0.875rem', { lineHeight: '1.25rem' }],
        'base': ['1rem', { lineHeight: '1.5rem' }],
        'lg': ['1.125rem', { lineHeight: '1.75rem' }],
        'xl': ['1.25rem', { lineHeight: '1.75rem' }],
        '2xl': ['1.5rem', { lineHeight: '2rem' }],
        '3xl': ['1.875rem', { lineHeight: '2.25rem' }],
        '4xl': ['2.25rem', { lineHeight: '2.5rem' }],
        '5xl': ['3rem', { lineHeight: '1' }],
        '6xl': ['3.75rem', { lineHeight: '1' }]
      }
    }
  },
  
  // Plugins for additional functionality
  plugins: [
    // Forms plugin for styling form elements
    require('@tailwindcss/forms')({
      strategy: 'class' // Use class-based form styling
    }),
    
    // Typography plugin for rich text
    require('@tailwindcss/typography'),
    
    // Aspect ratio plugin for responsive media
    require('@tailwindcss/aspect-ratio'),
    
    // Custom plugin for utilities
    function({ addUtilities, addComponents, theme }) {
      // Custom utilities
      const newUtilities = {
        '.text-shadow': {
          textShadow: '2px 2px 4px rgba(0,0,0,0.10)'
        },
        '.text-shadow-md': {
          textShadow: '4px 4px 8px rgba(0,0,0,0.12)'
        },
        '.text-shadow-lg': {
          textShadow: '15px 15px 30px rgba(0,0,0,0.15)'
        },
        '.text-shadow-none': {
          textShadow: 'none'
        },
        '.writing-vertical': {
          writingMode: 'vertical-rl'
        }
      };
      
      // Custom components
      const newComponents = {
        '.btn-custom': {
          padding: theme('spacing.2') + ' ' + theme('spacing.4'),
          borderRadius: theme('borderRadius.md'),
          fontWeight: theme('fontWeight.medium'),
          transition: 'all 0.2s ease-in-out',
          '&:hover': {
            transform: 'translateY(-1px)',
            boxShadow: theme('boxShadow.lg')
          }
        },
        '.card-custom': {
          backgroundColor: theme('colors.white'),
          borderRadius: theme('borderRadius.lg'),
          padding: theme('spacing.6'),
          boxShadow: theme('boxShadow.custom'),
          border: '1px solid ' + theme('colors.gray.200')
        }
      };
      
      addUtilities(newUtilities);
      addComponents(newComponents);
    }
  ],
  
  // CorePlugins configuration
  corePlugins: {
    // Disable unused core plugins for smaller bundle size
    preflight: true, // Keep CSS reset
    container: true, // Keep container utilities
  },
  
  // Safelist for dynamic classes that might not be detected
  safelist: [
    'bg-red-500',
    'bg-green-500',
    'bg-blue-500',
    'text-red-500',
    'text-green-500',
    'text-blue-500',
    // Add patterns for dynamic classes
    {
      pattern: /bg-(red|green|blue|yellow|indigo|purple|pink)-(100|200|300|400|500|600|700|800|900)/
    },
    {
      pattern: /text-(red|green|blue|yellow|indigo|purple|pink)-(100|200|300|400|500|600|700|800|900)/
    }
  ]
};
```

---


## server/controllers/authController.js

```javascript
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const RefreshToken = require('../models/RefreshToken');
const authService = require('../services/authService');
const tokenService = require('../services/tokenService');
const { validationResult } = require('express-validator');

// ES6+ class-based controller (legacy: function-based)
class AuthController {
    // Register new user with ES6+ async/await
    static async register(req, res) {
        try {
            // Validate input
            const errors = validationResult(req);
            if (!errors.isEmpty()) {
                return res.status(400).json({
                    success: false,
                    errors: errors.array()
                });
            }

            const { name, email, password } = req.body;

            // Check if user already exists
            const existingUser = await User.findOne({ email });
            if (existingUser) {
                return res.status(400).json({
                    success: false,
                    message: 'User already exists'
                });
            }

            // Hash password with bcrypt + salt
            const saltRounds = 12; // ES6+ const instead of var
            const hashedPassword = await bcrypt.hash(password, saltRounds);

            // Create new user
            const user = new User({
                name,
                email,
                password: hashedPassword,
                provider: 'local'
            });

            await user.save();

            // Generate tokens
            const { accessToken, refreshToken } = await tokenService.generateTokens(user._id);

            // Save refresh token
            await tokenService.saveRefreshToken(user._id, refreshToken);

            res.status(201).json({
                success: true,
                message: 'User registered successfully',
                user: {
                    id: user._id,
                    name: user.name,
                    email: user.email,
                    role: user.role
                },
                accessToken,
                refreshToken
            });

        } catch (error) {
            console.error('Registration error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Login user with local strategy
    static async login(req, res) {
        try {
            const errors = validationResult(req);
            if (!errors.isEmpty()) {
                return res.status(400).json({
                    success: false,
                    errors: errors.array()
                });
            }

            const { email, password } = req.body;

            // Find user by email
            const user = await User.findOne({ email }).select('+password');
            if (!user) {
                return res.status(401).json({
                    success: false,
                    message: 'Invalid credentials'
                });
            }

            // Verify password
            const isValidPassword = await bcrypt.compare(password, user.password);
            if (!isValidPassword) {
                return res.status(401).json({
                    success: false,
                    message: 'Invalid credentials'
                });
            }

            // Update last login
            user.lastLogin = new Date();
            user.loginCount = (user.loginCount || 0) + 1;
            await user.save();

            // Generate tokens
            const { accessToken, refreshToken } = await tokenService.generateTokens(user._id);
            await tokenService.saveRefreshToken(user._id, refreshToken);

            // Create session if using session-based auth
            req.session.userId = user._id;

            res.json({
                success: true,
                message: 'Login successful',
                user: {
                    id: user._id,
                    name: user.name,
                    email: user.email,
                    role: user.role,
                    lastLogin: user.lastLogin
                },
                accessToken,
                refreshToken
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
    static async logout(req, res) {
        try {
            const { refreshToken } = req.body;
            const userId = req.user?.id;

            // Remove refresh token from database
            if (refreshToken) {
                await RefreshToken.deleteOne({ token: refreshToken });
            }

            // Destroy session
            req.session.destroy((err) => {
                if (err) {
                    console.error('Session destruction error:', err);
                }
            });

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

    // Refresh access token
    static async refreshToken(req, res) {
        try {
            const { refreshToken } = req.body;

            if (!refreshToken) {
                return res.status(401).json({
                    success: false,
                    message: 'Refresh token required'
                });
            }

            // Verify refresh token
            const tokenDoc = await RefreshToken.findOne({ token: refreshToken });
            if (!tokenDoc || tokenDoc.expiresAt < new Date()) {
                return res.status(401).json({
                    success: false,
                    message: 'Invalid or expired refresh token'
                });
            }

            // Generate new access token
            const { accessToken } = await tokenService.generateTokens(tokenDoc.userId);

            res.json({
                success: true,
                accessToken
            });

        } catch (error) {
            console.error('Token refresh error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Google OAuth callback
    static async googleCallback(req, res) {
        try {
            const user = req.user; // From Passport.js

            // Generate tokens
            const { accessToken, refreshToken } = await tokenService.generateTokens(user._id);
            await tokenService.saveRefreshToken(user._id, refreshToken);

            // Redirect to frontend with tokens
            const redirectUrl = `${process.env.CLIENT_URL}/auth/callback?token=${accessToken}&user=${encodeURIComponent(JSON.stringify({
                id: user._id,
                name: user.name,
                email: user.email,
                role: user.role
            }))}`;

            res.redirect(redirectUrl);

        } catch (error) {
            console.error('Google OAuth callback error:', error);
            res.redirect(`${process.env.CLIENT_URL}/login?error=oauth_failed`);
        }
    }

    // Get current user profile
    static async getProfile(req, res) {
        try {
            const userId = req.user.id;
            const user = await User.findById(userId).select('-password');

            if (!user) {
                return res.status(404).json({
                    success: false,
                    message: 'User not found'
                });
            }

            res.json({
                success: true,
                user
            });

        } catch (error) {
            console.error('Get profile error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }
}

module.exports = AuthController;
```

---

## server/controllers/userController.js

```javascript
const User = require('../models/User');
const userService = require('../services/userService');
const { validationResult } = require('express-validator');

class UserController {
    // Get all users (admin only)
    static async getAllUsers(req, res) {
        try {
            const { page = 1, limit = 10, search = '' } = req.query;
            
            // Build search query with ES6+ object shorthand
            const searchQuery = search ? {
                $or: [
                    { name: { $regex: search, $options: 'i' } },
                    { email: { $regex: search, $options: 'i' } }
                ]
            } : {};

            // Pagination with ES6+ destructuring
            const users = await User.find(searchQuery)
                .select('-password')
                .limit(limit * 1)
                .skip((page - 1) * limit)
                .sort({ createdAt: -1 });

            const total = await User.countDocuments(searchQuery);

            res.json({
                success: true,
                users,
                pagination: {
                    current: parseInt(page),
                    pages: Math.ceil(total / limit),
                    total
                }
            });

        } catch (error) {
            console.error('Get users error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Get user by ID
    static async getUserById(req, res) {
        try {
            const { id } = req.params;
            const user = await User.findById(id).select('-password');

            if (!user) {
                return res.status(404).json({
                    success: false,
                    message: 'User not found'
                });
            }

            res.json({
                success: true,
                user
            });

        } catch (error) {
            console.error('Get user error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Update user profile
    static async updateProfile(req, res) {
        try {
            const errors = validationResult(req);
            if (!errors.isEmpty()) {
                return res.status(400).json({
                    success: false,
                    errors: errors.array()
                });
            }

            const userId = req.user.id;
            const { name, phone, bio } = req.body;

            // Update user with ES6+ object shorthand
            const updatedUser = await User.findByIdAndUpdate(
                userId,
                { name, phone, bio, updatedAt: new Date() },
                { new: true, runValidators: true }
            ).select('-password');

            if (!updatedUser) {
                return res.status(404).json({
                    success: false,
                    message: 'User not found'
                });
            }

            res.json({
                success: true,
                message: 'Profile updated successfully',
                user: updatedUser
            });

        } catch (error) {
            console.error('Update profile error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Delete user (admin only)
    static async deleteUser(req, res) {
        try {
            const { id } = req.params;

            // Prevent self-deletion
            if (id === req.user.id) {
                return res.status(400).json({
                    success: false,
                    message: 'Cannot delete your own account'
                });
            }

            const deletedUser = await User.findByIdAndDelete(id);

            if (!deletedUser) {
                return res.status(404).json({
                    success: false,
                    message: 'User not found'
                });
            }

            res.json({
                success: true,
                message: 'User deleted successfully'
            });

        } catch (error) {
            console.error('Delete user error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Get user statistics
    static async getUserStats(req, res) {
        try {
            const userId = req.user.id;
            const stats = await userService.getUserStatistics(userId);

            res.json({
                success: true,
                stats
            });

        } catch (error) {
            console.error('Get user stats error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }
}

module.exports = UserController;
```

---

## server/controllers/dashboardController.js

```javascript
const User = require('../models/User');
const Session = require('../models/Session');

class DashboardController {
    // Get dashboard statistics
    static async getDashboardStats(req, res) {
        try {
            const userId = req.user.id;
            
            // ES6+ Promise.all for concurrent queries
            const [
                totalUsers,
                activeSessions,
                userLoginCount,
                recentUsers
            ] = await Promise.all([
                User.countDocuments(),
                Session.countDocuments({ expiresAt: { $gt: new Date() } }),
                User.findById(userId).select('loginCount'),
                User.find()
                    .select('name email createdAt lastLogin')
                    .sort({ createdAt: -1 })
                    .limit(5)
            ]);

            res.json({
                success: true,
                stats: {
                    totalUsers,
                    activeSessions,
                    loginCount: userLoginCount?.loginCount || 0,
                    recentUsers
                }
            });

        } catch (error) {
            console.error('Dashboard stats error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }

    // Get user activity data
    static async getUserActivity(req, res) {
        try {
            const userId = req.user.id;
            const { days = 7 } = req.query;

            // Calculate date range
            const startDate = new Date();
            startDate.setDate(startDate.getDate() - days);

            // Get user login activity (this would need a separate activity log in production)
            const user = await User.findById(userId)
                .select('name email lastLogin loginCount createdAt');

            res.json({
                success: true,
                activity: {
                    user,
                    period: `${days} days`,
                    startDate,
                    endDate: new Date()
                }
            });

        } catch (error) {
            console.error('User activity error:', error);
            res.status(500).json({
                success: false,
                message: 'Internal server error'
            });
        }
    }
}

module.exports = DashboardController;
```

---

## server/models/User.js

```javascript
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

// ES6+ Schema definition with arrow functions
const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: [true, 'Name is required'],
        trim: true,
        maxlength: [50, 'Name cannot exceed 50 characters']
    },
    email: {
        type: String,
        required: [true, 'Email is required'],
        unique: true,
        lowercase: true,
        trim: true,
        match: [/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/, 'Please enter a valid email']
    },
    password: {
        type: String,
        required: function() {
            return this.provider === 'local'; // Only required for local auth
        },
        minlength: [6, 'Password must be at least 6 characters'],
        select: false // Don't include in queries by default
    },
    phone: {
        type: String,
        trim: true,
        match: [/^[\+]?[1-9][\d]{0,15}$/, 'Please enter a valid phone number']
    },
    bio: {
        type: String,
        maxlength: [500, 'Bio cannot exceed 500 characters']
    },
    avatar: {
        type: String,
        default: '/default-avatar.png'
    },
    role: {
        type: String,
        enum: ['user', 'admin', 'moderator'],
        default: 'user'
    },
    provider: {
        type: String,
        enum: ['local', 'google', 'github'],
        default: 'local'
    },
    providerId: {
        type: String,
        sparse: true // Allows multiple null values
    },
    emailVerified: {
        type: Boolean,
        default: false
    },
    emailVerificationToken: String,
    passwordResetToken: String,
    passwordResetExpires: Date,
    lastLogin: {
        type: Date,
        default: Date.now
    },
    loginCount: {
        type: Number,
        default: 0
    },
    isActive: {
        type: Boolean,
        default: true
    },
    twoFactorEnabled: {
        type: Boolean,
        default: false
    },
    twoFactorSecret: String
}, {
    timestamps: true, // Adds createdAt and updatedAt
    toJSON: { 
        transform: (doc, ret) => {
            delete ret.password;
            delete ret.passwordResetToken;
            delete ret.emailVerificationToken;
            delete ret.twoFactorSecret;
            return ret;
        }
    }
});

// ES6+ Index for performance
userSchema.index({ email: 1 });
userSchema.index({ provider: 1, providerId: 1 });

// Pre-save middleware for password hashing
userSchema.pre('save', async function(next) {
    // Only hash the password if it has been modified (or is new)
    if (!this.isModified('password')) return next();
    
    try {
        // Hash password with cost of 12
        const salt = await bcrypt.genSalt(12);
        this.password = await bcrypt.hash(this.password, salt);
        next();
    } catch (error) {
        next(error);
    }
});

// Instance method to check password
userSchema.methods.comparePassword = async function(candidatePassword) {
    return bcrypt.compare(candidatePassword, this.password);
};

// Static method to find by email
userSchema.statics.findByEmail = function(email) {
    return this.findOne({ email });
};

// Virtual for full name (if needed)
userSchema.virtual('fullName').get(function() {
    return this.name;
});

// Export model
module.exports = mongoose.model('User', userSchema);
```

---

## server/models/Session.js

```javascript
const mongoose = require('mongoose');

const sessionSchema = new mongoose.Schema({
    sessionId: {
        type: String,
        required: true,
        unique: true
    },
    userId: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User',
        required: true
    },
    sessionData: {
        type: mongoose.Schema.Types.Mixed,
        default: {}
    },
    userAgent: {
        type: String,
        trim: true
    },
    ipAddress: {
        type: String,
        trim: true
    },
    expiresAt: {
        type: Date,
        required: true,
        default: () => new Date(Date.now() + 24 * 60 * 60 * 1000) // 24 hours
    },
    isActive: {
        type: Boolean,
        default: true
    }
}, {
    timestamps: true
});

// ES6+ Index for automatic expiration
sessionSchema.index({ expiresAt: 1 }, { expireAfterSeconds: 0 });
sessionSchema.index({ userId: 1 });
sessionSchema.index({ sessionId: 1 });

// Static method to cleanup expired sessions
sessionSchema.statics.cleanupExpired = function() {
    return this.deleteMany({ expiresAt: { $lt: new Date() } });
};

module.exports = mongoose.model('Session', sessionSchema);
```

---

## server/models/RefreshToken.js

```javascript
const mongoose = require('mongoose');
const crypto = require('crypto');

const refreshTokenSchema = new mongoose.Schema({
    token: {
        type: String,
        required: true,
        unique: true
    },
    userId: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User',
        required: true
    },
    expiresAt: {
        type: Date,
        required: true,
        default: () => new Date(Date.now() + 7 * 24 * 60 * 60 * 1000) // 7 days
    },
    isRevoked: {
        type: Boolean,
        default: false
    },
    userAgent: String,
    ipAddress: String
}, {
    timestamps: true
});

// ES6+ Index for automatic expiration and performance
refreshTokenSchema.index({ expiresAt: 1 }, { expireAfterSeconds: 0 });
refreshTokenSchema.index({ userId: 1 });
refreshTokenSchema.index({ token: 1 });

// Pre-save middleware to generate token
refreshTokenSchema.pre('save', function(next) {
    if (!this.token) {
        this.token = crypto.randomBytes(64).toString('hex');
    }
    next();
});

// Static method to revoke all tokens for user
refreshTokenSchema.statics.revokeAllForUser = function(userId) {
    return this.updateMany({ userId }, { isRevoked: true });
};

module.exports = mongoose.model('RefreshToken', refreshTokenSchema);
```

---

## server/views/layouts/main.ejs

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= title || 'Full-Stack Web App' %></title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <!-- Custom CSS -->
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            background-color: #f8f9fa;
        }
        .main-content {
            min-height: calc(100vh - 120px);
        }
        .navbar-brand {
            font-weight: 700;
        }
        .card {
            border: none;
            box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
        }
        .btn {
            border-radius: 0.375rem;
        }
        .form-control:focus {
            border-color: #86b7fe;
            box-shadow: 0 0 0 0.2rem rgba(13, 110, 253, 0.25);
        }
        .footer {
            background-color: #343a40;
            color: white;
            padding: 2rem 0;
            margin-top: auto;
        }
    </style>
    
    <!-- Additional head content -->
    <%- include('../partials/head') %>
</head>
<body class="d-flex flex-column min-vh-100">
    <!-- Navigation -->
    <%- include('../partials/navbar') %>
    
    <!-- Flash Messages -->
    <% if (typeof messages !== 'undefined') { %>
        <% Object.keys(messages).forEach(type => { %>
            <% messages[type].forEach(message => { %>
                <div class="alert alert-<%= type === 'error' ? 'danger' : type %> alert-dismissible fade show m-3" role="alert">
                    <%= message %>
                    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
                </div>
            <% }) %>
        <% }) %>
    <% } %>
    
    <!-- Main Content -->
    <main class="main-content flex-grow-1">
        <%- body %>
    </main>
    
    <!-- Footer -->
    <%- include('../partials/footer') %>
    
    <!-- Bootstrap JS Bundle -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    
    <!-- Custom JavaScript -->
    <script>
        // ES6+ JavaScript for enhanced UX
        document.addEventListener('DOMContentLoaded', () => {
            // Auto-hide alerts after 5 seconds
            const alerts = document.querySelectorAll('.alert');
            alerts.forEach(alert => {
                setTimeout(() => {
                    if (alert && alert.parentNode) {
                        alert.classList.remove('show');
                        setTimeout(() => alert.remove(), 150);
                    }
                }, 5000);
            });
            
            // Form validation enhancement
            const forms = document.querySelectorAll('form[data-validate]');
            forms.forEach(form => {
                form.addEventListener('submit', (e) => {
                    if (!form.checkValidity()) {
                        e.preventDefault();
                        e.stopPropagation();
                    }
                    form.classList.add('was-validated');
                });
            });
        });
    </script>
    
    <!-- Additional scripts -->
    <%- include('../partials/scripts') %>
</body>
</html>
```

---

## server/views/layouts/auth.ejs

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= title || 'Authentication - Full-Stack Web App' %></title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <!-- Auth-specific CSS -->
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            padding: 15px;
        }
        .auth-container {
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
        }
        .auth-card {
            background: white;
            border-radius: 15px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        .auth-header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 2rem;
            text-align: center;
        }
        .auth-body {
            padding: 2rem;
        }
        .form-control {
            border-radius: 10px;
            border: 2px solid #e9ecef;
            padding: 12px 15px;
            transition: all 0.3s ease;
        }
        .form-control:focus {
            border-color: #667eea;
            box-shadow: 0 0 0 0.2rem rgba(102, 126, 234, 0.25);
        }
        .btn-auth {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none;
            border-radius: 10px;
            padding: 12px;
            font-weight: 600;
            transition: transform 0.2s ease;
        }
        .btn-auth:hover {
            transform: translateY(-2px);
            background: linear-gradient(135deg, #5a6fd8 0%, #6a4190 100%);
        }
        .btn-google {
            background: #db4437;
            border-color: #db4437;
            border-radius: 10px;
            padding: 12px;
        }
        .btn-google:hover {
            background: #c23321;
            border-color: #c23321;
        }
        .auth-links {
            text-align: center;
            margin-top: 1rem;
        }
        .auth-links a {
            color: #667eea;
            text-decoration: none;
            font-weight: 500;
        }
        .auth-links a:hover {
            color: #5a6fd8;
            text-decoration: underline;
        }
        .divider {
            text-align: center;
            margin: 1.5rem 0;
            position: relative;
        }
        .divider::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 0;
            right: 0;
            height: 1px;
            background: #e9ecef;
        }
        .divider span {
            background: white;
            padding: 0 1rem;
            color: #6c757d;
            font-size: 0.875rem;
        }
    </style>
</head>
<body>
    <div class="auth-container">
        <div class="auth-card">
            <!-- Flash Messages -->
            <% if (typeof messages !== 'undefined') { %>
                <% Object.keys(messages).forEach(type => { %>
                    <% messages[type].forEach(message => { %>
                        <div class="alert alert-<%= type === 'error' ? 'danger' : type %> alert-dismissible fade show m-3" role="alert">
                            <%= message %>
                            <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
                        </div>
                    <% }) %>
                <% }) %>
            <% } %>
            
            <!-- Auth Content -->
            <%- body %>
        </div>
        
        <!-- Back to Home Link -->
        <div class="text-center mt-3">
            <a href="/" class="text-white text-decoration-none">
                <i class="fas fa-arrow-left me-2"></i>Back to Home
            </a>
        </div>
    </div>
    
    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    
    <!-- Auth-specific JavaScript -->
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // Password visibility toggle
            const passwordFields = document.querySelectorAll('input[type="password"]');
            passwordFields.forEach(field => {
                const toggleBtn = document.createElement('button');
                toggleBtn.type = 'button';
                toggleBtn.className = 'btn btn-outline-secondary btn-sm position-absolute';
                toggleBtn.style.right = '10px';
                toggleBtn.style.top = '50%';
                toggleBtn.style.transform = 'translateY(-50%)';
                toggleBtn.innerHTML = '<i class="fas fa-eye"></i>';
                
                const wrapper = document.createElement('div');
                wrapper.className = 'position-relative';
                field.parentNode.insertBefore(wrapper, field);
                wrapper.appendChild(field);
                wrapper.appendChild(toggleBtn);
                
                toggleBtn.addEventListener('click', () => {
                    const isPassword = field.type === 'password';
                    field.type = isPassword ? 'text' : 'password';
                    toggleBtn.innerHTML = isPassword ? '<i class="fas fa-eye-slash"></i>' : '<i class="fas fa-eye"></i>';
                });
            });
            
            // Form submission loading state
            const forms = document.querySelectorAll('form');
            forms.forEach(form => {
                form.addEventListener('submit', (e) => {
                    const submitBtn = form.querySelector('button[type="submit"]');
                    if (submitBtn) {
                        submitBtn.disabled = true;
                        submitBtn.innerHTML = '<i class="fas fa-spinner fa-spin me-2"></i>Loading...';
                    }
                });
            });
        });
    </script>
</body>
</html>
```

---

## server/views/auth/login.ejs

```html
<div class="auth-header">
    <h2 class="mb-0">
        <i class="fas fa-sign-in-alt me-2"></i>
        Welcome Back
    </h2>
    <p class="mb-0 mt-2 opacity-75">Sign in to your account</p>
</div>

<div class="auth-body">
    <form action="/auth/login" method="POST" data-validate>
        <div class="mb-3">
            <label for="email" class="form-label">Email Address</label>
            <input 
                type="email" 
                class="form-control" 
                id="email" 
                name="email" 
                placeholder="Enter your email"
                value="<%= typeof email !== 'undefined' ? email : '' %>"
                required
            >
            <div class="invalid-feedback">
                Please provide a valid email address.
            </div>
        </div>
        
        <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input 
                type="password" 
                class="form-control" 
                id="password" 
                name="password" 
                placeholder="Enter your password"
                required
                minlength="6"
            >
            <div class="invalid-feedback">
                Password must be at least 6 characters long.
            </div>
        </div>
        
        <div class="mb-3 form-check">
            <input type="checkbox" class="form-check-input" id="remember" name="remember">
            <label class="form-check-label" for="remember">
                Remember me
            </label>
        </div>
        
        <button type="submit" class="btn btn-auth text-white w-100">
            <i class="fas fa-sign-in-alt me-2"></i>Sign In
        </button>
    </form>
    
    <div class="divider">
        <span>or</span>
    </div>
    
    <a href="/auth/google" class="btn btn-google text-white w-100">
        <i class="fab fa-google me-2"></i>Continue with Google
    </a>
    
    <div class="auth-links mt-3">
        <div class="row">
            <div class="col-6 text-start">
                <a href="/auth/forgot-password">Forgot Password?</a>
            </div>
            <div class="col-6 text-end">
                <a href="/auth/register">Create Account</a>
            </div>
        </div>
    </div>
</div>
```

---

## server/views/auth/register.ejs

```html
<div class="auth-header">
    <h2 class="mb-0">
        <i class="fas fa-user-plus me-2"></i>
        Create Account
    </h2>
    <p class="mb-0 mt-2 opacity-75">Join us today</p>
</div>

<div class="auth-body">
    <form action="/auth/register" method="POST" data-validate>
        <div class="mb-3">
            <label for="name" class="form-label">Full Name</label>
            <input 
                type="text" 
                class="form-control" 
                id="name" 
                name="name" 
                placeholder="Enter your full name"
                value="<%= typeof name !== 'undefined' ? name : '' %>"
                required
                minlength="2"
                maxlength="50"
            >
            <div class="invalid-feedback">
                Name must be between 2 and 50 characters.
            </div>
        </div>
        
        <div class="mb-3">
            <label for="email" class="form-label">Email Address</label>
            <input 
                type="email" 
                class="form-control" 
                id="email" 
                name="email" 
                placeholder="Enter your email"
                value="<%= typeof email !== 'undefined' ? email : '' %>"
                required
            >
            <div class="invalid-feedback">
                Please provide a valid email address.
            </div>
        </div>
        
        <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input 
                type="password" 
                class="form-control" 
                id="password" 
                name="password" 
                placeholder="Choose a strong password"
                required
                minlength="6"
            >
            <div class="invalid-feedback">
                Password must be at least 6 characters long.
            </div>
            <div class="form-text">
                <small>Use at least 6 characters with a mix of letters and numbers.</small>
            </div>
        </div>
        
        <div class="mb-3">
            <label for="confirmPassword" class="form-label">Confirm Password</label>
            <input 
                type="password" 
                class="form-control" 
                id="confirmPassword" 
                name="confirmPassword" 
                placeholder="Confirm your password"
                required
                minlength="6"
            >
            <div class="invalid-feedback">
                Passwords must match.
            </div>
        </div>
        
        <div class="mb-3 form-check">
            <input type="checkbox" class="form-check-input" id="terms" name="terms" required>
            <label class="form-check-label" for="terms">
                I agree to the <a href="/terms" target="_blank">Terms of Service</a> and 
                <a href="/privacy" target="_blank">Privacy Policy</a>
            </label>
            <div class="invalid-feedback">
                You must agree to the terms and conditions.
            </div>
        </div>
        
        <button type="submit" class="btn btn-auth text-white w-100">
            <i class="fas fa-user-plus me-2"></i>Create Account
        </button>
    </form>
    
    <div class="divider">
        <span>or</span>
    </div>
    
    <a href="/auth/google" class="btn btn-google text-white w-100">
        <i class="fab fa-google me-2"></i>Sign up with Google
    </a>
    
    <div class="auth-links mt-3 text-center">
        <p class="mb-0">Already have an account? <a href="/auth/login">Sign In</a></p>
    </div>
</div>

<script>
    // Password confirmation validation
    document.addEventListener('DOMContentLoaded', () => {
        const password = document.getElementById('password');
        const confirmPassword = document.getElementById('confirmPassword');
        
        const validatePasswords = () => {
            if (password.value !== confirmPassword.value) {
                confirmPassword.setCustomValidity('Passwords do not match');
            } else {
                confirmPassword.setCustomValidity('');
            }
        };
        
        password.addEventListener('input', validatePasswords);
        confirmPassword.addEventListener('input', validatePasswords);
    });
</script>
```

---

## server/views/auth/forgot-password.ejs

```html
<div class="auth-header">
    <h2 class="mb-0">
        <i class="fas fa-key me-2"></i>
        Reset Password
    </h2>
    <p class="mb-0 mt-2 opacity-75">Enter your email to reset your password</p>
</div>

<div class="auth-body">
    <% if (typeof step === 'undefined' || step === 'email') { %>
        <!-- Step 1: Email Input -->
        <form action="/auth/forgot-password" method="POST" data-validate>
            <div class="mb-3">
                <label for="email" class="form-label">Email Address</label>
                <input 
                    type="email" 
                    class="form-control" 
                    id="email" 
                    name="email" 
                    placeholder="Enter your email address"
                    required
                >
                <div class="invalid-feedback">
                    Please provide a valid email address.
                </div>
                <div class="form-text">
                    <small>We'll send you instructions to reset your password.</small>
                </div>
            </div>
            
            <button type="submit" class="btn btn-auth text-white w-100">
                <i class="fas fa-paper-plane me-2"></i>Send Reset Link
            </button>
        </form>
    <% } else if (step === 'sent') { %>
        <!-- Step 2: Email Sent Confirmation -->
        <div class="text-center">
            <div class="mb-4">
                <i class="fas fa-envelope-circle-check text-success" style="font-size: 4rem;"></i>
            </div>
            <h4 class="text-success mb-3">Email Sent!</h4>
            <p class="mb-4">
                We've sent password reset instructions to <strong><%= email %></strong>.
                Please check your email and follow the link to reset your password.
            </p>
            <div class="alert alert-info">
                <i class="fas fa-info-circle me-2"></i>
                If you don't see the email, check your spam folder or 
                <a href="/auth/forgot-password" class="alert-link">try again</a>.
            </div>
        </div>
    <% } else if (step === 'reset') { %>
        <!-- Step 3: New Password Form -->
        <form action="/auth/reset-password" method="POST" data-validate>
            <input type="hidden" name="token" value="<%= token %>">
            
            <div class="mb-3">
                <label for="password" class="form-label">New Password</label>
                <input 
                    type="password" 
                    class="form-control" 
                    id="password" 
                    name="password" 
                    placeholder="Enter new password"
                    required
                    minlength="6"
                >
                <div class="invalid-feedback">
                    Password must be at least 6 characters long.
                </div>
            </div>
            
            <div class="mb-3">
                <label for="confirmPassword" class="form-label">Confirm New Password</label>
                <input 
                    type="password" 
                    class="form-control" 
                    id="confirmPassword" 
                    name="confirmPassword" 
                    placeholder="Confirm new password"
                    required
                    minlength="6"
                >
                <div class="invalid-feedback">
                    Passwords must match.
                </div>
            </div>
            
            <button type="submit" class="btn btn-auth text-white w-100">
                <i class="fas fa-lock me-2"></i>Update Password
            </button>
        </form>
    <% } %>
    
    <div class="auth-links mt-3 text-center">
        <a href="/auth/login">
            <i class="fas fa-arrow-left me-1"></i>Back to Login
        </a>
    </div>
</div>
```

## server/views/dashboard/index.ejs

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard - FullStack App</title>
    <!-- Bootstrap CSS for responsive design -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <!-- Custom CSS -->
    <link rel="stylesheet" href="/css/dashboard.css">
</head>
<body>
    <!-- Include header partial -->
    <%- include('../partials/header') %>
    
    <!-- Main dashboard content -->
    <div class="container-fluid">
        <div class="row">
            <!-- Sidebar -->
            <nav class="col-md-3 col-lg-2 d-md-block bg-light sidebar">
                <div class="position-sticky pt-3">
                    <ul class="nav flex-column">
                        <li class="nav-item">
                            <a class="nav-link active" href="/dashboard">
                                <i class="fas fa-tachometer-alt me-2"></i>
                                Dashboard
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="/dashboard/profile">
                                <i class="fas fa-user me-2"></i>
                                Profile
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="/settings">
                                <i class="fas fa-cog me-2"></i>
                                Settings
                            </a>
                        </li>
                    </ul>
                </div>
            </nav>

            <!-- Main content -->
            <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
                <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
                    <h1 class="h2">Dashboard</h1>
                    <div class="btn-toolbar mb-2 mb-md-0">
                        <div class="btn-group me-2">
                            <button type="button" class="btn btn-sm btn-outline-secondary">Share</button>
                            <button type="button" class="btn btn-sm btn-outline-secondary">Export</button>
                        </div>
                    </div>
                </div>

                <!-- Welcome message with user data -->
                <div class="row mb-4">
                    <div class="col-12">
                        <div class="alert alert-success" role="alert">
                            <h4 class="alert-heading">Welcome back, <%= user.name %>!</h4>
                            <p>Last login: <%= new Date(user.lastLogin).toLocaleString() %></p>
                            <hr>
                            <p class="mb-0">You have successfully logged in to your dashboard.</p>
                        </div>
                    </div>
                </div>

                <!-- Statistics cards -->
                <div class="row mb-4">
                    <div class="col-xl-3 col-md-6 mb-4">
                        <div class="card border-left-primary shadow h-100 py-2">
                            <div class="card-body">
                                <div class="row no-gutters align-items-center">
                                    <div class="col mr-2">
                                        <div class="text-xs font-weight-bold text-primary text-uppercase mb-1">
                                            Total Users
                                        </div>
                                        <div class="h5 mb-0 font-weight-bold text-gray-800">
                                            <%= stats.totalUsers || 0 %>
                                        </div>
                                    </div>
                                    <div class="col-auto">
                                        <i class="fas fa-users fa-2x text-gray-300"></i>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="col-xl-3 col-md-6 mb-4">
                        <div class="card border-left-success shadow h-100 py-2">
                            <div class="card-body">
                                <div class="row no-gutters align-items-center">
                                    <div class="col mr-2">
                                        <div class="text-xs font-weight-bold text-success text-uppercase mb-1">
                                            Active Sessions
                                        </div>
                                        <div class="h5 mb-0 font-weight-bold text-gray-800">
                                            <%= stats.activeSessions || 0 %>
                                        </div>
                                    </div>
                                    <div class="col-auto">
                                        <i class="fas fa-clock fa-2x text-gray-300"></i>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="col-xl-3 col-md-6 mb-4">
                        <div class="card border-left-info shadow h-100 py-2">
                            <div class="card-body">
                                <div class="row no-gutters align-items-center">
                                    <div class="col mr-2">
                                        <div class="text-xs font-weight-bold text-info text-uppercase mb-1">
                                            Login Count
                                        </div>
                                        <div class="h5 mb-0 font-weight-bold text-gray-800">
                                            <%= user.loginCount || 0 %>
                                        </div>
                                    </div>
                                    <div class="col-auto">
                                        <i class="fas fa-sign-in-alt fa-2x text-gray-300"></i>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="col-xl-3 col-md-6 mb-4">
                        <div class="card border-left-warning shadow h-100 py-2">
                            <div class="card-body">
                                <div class="row no-gutters align-items-center">
                                    <div class="col mr-2">
                                        <div class="text-xs font-weight-bold text-warning text-uppercase mb-1">
                                            Account Status
                                        </div>
                                        <div class="h5 mb-0 font-weight-bold text-gray-800">
                                            <%= user.status || 'Active' %>
                                        </div>
                                    </div>
                                    <div class="col-auto">
                                        <i class="fas fa-check-circle fa-2x text-gray-300"></i>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Recent activity -->
                <div class="row">
                    <div class="col-lg-8">
                        <div class="card">
                            <div class="card-header">
                                <h6 class="m-0 font-weight-bold text-primary">Recent Activity</h6>
                            </div>
                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-bordered" width="100%" cellspacing="0">
                                        <thead>
                                            <tr>
                                                <th>Date</th>
                                                <th>Activity</th>
                                                <th>Status</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <% if (activities && activities.length > 0) { %>
                                                <% activities.forEach(activity => { %>
                                                    <tr>
                                                        <td><%= new Date(activity.date).toLocaleDateString() %></td>
                                                        <td><%= activity.description %></td>
                                                        <td>
                                                            <span class="badge bg-<%= activity.status === 'success' ? 'success' : 'secondary' %>">
                                                                <%= activity.status %>
                                                            </span>
                                                        </td>
                                                    </tr>
                                                <% }) %>
                                            <% } else { %>
                                                <tr>
                                                    <td colspan="3" class="text-center">No recent activity</td>
                                                </tr>
                                            <% } %>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="col-lg-4">
                        <div class="card">
                            <div class="card-header">
                                <h6 class="m-0 font-weight-bold text-primary">Quick Actions</h6>
                            </div>
                            <div class="card-body">
                                <div class="d-grid gap-2">
                                    <a href="/dashboard/profile" class="btn btn-primary">
                                        <i class="fas fa-user me-2"></i>Edit Profile
                                    </a>
                                    <a href="/settings" class="btn btn-secondary">
                                        <i class="fas fa-cog me-2"></i>Settings
                                    </a>
                                    <a href="/support" class="btn btn-info">
                                        <i class="fas fa-question-circle me-2"></i>Get Support
                                    </a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- Include footer partial -->
    <%- include('../partials/footer') %>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    <!-- Custom JS -->
    <script src="/js/dashboard.js"></script>
</body>
</html>
```

---

## server/views/dashboard/profile.ejs

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Profile - FullStack App</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <!-- Custom CSS -->
    <link rel="stylesheet" href="/css/profile.css">
</head>
<body>
    <!-- Include header partial -->
    <%- include('../partials/header') %>
    
    <div class="container-fluid">
        <div class="row">
            <!-- Sidebar -->
            <nav class="col-md-3 col-lg-2 d-md-block bg-light sidebar">
                <div class="position-sticky pt-3">
                    <ul class="nav flex-column">
                        <li class="nav-item">
                            <a class="nav-link" href="/dashboard">
                                <i class="fas fa-tachometer-alt me-2"></i>
                                Dashboard
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link active" href="/dashboard/profile">
                                <i class="fas fa-user me-2"></i>
                                Profile
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="/settings">
                                <i class="fas fa-cog me-2"></i>
                                Settings
                            </a>
                        </li>
                    </ul>
                </div>
            </nav>

            <!-- Main content -->
            <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
                <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
                    <h1 class="h2">User Profile</h1>
                    <div class="btn-toolbar mb-2 mb-md-0">
                        <button type="button" class="btn btn-sm btn-outline-secondary" onclick="enableEdit()">
                            <i class="fas fa-edit me-1"></i>Edit Profile
                        </button>
                    </div>
                </div>

                <!-- Profile form -->
                <div class="row">
                    <div class="col-lg-8">
                        <div class="card">
                            <div class="card-header">
                                <h5>Personal Information</h5>
                            </div>
                            <div class="card-body">
                                <!-- Success/Error messages -->
                                <% if (typeof success !== 'undefined' && success) { %>
                                    <div class="alert alert-success alert-dismissible fade show" role="alert">
                                        Profile updated successfully!
                                        <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
                                    </div>
                                <% } %>
                                
                                <% if (typeof error !== 'undefined' && error) { %>
                                    <div class="alert alert-danger alert-dismissible fade show" role="alert">
                                        <%= error %>
                                        <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
                                    </div>
                                <% } %>

                                <form id="profileForm" method="POST" action="/dashboard/profile">
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="name" class="form-label">Full Name *</label>
                                                <input 
                                                    type="text" 
                                                    class="form-control" 
                                                    id="name" 
                                                    name="name" 
                                                    value="<%= user.name %>" 
                                                    readonly 
                                                    required
                                                >
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="email" class="form-label">Email Address *</label>
                                                <input 
                                                    type="email" 
                                                    class="form-control" 
                                                    id="email" 
                                                    name="email" 
                                                    value="<%= user.email %>" 
                                                    readonly 
                                                    required
                                                >
                                            </div>
                                        </div>
                                    </div>

                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="phone" class="form-label">Phone Number</label>
                                                <input 
                                                    type="tel" 
                                                    class="form-control" 
                                                    id="phone" 
                                                    name="phone" 
                                                    value="<%= user.phone || '' %>" 
                                                    readonly
                                                >
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="dateOfBirth" class="form-label">Date of Birth</label>
                                                <input 
                                                    type="date" 
                                                    class="form-control" 
                                                    id="dateOfBirth" 
                                                    name="dateOfBirth" 
                                                    value="<%= user.dateOfBirth ? user.dateOfBirth.toISOString().split('T')[0] : '' %>" 
                                                    readonly
                                                >
                                            </div>
                                        </div>
                                    </div>

                                    <div class="mb-3">
                                        <label for="address" class="form-label">Address</label>
                                        <textarea 
                                            class="form-control" 
                                            id="address" 
                                            name="address" 
                                            rows="3" 
                                            readonly
                                        ><%= user.address || '' %></textarea>
                                    </div>

                                    <div class="mb-3">
                                        <label for="bio" class="form-label">Bio</label>
                                        <textarea 
                                            class="form-control" 
                                            id="bio" 
                                            name="bio" 
                                            rows="4" 
                                            placeholder="Tell us about yourself..." 
                                            readonly
                                        ><%= user.bio || '' %></textarea>
                                    </div>

                                    <!-- Hidden buttons shown when editing -->
                                    <div id="editButtons" style="display: none;">
                                        <div class="d-flex gap-2">
                                            <button type="submit" class="btn btn-primary">
                                                <i class="fas fa-save me-1"></i>Save Changes
                                            </button>
                                            <button type="button" class="btn btn-secondary" onclick="cancelEdit()">
                                                <i class="fas fa-times me-1"></i>Cancel
                                            </button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        </div>

                        <!-- Change Password Section -->
                        <div class="card mt-4">
                            <div class="card-header">
                                <h5>Change Password</h5>
                            </div>
                            <div class="card-body">
                                <form method="POST" action="/dashboard/change-password">
                                    <div class="mb-3">
                                        <label for="currentPassword" class="form-label">Current Password *</label>
                                        <input 
                                            type="password" 
                                            class="form-control" 
                                            id="currentPassword" 
                                            name="currentPassword" 
                                            required
                                        >
                                    </div>
                                    
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="newPassword" class="form-label">New Password *</label>
                                                <input 
                                                    type="password" 
                                                    class="form-control" 
                                                    id="newPassword" 
                                                    name="newPassword" 
                                                    minlength="6"
                                                    required
                                                >
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="confirmPassword" class="form-label">Confirm New Password *</label>
                                                <input 
                                                    type="password" 
                                                    class="form-control" 
                                                    id="confirmPassword" 
                                                    name="confirmPassword" 
                                                    required
                                                >
                                            </div>
                                        </div>
                                    </div>
                                    
                                    <button type="submit" class="btn btn-warning">
                                        <i class="fas fa-key me-1"></i>Change Password
                                    </button>
                                </form>
                            </div>
                        </div>
                    </div>

                    <!-- Profile sidebar -->
                    <div class="col-lg-4">
                        <div class="card">
                            <div class="card-header">
                                <h5>Profile Summary</h5>
                            </div>
                            <div class="card-body text-center">
                                <div class="mb-3">
                                    <img 
                                        src="<%= user.avatar || '/images/default-avatar.png' %>" 
                                        alt="Profile Picture" 
                                        class="rounded-circle" 
                                        width="100" 
                                        height="100"
                                        style="object-fit: cover;"
                                    >
                                </div>
                                <h5><%= user.name %></h5>
                                <p class="text-muted"><%= user.email %></p>
                                <div class="row text-center">
                                    <div class="col-6">
                                        <strong>Member Since</strong><br>
                                        <span class="text-muted">
                                            <%= new Date(user.createdAt).toLocaleDateString() %>
                                        </span>
                                    </div>
                                    <div class="col-6">
                                        <strong>Last Login</strong><br>
                                        <span class="text-muted">
                                            <%= user.lastLogin ? new Date(user.lastLogin).toLocaleDateString() : 'Never' %>
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Account Settings -->
                        <div class="card mt-3">
                            <div class="card-header">
                                <h5>Account Settings</h5>
                            </div>
                            <div class="card-body">
                                <div class="list-group list-group-flush">
                                    <div class="list-group-item d-flex justify-content-between align-items-center">
                                        Email Notifications
                                        <div class="form-check form-switch">
                                            <input 
                                                class="form-check-input" 
                                                type="checkbox" 
                                                id="emailNotifications"
                                                <%= user.emailNotifications ? 'checked' : '' %>
                                            >
                                        </div>
                                    </div>
                                    <div class="list-group-item d-flex justify-content-between align-items-center">
                                        SMS Notifications
                                        <div class="form-check form-switch">
                                            <input 
                                                class="form-check-input" 
                                                type="checkbox" 
                                                id="smsNotifications"
                                                <%= user.smsNotifications ? 'checked' : '' %>
                                            >
                                        </div>
                                    </div>
                                    <div class="list-group-item d-flex justify-content-between align-items-center">
                                        Two-Factor Auth
                                        <span class="badge bg-<%= user.twoFactorEnabled ? 'success' : 'secondary' %>">
                                            <%= user.twoFactorEnabled ? 'Enabled' : 'Disabled' %>
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- Include footer partial -->
    <%- include('../partials/footer') %>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    
    <!-- Custom JavaScript for profile editing -->
    <script>
        function enableEdit() {
            // Enable all form inputs
            const inputs = document.querySelectorAll('#profileForm input, #profileForm textarea');
            inputs.forEach(input => {
                input.removeAttribute('readonly');
            });
            
            // Show edit buttons
            document.getElementById('editButtons').style.display = 'block';
            
            // Hide edit button
            document.querySelector('.btn-outline-secondary').style.display = 'none';
        }
        
        function cancelEdit() {
            // Disable all form inputs
            const inputs = document.querySelectorAll('#profileForm input, #profileForm textarea');
            inputs.forEach(input => {
                input.setAttribute('readonly', true);
            });
            
            // Hide edit buttons
            document.getElementById('editButtons').style.display = 'none';
            
            // Show edit button
            document.querySelector('.btn-outline-secondary').style.display = 'inline-block';
            
            // Reset form to original values
            document.getElementById('profileForm').reset();
        }
        
        // Password confirmation validation
        document.getElementById('confirmPassword').addEventListener('input', function() {
            const password = document.getElementById('newPassword').value;
            const confirm = this.value;
            
            if (password !== confirm) {
                this.setCustomValidity('Passwords do not match');
            } else {
                this.setCustomValidity('');
            }
        });
    </script>
</body>
</html>
```

## server/views/partials/header.ejs

```html
<!-- Header partial for EJS templates -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= title || 'Full-Stack Web App' %></title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <!-- Custom CSS -->
    <style>
        body {
            font-family: 'Roboto', sans-serif;
        }
        .navbar-brand {
            font-weight: 700;
        }
        .footer {
            margin-top: auto;
        }
        .alert {
            border-radius: 0.5rem;
        }
    </style>
</head>
<body class="d-flex flex-column min-vh-100">
```

---

## server/views/partials/footer.ejs

```html
<!-- Footer partial for EJS templates -->
    <footer class="footer bg-dark text-light mt-auto py-4">
        <div class="container">
            <div class="row">
                <div class="col-md-6">
                    <h5>FullStack Web App</h5>
                    <p class="mb-0">
                        Built with Node.js, Express, MongoDB, MySQL, and modern web technologies.
                    </p>
                </div>
                
                <div class="col-md-3">
                    <h6>Quick Links</h6>
                    <ul class="list-unstyled">
                        <li><a href="/" class="text-light text-decoration-none">Home</a></li>
                        <li><a href="/about" class="text-light text-decoration-none">About</a></li>
                        <li><a href="/contact" class="text-light text-decoration-none">Contact</a></li>
                        <% if (locals.user) { %>
                            <li><a href="/dashboard" class="text-light text-decoration-none">Dashboard</a></li>
                        <% } %>
                    </ul>
                </div>
                
                <div class="col-md-3">
                    <h6>Technologies</h6>
                    <ul class="list-unstyled small">
                        <li>Node.js & Express</li>
                        <li>MongoDB & MySQL</li>
                        <li>JWT & OAuth</li>
                        <li>React & Bootstrap</li>
                    </ul>
                </div>
            </div>
            
            <hr class="my-3">
            
            <div class="row align-items-center">
                <div class="col-md-6">
                    <p class="mb-0">
                        &copy; <%= new Date().getFullYear() %> FullStack Web App. All rights reserved.
                    </p>
                </div>
                <div class="col-md-6 text-md-end">
                    <small class="text-muted">
                        Server-side rendered with EJS
                    </small>
                </div>
            </div>
        </div>
    </footer>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    
    <!-- Custom JavaScript -->
    <script>
        // Flash message auto-hide
        document.addEventListener('DOMContentLoaded', function() {
            const alerts = document.querySelectorAll('.alert-dismissible');
            alerts.forEach(alert => {
                setTimeout(() => {
                    if (alert) {
                        alert.style.transition = 'opacity 0.5s';
                        alert.style.opacity = '0';
                        setTimeout(() => alert.remove(), 500);
                    }
                }, 5000); // Auto-hide after 5 seconds
            });
        });
    </script>
</body>
</html>
```

---

## server/views/partials/navbar.ejs

```html
<!-- Navbar partial for EJS templates -->
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
        <!-- Brand -->
        <a class="navbar-brand" href="/">
            <i class="fas fa-code me-2"></i>
            FullStack App
        </a>
        
        <!-- Mobile toggle button -->
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        
        <!-- Navigation items -->
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav me-auto">
                <li class="nav-item">
                    <a class="nav-link <%= (typeof page !== 'undefined' && page === 'home') ? 'active' : '' %>" href="/">
                        <i class="fas fa-home me-1"></i>Home
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link <%= (typeof page !== 'undefined' && page === 'about') ? 'active' : '' %>" href="/about">
                        <i class="fas fa-info-circle me-1"></i>About
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link <%= (typeof page !== 'undefined' && page === 'contact') ? 'active' : '' %>" href="/contact">
                        <i class="fas fa-envelope me-1"></i>Contact
                    </a>
                </li>
                
                <!-- Authenticated user links -->
                <% if (locals.user) { %>
                    <li class="nav-item">
                        <a class="nav-link <%= (typeof page !== 'undefined' && page === 'dashboard') ? 'active' : '' %>" href="/dashboard">
                            <i class="fas fa-tachometer-alt me-1"></i>Dashboard
                        </a>
                    </li>
                    
                    <!-- Admin-only links -->
                    <% if (locals.user.role === 'admin') { %>
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="adminDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                                <i class="fas fa-cog me-1"></i>Admin
                            </a>
                            <ul class="dropdown-menu" aria-labelledby="adminDropdown">
                                <li><a class="dropdown-item" href="/admin/users"><i class="fas fa-users me-2"></i>Manage Users</a></li>
                                <li><a class="dropdown-item" href="/admin/settings"><i class="fas fa-settings me-2"></i>Settings</a></li>
                                <li><hr class="dropdown-divider"></li>
                                <li><a class="dropdown-item" href="/admin/logs"><i class="fas fa-file-alt me-2"></i>System Logs</a></li>
                            </ul>
                        </li>
                    <% } %>
                <% } %>
            </ul>
            
            <!-- Authentication buttons -->
            <div class="d-flex align-items-center">
                <% if (locals.user) { %>
                    <!-- Logged in user -->
                    <div class="dropdown">
                        <button class="btn btn-outline-light dropdown-toggle" type="button" id="userDropdown" data-bs-toggle="dropdown" aria-expanded="false">
                            <i class="fas fa-user me-1"></i>
                            <%= user.name %>
                        </button>
                        <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="userDropdown">
                            <li>
                                <a class="dropdown-item" href="/profile">
                                    <i class="fas fa-user-circle me-2"></i>Profile
                                </a>
                            </li>
                            <li>
                                <a class="dropdown-item" href="/settings">
                                    <i class="fas fa-cog me-2"></i>Settings
                                </a>
                            </li>
                            <li><hr class="dropdown-divider"></li>
                            <li>
                                <form action="/auth/logout" method="POST" class="d-inline">
                                    <button type="submit" class="dropdown-item text-danger">
                                        <i class="fas fa-sign-out-alt me-2"></i>Logout
                                    </button>
                                </form>
                            </li>
                        </ul>
                    </div>
                <% } else { %>
                    <!-- Not logged in -->
                    <div class="d-flex gap-2">
                        <a href="/auth/login" class="btn btn-outline-light btn-sm">
                            <i class="fas fa-sign-in-alt me-1"></i>Login
                        </a>
                        <a href="/auth/register" class="btn btn-primary btn-sm">
                            <i class="fas fa-user-plus me-1"></i>Register
                        </a>
                    </div>
                <% } %>
            </div>
        </div>
    </div>
</nav>

<!-- Flash messages -->
<% if (locals.messages) { %>
    <div class="container mt-3">
        <!-- Success messages -->
        <% if (messages.success) { %>
            <% messages.success.forEach(function(message) { %>
                <div class="alert alert-success alert-dismissible fade show" role="alert">
                    <i class="fas fa-check-circle me-2"></i>
                    <%= message %>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                </div>
            <% }); %>
        <% } %>
        
        <!-- Error messages -->
        <% if (messages.error) { %>
            <% messages.error.forEach(function(message) { %>
                <div class="alert alert-danger alert-dismissible fade show" role="alert">
                    <i class="fas fa-exclamation-triangle me-2"></i>
                    <%= message %>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                </div>
            <% }); %>
        <% } %>
        
        <!-- Warning messages -->
        <% if (messages.warning) { %>
            <% messages.warning.forEach(function(message) { %>
                <div class="alert alert-warning alert-dismissible fade show" role="alert">
                    <i class="fas fa-exclamation-circle me-2"></i>
                    <%= message %>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                </div>
            <% }); %>
        <% } %>
        
        <!-- Info messages -->
        <% if (messages.info) { %>
            <% messages.info.forEach(function(message) { %>
                <div class="alert alert-info alert-dismissible fade show" role="alert">
                    <i class="fas fa-info-circle me-2"></i>
                    <%= message %>
                    <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                </div>
            <% }); %>
        <% } %>
    </div>
<% } %>
```

---

## server/views/errors/404.ejs

```html
<!-- 404 Error Page -->
<%- include('../partials/header', { title: '404 - Page Not Found' }) %>
<%- include('../partials/navbar', { page: '404' }) %>

<div class="container my-5">
    <div class="row justify-content-center">
        <div class="col-md-8 text-center">
            <!-- Error illustration -->
            <div class="mb-4">
                <i class="fas fa-exclamation-triangle text-warning" style="font-size: 8rem;"></i>
            </div>
            
            <!-- Error message -->
            <h1 class="display-1 fw-bold text-primary">404</h1>
            <h2 class="mb-3">Oops! Page Not Found</h2>
            <p class="lead mb-4">
                The page you're looking for doesn't exist. It might have been moved, deleted, or you entered the wrong URL.
            </p>
            
            <!-- Helpful information -->
            <div class="row text-start mb-4">
                <div class="col-md-6">
                    <h5><i class="fas fa-lightbulb text-warning me-2"></i>What you can do:</h5>
                    <ul>
                        <li>Check the URL for typing errors</li>
                        <li>Go back to the previous page</li>
                        <li>Visit our homepage</li>
                        <li>Use the search feature</li>
                    </ul>
                </div>
                
                <div class="col-md-6">
                    <h5><i class="fas fa-link text-info me-2"></i>Quick Links:</h5>
                    <ul>
                        <li><a href="/" class="text-decoration-none">Homepage</a></li>
                        <li><a href="/about" class="text-decoration-none">About Us</a></li>
                        <li><a href="/contact" class="text-decoration-none">Contact</a></li>
                        <% if (locals.user) { %>
                            <li><a href="/dashboard" class="text-decoration-none">Dashboard</a></li>
                        <% } else { %>
                            <li><a href="/auth/login" class="text-decoration-none">Login</a></li>
                        <% } %>
                    </ul>
                </div>
            </div>
            
            <!-- Action buttons -->
            <div class="d-flex justify-content-center gap-3 flex-wrap">
                <a href="/" class="btn btn-primary btn-lg">
                    <i class="fas fa-home me-2"></i>Go Home
                </a>
                
                <button onclick="history.back()" class="btn btn-outline-secondary btn-lg">
                    <i class="fas fa-arrow-left me-2"></i>Go Back
                </button>
                
                <a href="/contact" class="btn btn-outline-info btn-lg">
                    <i class="fas fa-envelope me-2"></i>Contact Support
                </a>
            </div>
            
            <!-- Search box -->
            <div class="mt-5">
                <form action="/search" method="GET" class="d-flex justify-content-center">
                    <div class="input-group" style="max-width: 400px;">
                        <input 
                            type="text" 
                            class="form-control" 
                            placeholder="Search our website..." 
                            name="q"
                            required
                        >
                        <button class="btn btn-outline-primary" type="submit">
                            <i class="fas fa-search"></i>
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
    
    <!-- Additional info section -->
    <div class="row mt-5">
        <div class="col-12">
            <div class="card border-0 bg-light">
                <div class="card-body text-center">
                    <h5 class="card-title">Need Help?</h5>
                    <p class="card-text">
                        If you believe this is an error or need assistance, please don't hesitate to reach out to our support team.
                    </p>
                    <small class="text-muted">
                        Error Code: 404 | Timestamp: <%= new Date().toISOString() %>
                        <% if (locals.requestUrl) { %>
                            | Requested URL: <%= requestUrl %>
                        <% } %>
                    </small>
                </div>
            </div>
        </div>
    </div>
</div>

<%- include('../partials/footer') %>
```

---

## server/views/errors/500.ejs

```html
<!-- 500 Internal Server Error Page -->
<%- include('../partials/header', { title: '500 - Server Error' }) %>
<%- include('../partials/navbar', { page: '500' }) %>

<div class="container my-5">
    <div class="row justify-content-center">
        <div class="col-md-8 text-center">
            <!-- Error illustration -->
            <div class="mb-4">
                <i class="fas fa-server text-danger" style="font-size: 8rem;"></i>
            </div>
            
            <!-- Error message -->
            <h1 class="display-1 fw-bold text-danger">500</h1>
            <h2 class="mb-3">Internal Server Error</h2>
            <p class="lead mb-4">
                Something went wrong on our end. We're working to fix this issue as quickly as possible.
            </p>
            
            <!-- Error details (only in development) -->
            <% if (typeof error !== 'undefined' && process.env.NODE_ENV === 'development') { %>
                <div class="alert alert-danger text-start mb-4">
                    <h5><i class="fas fa-bug me-2"></i>Debug Information:</h5>
                    <pre class="mb-0"><%= error.stack || error.message || error %></pre>
                </div>
            <% } %>
            
            <!-- Helpful information -->
            <div class="row text-start mb-4">
                <div class="col-md-6">
                    <h5><i class="fas fa-info-circle text-info me-2"></i>What happened?</h5>
                    <ul>
                        <li>A server error occurred</li>
                        <li>The request couldn't be processed</li>
                        <li>Our team has been notified</li>
                        <li>We're working on a fix</li>
                    </ul>
                </div>
                
                <div class="col-md-6">
                    <h5><i class="fas fa-tools text-warning me-2"></i>What you can do:</h5>
                    <ul>
                        <li>Refresh the page in a few minutes</li>
                        <li>Try again later</li>
                        <li>Contact support if issue persists</li>
                        <li>Check our status page</li>
                    </ul>
                </div>
            </div>
            
            <!-- Action buttons -->
            <div class="d-flex justify-content-center gap-3 flex-wrap">
                <button onclick="location.reload()" class="btn btn-primary btn-lg">
                    <i class="fas fa-redo me-2"></i>Try Again
                </button>
                
                <a href="/" class="btn btn-outline-primary btn-lg">
                    <i class="fas fa-home me-2"></i>Go Home
                </a>
                
                <button onclick="history.back()" class="btn btn-outline-secondary btn-lg">
                    <i class="fas fa-arrow-left me-2"></i>Go Back
                </button>
                
                <a href="/contact" class="btn btn-outline-danger btn-lg">
                    <i class="fas fa-life-ring me-2"></i>Get Help
                </a>
            </div>
            
            <!-- Status check -->
            <div class="mt-5">
                <div class="card border-warning">
                    <div class="card-body">
                        <h5 class="card-title">
                            <i class="fas fa-heartbeat text-warning me-2"></i>
                            System Status
                        </h5>
                        <div class="d-flex justify-content-center align-items-center">
                            <div class="spinner-border spinner-border-sm text-warning me-2" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <span>Checking system status...</span>
                        </div>
                        <div class="mt-3">
                            <a href="/status" class="btn btn-sm btn-outline-warning">
                                View Status Page
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Error reporting -->
    <div class="row mt-5">
        <div class="col-12">
            <div class="card border-0 bg-light">
                <div class="card-body">
                    <h5 class="card-title text-center">Report This Error</h5>
                    <p class="text-center mb-3">
                        Help us improve by reporting this error. Your feedback is valuable to us.
                    </p>
                    
                    <form action="/report-error" method="POST" class="row g-3">
                        <div class="col-md-6">
                            <input type="email" class="form-control" placeholder="Your email (optional)" name="email">
                        </div>
                        <div class="col-md-6">
                            <input type="text" class="form-control" placeholder="What were you trying to do?" name="action" required>
                        </div>
                        <div class="col-12">
                            <textarea class="form-control" rows="3" placeholder="Additional details..." name="description"></textarea>
                        </div>
                        <div class="col-12 text-center">
                            <button type="submit" class="btn btn-warning">
                                <i class="fas fa-paper-plane me-2"></i>Send Report
                            </button>
                        </div>
                        
                        <!-- Hidden fields for error tracking -->
                        <input type="hidden" name="errorId" value="<%= new Date().getTime() %>">
                        <input type="hidden" name="timestamp" value="<%= new Date().toISOString() %>">
                        <% if (locals.requestUrl) { %>
                            <input type="hidden" name="url" value="<%= requestUrl %>">
                        <% } %>
                    </form>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Technical details -->
    <div class="row mt-3">
        <div class="col-12">
            <small class="text-muted d-block text-center">
                Error ID: ERR-<%= new Date().getTime() %> | 
                Timestamp: <%= new Date().toISOString() %>
                <% if (locals.requestUrl) { %>
                    | URL: <%= requestUrl %>
                <% } %>
            </small>
        </div>
    </div>
</div>

<!-- Auto-refresh script -->
<script>
    // Auto-refresh page after 30 seconds (optional)
    let autoRefresh = false;
    
    if (autoRefresh) {
        setTimeout(() => {
            if (confirm('Would you like to try refreshing the page?')) {
                location.reload();
            }
        }, 30000);
    }
    
    // Status check functionality
    document.addEventListener('DOMContentLoaded', function() {
        // Simulate status check
        setTimeout(() => {
            const statusElement = document.querySelector('.spinner-border').parentElement;
            statusElement.innerHTML = '<i class="fas fa-exclamation-triangle text-warning me-2"></i>System experiencing issues';
        }, 2000);
    });
</script>

<%- include('../partials/footer') %>
```


---

## server/routes/authRoutes.js

```javascript
const express = require('express');
const passport = require('passport');
const rateLimit = require('express-rate-limit');
const { body } = require('express-validator');
const AuthController = require('../controllers/authController');
const auth = require('../middleware/auth');

const router = express.Router();

// Rate limiting for auth routes
const authLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 5, // 5 attempts per window
    message: {
        success: false,
        message: 'Too many authentication attempts, please try again later'
    },
    standardHeaders: true,
    legacyHeaders: false
});

// Validation middleware using ES6+ array methods
const registerValidation = [
    body('name')
        .trim()
        .isLength({ min: 2, max: 50 })
        .withMessage('Name must be between 2 and 50 characters'),
    body('email')
        .isEmail()
        .normalizeEmail()
        .withMessage('Please provide a valid email'),
    body('password')
        .isLength({ min: 6 })
        .withMessage('Password must be at least 6 characters')
        .matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)
        .withMessage('Password must contain at least one uppercase letter, one lowercase letter, and one number')
];

const loginValidation = [
    body('email')
        .isEmail()
        .normalizeEmail()
        .withMessage('Please provide a valid email'),
    body('password')
        .notEmpty()
        .withMessage('Password is required')
];

// Auth routes with ES6+ route handlers
router.post('/register', authLimiter, registerValidation, AuthController.register);
router.post('/login', authLimiter, loginValidation, AuthController.login);
router.post('/logout', auth, AuthController.logout);
router.post('/refresh-token', AuthController.refreshToken);

// OAuth routes
router.get('/google', 
    passport.authenticate('google', { scope: ['profile', 'email'] })
);

router.get('/google/callback',
    passport.authenticate('google', { failureRedirect: '/login' }),
    AuthController.googleCallback
);

// Protected routes
router.get('/profile', auth, AuthController.getProfile);

// Verify token route
router.get('/verify', auth, (req, res) => {
    res.json({
        success: true,
        user: req.user,
        message: 'Token is valid'
    });
});

module.exports = router;
```

---

## server/routes/userRoutes.js

```javascript
const express = require('express');
const { body } = require('express-validator');
const UserController = require('../controllers/userController');
const auth = require('../middleware/auth');
const { requireRole } = require('../middleware/auth');

const router = express.Router();

// User profile validation
const profileValidation = [
    body('name')
        .optional()
        .trim()
        .isLength({ min: 2, max: 50 })
        .withMessage('Name must be between 2 and 50 characters'),
    body('phone')
        .optional()
        .isMobilePhone()
        .withMessage('Please provide a valid phone number'),
    body('bio')
        .optional()
        .isLength({ max: 500 })
        .withMessage('Bio cannot exceed 500 characters')
];

// Protected user routes
router.use(auth); // Apply auth middleware to all routes

// User profile routes
router.get('/profile', UserController.getUserById);
router.put('/profile', profileValidation, UserController.updateProfile);
router.get('/stats', UserController.getUserStats);

// Admin only routes
router.get('/', requireRole('admin'), UserController.getAllUsers);
router.get('/:id', requireRole('admin'), UserController.getUserById);
router.delete('/:id', requireRole('admin'), UserController.deleteUser);

module.exports = router;
```

---

## server/routes/apiRoutes.js

```javascript
const express = require('express');
const DashboardController = require('../controllers/dashboardController');
const auth = require('../middleware/auth');
const rateLimit = require('express-rate-limit');

const router = express.Router();

// API rate limiting
const apiLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // 100 requests per window
    message: {
        success: false,
        message: 'Too many API requests, please try again later'
    }
});

// Apply middleware to all API routes
router.use(apiLimiter);
router.use(auth);

// Dashboard API routes
router.get('/dashboard/stats', DashboardController.getDashboardStats);
router.get('/dashboard/activity', DashboardController.getUserActivity);

// Health check endpoint
router.get('/health', (req, res) => {
    res.json({
        success: true,
        message: 'API is healthy',
        timestamp: new Date().toISOString(),
        uptime: process.uptime()
    });
});

module.exports = router;
```

---

## server/routes/viewRoutes.js

```javascript
const express = require('express');
const auth = require('../middleware/auth');

const router = express.Router();

// Middleware to set common view variables
router.use((req, res, next) => {
    res.locals.user = req.user || null;
    res.locals.isAuthenticated = !!req.user;
    res.locals.currentPath = req.path;
    next();
});

// Public routes
router.get('/', (req, res) => {
    res.render('index', {
        title: 'Home - Full-Stack Web App',
        description: 'Welcome to our full-stack web application'
    });
});

router.get('/about', (req, res) => {
    res.render('about', {
        title: 'About - Full-Stack Web App'
    });
});

router.get('/contact', (req, res) => {
    res.render('contact', {
        title: 'Contact - Full-Stack Web App'
    });
});

// Auth routes (public)
router.get('/login', (req, res) => {
    if (req.user) {
        return res.redirect('/dashboard');
    }
    res.render('auth/login', {
        title: 'Login',
        error: req.query.error
    });
});

router.get('/register', (req, res) => {
    if (req.user) {
        return res.redirect('/dashboard');
    }
    res.render('auth/register', {
        title: 'Register'
    });
});

// Protected routes
router.get('/dashboard', auth, (req, res) => {
    res.render('dashboard/index', {
        title: 'Dashboard',
        user: req.user
    });
});

router.get('/profile', auth, (req, res) => {
    res.render('dashboard/profile', {
        title: 'Profile',
        user: req.user
    });
});

// Error routes
router.get('/404', (req, res) => {
    res.status(404).render('errors/404', {
        title: 'Page Not Found'
    });
});

router.get('/500', (req, res) => {
    res.status(500).render('errors/500', {
        title: 'Server Error'
    });
});

module.exports = router;
```


# Full-Stack Web Application - Middleware Implementation

## server/middleware/auth.js

```javascript
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const { promisify } = require('util'); // ES6+ destructuring from util

// JWT token verification middleware
const authenticateToken = async (req, res, next) => {
    try {
        // Extract token from Authorization header or cookies
        const authHeader = req.headers['authorization'];
        const token = authHeader && authHeader.split(' ')[1] || req.cookies.token;
        
        if (!token) {
            return res.status(401).json({ 
                success: false, 
                message: 'Access token required' 
            });
        }

        // Verify JWT token using ES6+ async/await
        const decoded = await promisify(jwt.verify)(token, process.env.JWT_SECRET);
        
        // Fetch user from database
        const user = await User.findById(decoded.userId).select('-password');
        if (!user) {
            return res.status(401).json({ 
                success: false, 
                message: 'User not found' 
            });
        }

        // Attach user to request object
        req.user = user;
        next();
    } catch (error) {
        // Handle different JWT errors
        if (error.name === 'TokenExpiredError') {
            return res.status(401).json({ 
                success: false, 
                message: 'Token expired' 
            });
        }
        if (error.name === 'JsonWebTokenError') {
            return res.status(401).json({ 
                success: false, 
                message: 'Invalid token' 
            });
        }
        
        console.error('Auth middleware error:', error);
        res.status(500).json({ 
            success: false, 
            message: 'Authentication failed' 
        });
    }
};

// Optional authentication middleware (doesn't fail if no token)
const optionalAuth = async (req, res, next) => {
    try {
        const authHeader = req.headers['authorization'];
        const token = authHeader && authHeader.split(' ')[1] || req.cookies.token;
        
        if (token) {
            const decoded = await promisify(jwt.verify)(token, process.env.JWT_SECRET);
            const user = await User.findById(decoded.userId).select('-password');
            if (user) {
                req.user = user;
            }
        }
        next();
    } catch (error) {
        // Continue without authentication
        next();
    }
};

// Role-based authorization middleware
const authorize = (...roles) => {
    return (req, res, next) => {
        if (!req.user) {
            return res.status(401).json({ 
                success: false, 
                message: 'Authentication required' 
            });
        }
        
        // Check if user has required role using ES6+ includes
        if (!roles.includes(req.user.role)) {
            return res.status(403).json({ 
                success: false, 
                message: 'Insufficient permissions' 
            });
        }
        
        next();
    };
};

// Session-based authentication middleware
const authenticateSession = (req, res, next) => {
    if (req.session && req.session.userId) {
        User.findById(req.session.userId)
            .select('-password')
            .then(user => {
                if (user) {
                    req.user = user;
                    next();
                } else {
                    req.session.destroy();
                    res.status(401).json({ 
                        success: false, 
                        message: 'Session invalid' 
                    });
                }
            })
            .catch(error => {
                console.error('Session auth error:', error);
                res.status(500).json({ 
                    success: false, 
                    message: 'Authentication failed' 
                });
            });
    } else {
        res.status(401).json({ 
            success: false, 
            message: 'Session required' 
        });
    }
};

module.exports = {
    authenticateToken,
    optionalAuth,
    authorize,
    authenticateSession
};
```

---

## server/middleware/validation.js

```javascript
const { body, validationResult, param, query } = require('express-validator');
const xss = require('xss');
const DOMPurify = require('dompurify');
const { JSDOM } = require('jsdom');

// Create DOMPurify instance for server-side sanitization
const window = new JSDOM('').window;
const purify = DOMPurify(window);

// Validation error handler
const handleValidationErrors = (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
        // ES6+ map to format errors
        const formattedErrors = errors.array().map(error => ({
            field: error.param,
            message: error.msg,
            value: error.value
        }));
        
        return res.status(400).json({
            success: false,
            message: 'Validation failed',
            errors: formattedErrors
        });
    }
    next();
};

// Sanitization middleware
const sanitizeInput = (req, res, next) => {
    // Sanitize request body recursively
    const sanitizeObject = (obj) => {
        for (let key in obj) {
            if (typeof obj[key] === 'string') {
                // Remove XSS attempts and purify HTML
                obj[key] = purify.sanitize(xss(obj[key]));
            } else if (typeof obj[key] === 'object' && obj[key] !== null) {
                sanitizeObject(obj[key]);
            }
        }
    };

    if (req.body) sanitizeObject(req.body);
    if (req.query) sanitizeObject(req.query);
    if (req.params) sanitizeObject(req.params);
    
    next();
};

// User registration validation rules
const validateUserRegistration = [
    body('name')
        .trim()
        .isLength({ min: 2, max: 50 })
        .withMessage('Name must be between 2 and 50 characters')
        .matches(/^[a-zA-Z\s]+$/)
        .withMessage('Name can only contain letters and spaces'),
    
    body('email')
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
                throw new Error('Passwords do not match');
            }
            return true;
        }),
    
    handleValidationErrors
];

// User login validation rules
const validateUserLogin = [
    body('email')
        .isEmail()
        .normalizeEmail()
        .withMessage('Please provide a valid email address'),
    
    body('password')
        .notEmpty()
        .withMessage('Password is required'),
    
    handleValidationErrors
];

// User profile update validation
const validateProfileUpdate = [
    body('name')
        .optional()
        .trim()
        .isLength({ min: 2, max: 50 })
        .withMessage('Name must be between 2 and 50 characters'),
    
    body('email')
        .optional()
        .isEmail()
        .normalizeEmail()
        .withMessage('Please provide a valid email address'),
    
    body('phone')
        .optional()
        .matches(/^[\+]?[1-9][\d]{0,15}$/)
        .withMessage('Please provide a valid phone number'),
    
    body('bio')
        .optional()
        .isLength({ max: 500 })
        .withMessage('Bio cannot exceed 500 characters'),
    
    handleValidationErrors
];

// Password change validation
const validatePasswordChange = [
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
                throw new Error('New passwords do not match');
            }
            return true;
        }),
    
    handleValidationErrors
];

// ID parameter validation
const validateObjectId = [
    param('id')
        .isMongoId()
        .withMessage('Invalid ID format'),
    
    handleValidationErrors
];

// Pagination validation
const validatePagination = [
    query('page')
        .optional()
        .isInt({ min: 1 })
        .withMessage('Page must be a positive integer'),
    
    query('limit')
        .optional()
        .isInt({ min: 1, max: 100 })
        .withMessage('Limit must be between 1 and 100'),
    
    handleValidationErrors
];

module.exports = {
    handleValidationErrors,
    sanitizeInput,
    validateUserRegistration,
    validateUserLogin,
    validateProfileUpdate,
    validatePasswordChange,
    validateObjectId,
    validatePagination
};
```

---

## server/middleware/rateLimit.js

```javascript
const rateLimit = require('express-rate-limit');
const slowDown = require('express-slow-down');
const MongoStore = require('rate-limit-mongo');

// MongoDB store for rate limiting (distributed systems)
const mongoStore = new MongoStore({
    uri: process.env.MONGODB_URI,
    collectionName: 'rateLimits',
    expireTimeMs: 15 * 60 * 1000 // 15 minutes
});

// General API rate limiting
const apiLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: {
        success: false,
        message: 'Too many requests from this IP, please try again later.'
    },
    standardHeaders: true, // Return rate limit info in the `RateLimit-*` headers
    legacyHeaders: false, // Disable the `X-RateLimit-*` headers
    store: mongoStore,
    skip: (req) => {
        // Skip rate limiting for trusted IPs or internal requests
        const trustedIPs = process.env.TRUSTED_IPS?.split(',') || [];
        return trustedIPs.includes(req.ip);
    }
});

// Strict rate limiting for authentication endpoints
const authLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 5, // limit each IP to 5 auth requests per windowMs
    message: {
        success: false,
        message: 'Too many authentication attempts, please try again in 15 minutes.'
    },
    standardHeaders: true,
    legacyHeaders: false,
    store: mongoStore,
    skipSuccessfulRequests: true // Don't count successful requests
});

// Progressive delay for repeated requests
const speedLimiter = slowDown({
    windowMs: 15 * 60 * 1000, // 15 minutes
    delayAfter: 2, // allow 2 requests per windowMs without delay
    delayMs: 500, // add 500ms delay per request after delayAfter
    maxDelayMs: 20000, // maximum delay of 20 seconds
    store: mongoStore
});

// Account creation rate limiting
const createAccountLimiter = rateLimit({
    windowMs: 60 * 60 * 1000, // 1 hour
    max: 3, // limit each IP to 3 account creation requests per hour
    message: {
        success: false,
        message: 'Too many accounts created from this IP, please try again in an hour.'
    },
    standardHeaders: true,
    legacyHeaders: false,
    store: mongoStore
});

// Password reset rate limiting
const passwordResetLimiter = rateLimit({
    windowMs: 60 * 60 * 1000, // 1 hour
    max: 3, // limit each IP to 3 password reset requests per hour
    message: {
        success: false,
        message: 'Too many password reset attempts, please try again in an hour.'
    },
    standardHeaders: true,
    legacyHeaders: false,
    store: mongoStore
});

// File upload rate limiting
const uploadLimiter = rateLimit({
    windowMs: 60 * 60 * 1000, // 1 hour
    max: 10, // limit each IP to 10 file uploads per hour
    message: {
        success: false,
        message: 'Too many file uploads, please try again in an hour.'
    },
    standardHeaders: true,
    legacyHeaders: false,
    store: mongoStore
});

// Custom rate limiter factory
const createRateLimiter = (options = {}) => {
    const defaultOptions = {
        windowMs: 15 * 60 * 1000,
        max: 100,
        message: {
            success: false,
            message: 'Too many requests, please try again later.'
        },
        standardHeaders: true,
        legacyHeaders: false,
        store: mongoStore
    };
    
    return rateLimit({ ...defaultOptions, ...options });
};

module.exports = {
    apiLimiter,
    authLimiter,
    speedLimiter,
    createAccountLimiter,
    passwordResetLimiter,
    uploadLimiter,
    createRateLimiter
};
```

---

## server/middleware/cors.js

```javascript
const cors = require('cors');

// CORS configuration
const corsOptions = {
    origin: function (origin, callback) {
        // List of allowed origins
        const allowedOrigins = [
            process.env.CLIENT_URL || 'http://localhost:3000',
            process.env.ADMIN_URL || 'http://localhost:3001',
            'http://localhost:3000',
            'http://localhost:3001'
        ];
        
        // Allow requests with no origin (mobile apps, Postman, etc.)
        if (!origin) return callback(null, true);
        
        if (allowedOrigins.indexOf(origin) !== -1) {
            callback(null, true);
        } else {
            callback(new Error('Not allowed by CORS policy'));
        }
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
        'X-CSRF-Token'
    ],
    optionsSuccessStatus: 200 // Legacy browser support
};

// Development CORS (more permissive)
const devCorsOptions = {
    origin: true, // Allow all origins in development
    credentials: true,
    methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS'],
    allowedHeaders: '*',
    optionsSuccessStatus: 200
};

// Production CORS (strict)
const prodCorsOptions = {
    ...corsOptions,
    origin: function (origin, callback) {
        const allowedOrigins = [
            process.env.CLIENT_URL,
            process.env.ADMIN_URL
        ].filter(Boolean); // Remove undefined values
        
        if (!origin) return callback(null, false); // Disallow requests with no origin
        
        if (allowedOrigins.includes(origin)) {
            callback(null, true);
        } else {
            callback(new Error('Not allowed by CORS policy'));
        }
    }
};

// Custom CORS middleware with additional security
const customCorsMiddleware = (req, res, next) => {
    const origin = req.get('Origin');
    
    // Add security headers
    res.setHeader('X-Content-Type-Options', 'nosniff');
    res.setHeader('X-Frame-Options', 'DENY');
    res.setHeader('X-XSS-Protection', '1; mode=block');
    
    // Log CORS requests in development
    if (process.env.NODE_ENV === 'development' && origin) {
        console.log(`CORS request from origin: ${origin}`);
    }
    
    next();
};

// Choose CORS configuration based on environment
const getCorsOptions = () => {
    if (process.env.NODE_ENV === 'production') {
        return prodCorsOptions;
    } else if (process.env.NODE_ENV === 'development') {
        return devCorsOptions;
    } else {
        return corsOptions;
    }
};

module.exports = {
    corsOptions,
    devCorsOptions,
    prodCorsOptions,
    customCorsMiddleware,
    getCorsOptions,
    // Export configured CORS middleware
    corsMiddleware: cors(getCorsOptions())
};
```

---

## server/middleware/errorHandler.js

```javascript
const fs = require('fs').promises;
const path = require('path');

// Custom error class for application errors
class AppError extends Error {
    constructor(message, statusCode, isOperational = true) {
        super(message);
        this.statusCode = statusCode;
        this.isOperational = isOperational;
        this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
        
        // Capture stack trace
        Error.captureStackTrace(this, this.constructor);
    }
}

// Async error wrapper
const asyncHandler = (fn) => {
    return (req, res, next) => {
        Promise.resolve(fn(req, res, next)).catch(next);
    };
};

// Error logging function
const logError = async (error, req) => {
    const logData = {
        timestamp: new Date().toISOString(),
        method: req.method,
        url: req.url,
        ip: req.ip,
        userAgent: req.get('User-Agent'),
        userId: req.user?.id || 'anonymous',
        error: {
            message: error.message,
            stack: error.stack,
            statusCode: error.statusCode
        }
    };
    
    try {
        // Log to file in production
        if (process.env.NODE_ENV === 'production') {
            const logFile = path.join(__dirname, '../../logs', 'error.log');
            await fs.appendFile(logFile, JSON.stringify(logData) + '\n');
        }
        
        // Log to console in development
        if (process.env.NODE_ENV === 'development') {
            console.error('Error Details:', logData);
        }
    } catch (logError) {
        console.error('Failed to log error:', logError);
    }
};

// Development error response
const sendErrorDev = (err, req, res) => {
    res.status(err.statusCode).json({
        success: false,
        error: err,
        message: err.message,
        stack: err.stack,
        request: {
            method: req.method,
            url: req.url,
            body: req.body,
            params: req.params,
            query: req.query
            }
    });
};

// Production error response
const sendErrorProd = (err, req, res) => {
    // Operational, trusted error: send message to client
    if (err.isOperational) {
        res.status(err.statusCode).json({
            success: false,
            message: err.message
        });
    } else {
        // Programming or other unknown error: don't leak error details
        console.error('ERROR:', err);
        
        res.status(500).json({
            success: false,
            message: 'Something went wrong!'
        });
    }
};

// Handle specific error types
const handleCastErrorDB = (err) => {
    const message = `Invalid ${err.path}: ${err.value}`;
    return new AppError(message, 400);
};

const handleDuplicateFieldsDB = (err) => {
    const value = err.errmsg.match(/(["'])(\\?.)*?\1/)[0];
    const message = `Duplicate field value: ${value}. Please use another value!`;
    return new AppError(message, 400);
};

const handleValidationErrorDB = (err) => {
    const errors = Object.values(err.errors).map(el => el.message);
    const message = `Invalid input data. ${errors.join('. ')}`;
    return new AppError(message, 400);
};

const handleJWTError = () =>
    new AppError('Invalid token. Please log in again!', 401);

const handleJWTExpiredError = () =>
    new AppError('Your token has expired! Please log in again.', 401);

// Main error handling middleware
const errorHandler = async (err, req, res, next) => {
    // Set default error properties
    err.statusCode = err.statusCode || 500;
    err.status = err.status || 'error';
    
    // Log error
    await logError(err, req);
    
    if (process.env.NODE_ENV === 'development') {
        sendErrorDev(err, req, res);
    } else {
        let error = { ...err };
        error.message = err.message;
        
        // Handle specific error types
        if (error.name === 'CastError') error = handleCastErrorDB(error);
        if (error.code === 11000) error = handleDuplicateFieldsDB(error);
        if (error.name === 'ValidationError') error = handleValidationErrorDB(error);
        if (error.name === 'JsonWebTokenError') error = handleJWTError();
        if (error.name === 'TokenExpiredError') error = handleJWTExpiredError();
        
        sendErrorProd(error, req, res);
    }
};

// 404 handler
const notFound = (req, res, next) => {
    const error = new AppError(`Not found - ${req.originalUrl}`, 404);
    next(error);
};

// Unhandled rejection handler
const handleUnhandledRejection = (server) => {
    process.on('unhandledRejection', (err, promise) => {
        console.log('Unhandled Rejection at:', promise, 'reason:', err);
        // Close server & exit process
        server.close(() => {
            process.exit(1);
        });
    });
};

// Uncaught exception handler
const handleUncaughtException = () => {
    process.on('uncaughtException', (err) => {
        console.log('Uncaught Exception thrown:', err);
        process.exit(1);
    });
};

module.exports = {
    AppError,
    asyncHandler,
    errorHandler,
    notFound,
    handleUnhandledRejection,
    handleUncaughtException
};
```

---

## server/middleware/logger.js

```javascript
const winston = require('winston');
const morgan = require('morgan');
const path = require('path');
const fs = require('fs');

// Create logs directory if it doesn't exist
const logsDir = path.join(__dirname, '../../logs');
if (!fs.existsSync(logsDir)) {
    fs.mkdirSync(logsDir, { recursive: true });
}

// Custom log format
const customFormat = winston.format.combine(
    winston.format.timestamp({
        format: 'YYYY-MM-DD HH:mm:ss'
    }),
    winston.format.errors({ stack: true }),
    winston.format.json(),
    winston.format.prettyPrint()
);

// Winston logger configuration
const logger = winston.createLogger({
    level: process.env.LOG_LEVEL || 'info',
    format: customFormat,
    defaultMeta: { service: 'fullstack-app' },
    transports: [
        // Write all logs with level 'error' and below to error.log
        new winston.transports.File({
            filename: path.join(logsDir, 'error.log'),
            level: 'error',
            maxsize: 5242880, // 5MB
            maxFiles: 5,
            tailable: true
        }),
        
        // Write all logs with level 'info' and below to combined.log
        new winston.transports.File({
            filename: path.join(logsDir, 'combined.log'),
            maxsize: 5242880, // 5MB
            maxFiles: 5,
            tailable: true
        })
    ],
    
    // Handle exceptions
    exceptionHandlers: [
        new winston.transports.File({
            filename: path.join(logsDir, 'exceptions.log')
        })
    ],
    
    // Handle rejections
    rejectionHandlers: [
        new winston.transports.File({
            filename: path.join(logsDir, 'rejections.log')
        })
    ]
});

// Add console transport for development
if (process.env.NODE_ENV !== 'production') {
    logger.add(new winston.transports.Console({
        format: winston.format.combine(
            winston.format.colorize(),
            winston.format.simple()
        )
    }));
}

// Morgan stream for HTTP request logging
const morganStream = {
    write: (message) => {
        logger.info(message.trim());
    }
};

// Custom Morgan tokens
morgan.token('user-id', (req) => {
    return req.user ? req.user.id : 'anonymous';
});

morgan.token('real-ip', (req) => {
    return req.ip || req.connection.remoteAddress;
});

// Development logging format
const developmentFormat = ':method :url :status :res[content-length] - :response-time ms - :user-id - :real-ip';

// Production logging format
const productionFormat = ':remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] ":referrer" ":user-agent" - :user-id - :response-time ms';

// Request logging middleware
const requestLogger = morgan(
    process.env.NODE_ENV === 'production' ? productionFormat : developmentFormat,
    { stream: morganStream }
);

// Security logging middleware
const securityLogger = (req, res, next) => {
    // Log security-related events
    const securityEvents = [
        'login',
        'logout',
        'register',
        'password-change',
        'password-reset',
        'oauth-login',
        'failed-login'
    ];
    
    // Check if this is a security-related request
    const isSecurityEvent = securityEvents.some(event => 
        req.url.includes(event) || req.body?.action === event
    );
    
    if (isSecurityEvent) {
        logger.info('Security Event', {
            type: 'security',
            method: req.method,
            url: req.url,
            ip: req.ip,
            userAgent: req.get('User-Agent'),
            userId: req.user?.id || 'anonymous',
            timestamp: new Date().toISOString()
        });
    }
    
    next();
};

// Database query logging
const dbLogger = (query, executionTime) => {
    if (process.env.NODE_ENV === 'development') {
        logger.debug('Database Query', {
            query: query.toString(),
            executionTime: `${executionTime}ms`,
            timestamp: new Date().toISOString()
        });
    }
};

// API endpoint logging
const apiLogger = (endpoint, method, userId, responseTime) => {
    logger.info('API Call', {
        endpoint,
        method,
        userId: userId || 'anonymous',
        responseTime: `${responseTime}ms`,
        timestamp: new Date().toISOString()
    });
};

// Performance monitoring middleware
const performanceLogger = (req, res, next) => {
    const startTime = Date.now();
    
    // Override res.end to log response time
    const originalEnd = res.end;
    res.end = function(...args) {
        const responseTime = Date.now() - startTime;
        
        // Log slow requests (> 1 second)
        if (responseTime > 1000) {
            logger.warn('Slow Request', {
                method: req.method,
                url: req.url,
                responseTime: `${responseTime}ms`,
                statusCode: res.statusCode,
                userAgent: req.get('User-Agent'),
                timestamp: new Date().toISOString()
            });
        }
        
        originalEnd.apply(this, args);
    };
    
    next();
};

module.exports = {
    logger,
    requestLogger,
    securityLogger,
    dbLogger,
    apiLogger,
    performanceLogger
};
```

## Configuration Files

---

## server/config/database.js

```javascript
const mongoose = require('mongoose');
const mysql = require('mysql2/promise');

// MongoDB Configuration with ES6+ async/await
const connectMongoDB = async () => {
    try {
        const mongoURI = process.env.MONGODB_URI || 'mongodb://localhost:27017/fullstack_app';
        
        // ES6+ object shorthand for options
        const options = {
            useNewUrlParser: true,
            useUnifiedTopology: true,
            maxPoolSize: 10, // Maintain up to 10 socket connections
            serverSelectionTimeoutMS: 5000, // Keep trying to send operations for 5 seconds
            socketTimeoutMS: 45000, // Close sockets after 45 seconds of inactivity
        };

        await mongoose.connect(mongoURI, options);
        console.log('✅ MongoDB connected successfully');
        
        // Connection event listeners
        mongoose.connection.on('error', (err) => {
            console.error('❌ MongoDB connection error:', err);
        });
        
        mongoose.connection.on('disconnected', () => {
            console.log('⚠️ MongoDB disconnected');
        });
        
    } catch (error) {
        console.error('❌ MongoDB connection failed:', error);
        process.exit(1);
    }
};

// MySQL Configuration with connection pooling
const createMySQLPool = () => {
    try {
        const pool = mysql.createPool({
            host: process.env.MYSQL_HOST || 'localhost',
            user: process.env.MYSQL_USER || 'root',
            password: process.env.MYSQL_PASSWORD || '',
            database: process.env.MYSQL_DATABASE || 'fullstack_app',
            waitForConnections: true,
            connectionLimit: 10,
            queueLimit: 0,
            acquireTimeout: 60000,
            timeout: 60000,
        });

        console.log('✅ MySQL pool created successfully');
        return pool;
    } catch (error) {
        console.error('❌ MySQL pool creation failed:', error);
        process.exit(1);
    }
};

// Test MySQL connection
const testMySQLConnection = async (pool) => {
    try {
        const connection = await pool.getConnection();
        console.log('✅ MySQL connected successfully');
        connection.release();
    } catch (error) {
        console.error('❌ MySQL connection failed:', error);
    }
};

// Export both database connections
module.exports = {
    connectMongoDB,
    createMySQLPool,
    testMySQLConnection
};
```

---

## server/config/passport.js

```javascript
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
const GoogleStrategy = require('passport-google-oauth20').Strategy;
const JwtStrategy = require('passport-jwt').Strategy;
const ExtractJwt = require('passport-jwt').ExtractJwt;
const bcrypt = require('bcryptjs');
const User = require('../models/User');

// Local Strategy for email/password authentication
passport.use(new LocalStrategy({
    usernameField: 'email', // Use email instead of username
    passwordField: 'password'
}, async (email, password, done) => {
    try {
        // Find user by email
        const user = await User.findOne({ email: email.toLowerCase() });
        
        if (!user) {
            return done(null, false, { message: 'Invalid email or password' });
        }

        // Check if account is active
        if (!user.isActive) {
            return done(null, false, { message: 'Account is deactivated' });
        }

        // Verify password using bcrypt
        const isMatch = await bcrypt.compare(password, user.password);
        
        if (!isMatch) {
            return done(null, false, { message: 'Invalid email or password' });
        }

        // Update last login
        user.lastLogin = new Date();
        await user.save();

        return done(null, user);
    } catch (error) {
        return done(error);
    }
}));

// Google OAuth Strategy
passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: process.env.GOOGLE_CALLBACK_URL || '/auth/google/callback'
}, async (accessToken, refreshToken, profile, done) => {
    try {
        // Check if user already exists with Google ID
        let user = await User.findOne({ googleId: profile.id });

        if (user) {
            // Update last login
            user.lastLogin = new Date();
            await user.save();
            return done(null, user);
        }

        // Check if user exists with same email
        user = await User.findOne({ email: profile.emails[0].value });

        if (user) {
            // Link Google account to existing user
            user.googleId = profile.id;
            user.avatar = profile.photos[0].value;
            user.lastLogin = new Date();
            await user.save();
            return done(null, user);
        }

        // Create new user with Google data
        const newUser = new User({
            googleId: profile.id,
            name: profile.displayName,
            email: profile.emails[0].value,
            avatar: profile.photos[0].value,
            isActive: true,
            emailVerified: true, // Google emails are pre-verified
            provider: 'google',
            lastLogin: new Date()
        });

        await newUser.save();
        return done(null, newUser);
    } catch (error) {
        return done(error);
    }
}));

// JWT Strategy for API authentication
passport.use(new JwtStrategy({
    jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
    secretOrKey: process.env.JWT_SECRET
}, async (payload, done) => {
    try {
        const user = await User.findById(payload.userId).select('-password');
        
        if (user) {
            return done(null, user);
        }
        
        return done(null, false);
    } catch (error) {
        return done(error);
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
        done(error);
    }
});

module.exports = passport;
```

---

## server/config/jwt.js

```javascript
const jwt = require('jsonwebtoken');
const crypto = require('crypto');

// JWT Configuration with ES6+ class syntax
class JWTConfig {
    constructor() {
        this.accessTokenSecret = process.env.JWT_SECRET || this.generateSecret();
        this.refreshTokenSecret = process.env.JWT_REFRESH_SECRET || this.generateSecret();
        this.accessTokenExpiry = process.env.JWT_EXPIRES_IN || '15m';
        this.refreshTokenExpiry = process.env.JWT_REFRESH_EXPIRES_IN || '7d';
    }

    // Generate random secret if not provided
    generateSecret() {
        return crypto.randomBytes(64).toString('hex');
    }

    // Generate access token
    generateAccessToken(payload) {
        return jwt.sign(payload, this.accessTokenSecret, {
            expiresIn: this.accessTokenExpiry,
            issuer: 'fullstack-app',
            audience: 'fullstack-app-users'
        });
    }

    // Generate refresh token
    generateRefreshToken(payload) {
        return jwt.sign(payload, this.refreshTokenSecret, {
            expiresIn: this.refreshTokenExpiry,
            issuer: 'fullstack-app',
            audience: 'fullstack-app-users'
        });
    }

    // Verify access token
    verifyAccessToken(token) {
        try {
            return jwt.verify(token, this.accessTokenSecret);
        } catch (error) {
            throw new Error('Invalid access token');
        }
    }

    // Verify refresh token
    verifyRefreshToken(token) {
        try {
            return jwt.verify(token, this.refreshTokenSecret);
        } catch (error) {
            throw new Error('Invalid refresh token');
        }
    }

    // Generate token pair
    generateTokenPair(userId, email) {
        const payload = { userId, email };
        
        return {
            accessToken: this.generateAccessToken(payload),
            refreshToken: this.generateRefreshToken(payload),
            expiresIn: this.accessTokenExpiry
        };
    }

    // Decode token without verification (for debugging)
    decodeToken(token) {
        return jwt.decode(token);
    }
}

// Export singleton instance
module.exports = new JWTConfig();
```

---

## server/config/oauth.js

```javascript
// OAuth Configuration for multiple providers
const oauthConfig = {
    google: {
        clientID: process.env.GOOGLE_CLIENT_ID,
        clientSecret: process.env.GOOGLE_CLIENT_SECRET,
        callbackURL: process.env.GOOGLE_CALLBACK_URL || '/auth/google/callback',
        scope: ['profile', 'email']
    },
    
    // Facebook OAuth (if needed)
    facebook: {
        clientID: process.env.FACEBOOK_CLIENT_ID,
        clientSecret: process.env.FACEBOOK_CLIENT_SECRET,
        callbackURL: process.env.FACEBOOK_CALLBACK_URL || '/auth/facebook/callback',
        scope: ['email', 'public_profile']
    },
    
    // GitHub OAuth (if needed)
    github: {
        clientID: process.env.GITHUB_CLIENT_ID,
        clientSecret: process.env.GITHUB_CLIENT_SECRET,
        callbackURL: process.env.GITHUB_CALLBACK_URL || '/auth/github/callback',
        scope: ['user:email']
    }
};

// OAuth redirect URLs for frontend
const getOAuthRedirectURL = (provider, success = true) => {
    const baseURL = process.env.CLIENT_URL || 'http://localhost:3000';
    
    if (success) {
        return `${baseURL}/auth/callback?provider=${provider}`;
    } else {
        return `${baseURL}/login?error=oauth_failed&provider=${provider}`;
    }
};

// Validate OAuth configuration
const validateOAuthConfig = () => {
    const requiredVars = [
        'GOOGLE_CLIENT_ID',
        'GOOGLE_CLIENT_SECRET'
    ];
    
    const missing = requiredVars.filter(varName => !process.env[varName]);
    
    if (missing.length > 0) {
        console.warn(`⚠️ Missing OAuth environment variables: ${missing.join(', ')}`);
        console.warn('OAuth authentication may not work properly');
    }
};

module.exports = {
    oauthConfig,
    getOAuthRedirectURL,
    validateOAuthConfig
};
```

---

## Backend Services

---

## server/services/authService.js

```javascript
const bcrypt = require('bcryptjs');
const User = require('../models/User');
const RefreshToken = require('../models/RefreshToken');
const jwtConfig = require('../config/jwt');
const emailService = require('./emailService');

class AuthService {
    // Register new user with ES6+ async/await
    async registerUser(userData) {
        try {
            const { name, email, password } = userData;
            
            // Check if user already exists
            const existingUser = await User.findOne({ 
                email: email.toLowerCase() 
            });
            
            if (existingUser) {
                throw new Error('User already exists with this email');
            }

            // Hash password with salt
            const saltRounds = 12;
            const hashedPassword = await bcrypt.hash(password, saltRounds);

            // Create new user
            const newUser = new User({
                name: name.trim(),
                email: email.toLowerCase(),
                password: hashedPassword,
                isActive: true,
                provider: 'local'
            });

            await newUser.save();

            // Send welcome email
            await emailService.sendWelcomeEmail(newUser.email, newUser.name);

            return {
                success: true,
                message: 'User registered successfully',
                user: {
                    id: newUser._id,
                    name: newUser.name,
                    email: newUser.email
                }
            };
        } catch (error) {
            throw new Error(error.message || 'Registration failed');
        }
    }

    // Login user and generate tokens
    async loginUser(email, password) {
        try {
            // Find user by email
            const user = await User.findOne({ 
                email: email.toLowerCase() 
            });

            if (!user) {
                throw new Error('Invalid email or password');
            }

            // Check if account is active
            if (!user.isActive) {
                throw new Error('Account is deactivated');
            }

            // Verify password
            const isValidPassword = await bcrypt.compare(password, user.password);
            if (!isValidPassword) {
                throw new Error('Invalid email or password');
            }

            // Generate tokens
            const tokens = jwtConfig.generateTokenPair(user._id, user.email);

            // Save refresh token to database
            await this.saveRefreshToken(user._id, tokens.refreshToken);

            // Update last login
            user.lastLogin = new Date();
            await user.save();

            return {
                success: true,
                message: 'Login successful',
                user: {
                    id: user._id,
                    name: user.name,
                    email: user.email,
                    role: user.role,
                    avatar: user.avatar
                },
                tokens
            };
        } catch (error) {
            throw new Error(error.message || 'Login failed');
        }
    }

    // Save refresh token to database
    async saveRefreshToken(userId, token) {
        try {
            // Remove old refresh tokens for this user
            await RefreshToken.deleteMany({ userId });

            // Save new refresh token
            const refreshToken = new RefreshToken({
                userId,
                token,
                expiresAt: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000) // 7 days
            });

            await refreshToken.save();
        } catch (error) {
            console.error('Error saving refresh token:', error);
        }
    }

    // Refresh access token
    async refreshAccessToken(refreshToken) {
        try {
            // Verify refresh token
            const decoded = jwtConfig.verifyRefreshToken(refreshToken);

            // Check if refresh token exists in database
            const storedToken = await RefreshToken.findOne({
                token: refreshToken,
                userId: decoded.userId
            });

            if (!storedToken || storedToken.expiresAt < new Date()) {
                throw new Error('Invalid or expired refresh token');
            }

            // Generate new access token
            const newAccessToken = jwtConfig.generateAccessToken({
                userId: decoded.userId,
                email: decoded.email
            });

            return {
                success: true,
                accessToken: newAccessToken,
                expiresIn: jwtConfig.accessTokenExpiry
            };
        } catch (error) {
            throw new Error('Token refresh failed');
        }
    }

    // Logout user and invalidate tokens
    async logoutUser(userId, refreshToken) {
        try {
            // Remove refresh token from database
            if (refreshToken) {
                await RefreshToken.deleteOne({
                    token: refreshToken,
                    userId
                });
            } else {
                // Remove all refresh tokens for user
                await RefreshToken.deleteMany({ userId });
            }

            return {
                success: true,
                message: 'Logout successful'
            };
        } catch (error) {
            throw new Error('Logout failed');
        }
    }

    // Change user password
    async changePassword(userId, oldPassword, newPassword) {
        try {
            const user = await User.findById(userId);
            if (!user) {
                throw new Error('User not found');
            }

            // Verify old password
            const isValidPassword = await bcrypt.compare(oldPassword, user.password);
            if (!isValidPassword) {
                throw new Error('Current password is incorrect');
            }

            // Hash new password
            const saltRounds = 12;
            const hashedPassword = await bcrypt.hash(newPassword, saltRounds);

            // Update password
            user.password = hashedPassword;
            user.passwordChangedAt = new Date();
            await user.save();

            // Invalidate all refresh tokens
            await RefreshToken.deleteMany({ userId });

            return {
                success: true,
                message: 'Password changed successfully'
            };
        } catch (error) {
            throw new Error(error.message || 'Password change failed');
        }
    }
}

module.exports = new AuthService();
```

---

## server/services/emailService.js

```javascript
const nodemailer = require('nodemailer');

class EmailService {
    constructor() {
        // Configure email transporter
        this.transporter = nodemailer.createTransporter({
            host: process.env.SMTP_HOST || 'smtp.gmail.com',
            port: process.env.SMTP_PORT || 587,
            secure: false, // true for 465, false for other ports
            auth: {
                user: process.env.SMTP_USER,
                pass: process.env.SMTP_PASS
            }
        });

        // Verify transporter configuration
        this.verifyTransporter();
    }

    async verifyTransporter() {
        try {
            await this.transporter.verify();
            console.log('✅ Email service is ready');
        } catch (error) {
            console.error('❌ Email service error:', error.message);
        }
    }

    // Send welcome email
    async sendWelcomeEmail(email, name) {
        try {
            const mailOptions = {
                from: process.env.FROM_EMAIL || 'noreply@fullstackapp.com',
                to: email,
                subject: 'Welcome to FullStack App!',
                html: `
                    <div style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
                        <h2 style="color: #333;">Welcome ${name}!</h2>
                        <p>Thank you for registering with FullStack App.</p>
                        <p>Your account has been created successfully. You can now login and start using our services.</p>
                        <div style="margin: 20px 0;">
                            <a href="${process.env.CLIENT_URL}/login" 
                               style="background-color: #007bff; color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px;">
                                Login to Your Account
                            </a>
                        </div>
                        <p>Best regards,<br>FullStack App Team</p>
                    </div>
                `
            };

            await this.transporter.sendMail(mailOptions);
            console.log(`✅ Welcome email sent to ${email}`);
        } catch (error) {
            console.error('❌ Failed to send welcome email:', error);
        }
    }

    // Send password reset email
    async sendPasswordResetEmail(email, resetToken) {
        try {
            const resetUrl = `${process.env.CLIENT_URL}/reset-password?token=${resetToken}`;
            
            const mailOptions = {
                from: process.env.FROM_EMAIL || 'noreply@fullstackapp.com',
                to: email,
                subject: 'Password Reset Request',
                html: `
                    <div style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
                        <h2 style="color: #333;">Password Reset Request</h2>
                        <p>You requested to reset your password. Click the button below to reset it:</p>
                        <div style="margin: 20px 0;">
                            <a href="${resetUrl}" 
                               style="background-color: #dc3545; color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px;">
                                Reset Password
                            </a>
                        </div>
                        <p>This link will expire in 1 hour.</p>
                        <p>If you didn't request this, please ignore this email.</p>
                        <p>Best regards,<br>FullStack App Team</p>
                    </div>
                `
            };

            await this.transporter.sendMail(mailOptions);
            console.log(`✅ Password reset email sent to ${email}`);
        } catch (error) {
            console.error('❌ Failed to send password reset email:', error);
        }
    }

    // Send email verification
    async sendEmailVerification(email, verificationToken) {
        try {
            const verifyUrl = `${process.env.CLIENT_URL}/verify-email?token=${verificationToken}`;
            
            const mailOptions = {
                from: process.env.FROM_EMAIL || 'noreply@fullstackapp.com',
                to: email,
                subject: 'Verify Your Email Address',
                html: `
                    <div style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
                        <h2 style="color: #333;">Email Verification</h2>
                        <p>Please verify your email address by clicking the button below:</p>
                        <div style="margin: 20px 0;">
                            <a href="${verifyUrl}" 
                               style="background-color: #28a745; color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px;">
                                Verify Email
                            </a>
                        </div>
                        <p>This link will expire in 24 hours.</p>
                        <p>Best regards,<br>FullStack App Team</p>
                    </div>
                `
            };

            await this.transporter.sendMail(mailOptions);
            console.log(`✅ Email verification sent to ${email}`);
        } catch (error) {
            console.error('❌ Failed to send email verification:', error);
        }
    }
}

module.exports = new EmailService();
```

---

## server/services/tokenService.js

```javascript
const crypto = require('crypto');
const jwt = require('jsonwebtoken');
const RefreshToken = require('../models/RefreshToken');

class TokenService {
    // Generate secure random token
    generateSecureToken(length = 32) {
        return crypto.randomBytes(length).toString('hex');
    }

    // Generate email verification token
    generateEmailVerificationToken(email) {
        const payload = {
            email,
            type: 'email_verification',
            timestamp: Date.now()
        };
        
        return jwt.sign(payload, process.env.JWT_SECRET, { expiresIn: '24h' });
    }

    // Generate password reset token
    generatePasswordResetToken(userId, email) {
        const payload = {
            userId,
            email,
            type: 'password_reset',
            timestamp: Date.now()
        };
        
        return jwt.sign(payload, process.env.JWT_SECRET, { expiresIn: '1h' });
    }

    // Verify email verification token
    verifyEmailVerificationToken(token) {
        try {
            const decoded = jwt.verify(token, process.env.JWT_SECRET);
            
            if (decoded.type !== 'email_verification') {
                throw new Error('Invalid token type');
            }
            
            return decoded;
        } catch (error) {
            throw new Error('Invalid or expired verification token');
        }
    }

    // Verify password reset token
    verifyPasswordResetToken(token) {
        try {
            const decoded = jwt.verify(token, process.env.JWT_SECRET);
            
            if (decoded.type !== 'password_reset') {
                throw new Error('Invalid token type');
            }
            
            return decoded;
        } catch (error) {
            throw new Error('Invalid or expired reset token');
        }
    }

    // Clean up expired refresh tokens
    async cleanupExpiredTokens() {
        try {
            const result = await RefreshToken.deleteMany({
                expiresAt: { $lt: new Date() }
            });
            
            if (result.deletedCount > 0) {
                console.log(`🧹 Cleaned up ${result.deletedCount} expired refresh tokens`);
            }
        } catch (error) {
            console.error('❌ Error cleaning up expired tokens:', error);
        }
    }

    // Revoke all user tokens
    async revokeAllUserTokens(userId) {
        try {
            await RefreshToken.deleteMany({ userId });
            console.log(`🔒 Revoked all tokens for user ${userId}`);
        } catch (error) {
            console.error('❌ Error revoking user tokens:', error);
        }
    }

    // Get token statistics
    async getTokenStats() {
        try {
            const totalTokens = await RefreshToken.countDocuments();
            const expiredTokens = await RefreshToken.countDocuments({
                expiresAt: { $lt: new Date() }
            });
            
            return {
                total: totalTokens,
                expired: expiredTokens,
                active: totalTokens - expiredTokens
            };
        } catch (error) {
            console.error('❌ Error getting token stats:', error);
            return { total: 0, expired: 0, active: 0 };
        }
    }
}

module.exports = new TokenService();
```

---

## server/services/userService.js

```javascript
const User = require('../models/User');
const bcrypt = require('bcryptjs');

class UserService {
    // Get user profile by ID
    async getUserProfile(userId) {
        try {
            const user = await User.findById(userId)
                .select('-password -__v')
                .lean();
            
            if (!user) {
                throw new Error('User not found');
            }
            
            return {
                success: true,
                user
            };
        } catch (error) {
            throw new Error(error.message || 'Failed to fetch user profile');
        }
    }

    // Update user profile
    async updateUserProfile(userId, updateData) {
        try {
            const allowedUpdates = ['name', 'phone', 'bio', 'avatar'];
            const updates = {};
            
            // Filter allowed updates
            Object.keys(updateData).forEach(key => {
                if (allowedUpdates.includes(key) && updateData[key] !== undefined) {
                    updates[key] = updateData[key];
                }
            });
            
            if (Object.keys(updates).length === 0) {
                throw new Error('No valid updates provided');
            }
            
            const user = await User.findByIdAndUpdate(
                userId,
                { ...updates, updatedAt: new Date() },
                { new: true, runValidators: true }
            ).select('-password -__v');
            
            if (!user) {
                throw new Error('User not found');
            }
            
            return {
                success: true,
                message: 'Profile updated successfully',
                user
            };
        } catch (error) {
            throw new Error(error.message || 'Failed to update profile');
        }
    }

    // Get user statistics for dashboard
    async getUserStats(userId) {
        try {
            const user = await User.findById(userId);
            if (!user) {
                throw new Error('User not found');
            }
            
            // Get various statistics
            const stats = {
                totalUsers: await User.countDocuments(),
                activeUsers: await User.countDocuments({ isActive: true }),
                newUsersThisMonth: await User.countDocuments({
                    createdAt: { 
                        $gte: new Date(new Date().getFullYear(), new Date().getMonth(), 1) 
                    }
                }),
                userLoginCount: user.loginCount || 0,
                accountAge: Math.floor((new Date() - user.createdAt) / (1000 * 60 * 60 * 24)),
                lastLogin: user.lastLogin
            };
            
            return {
                success: true,
                stats
            };
        } catch (error) {
            throw new Error(error.message || 'Failed to fetch user statistics');
        }
    }

    // Search users (admin functionality)
    async searchUsers(query, page = 1, limit = 10) {
        try {
            const skip = (page - 1) * limit;
            
            const searchQuery = {
                $or: [
                    { name: { $regex: query, $options: 'i' } },
                    { email: { $regex: query, $options: 'i' } }
                ]
            };
            
            const users = await User.find(searchQuery)
                .select('-password -__v')
                .sort({ createdAt: -1 })
                .skip(skip)
                .limit(limit)
                .lean();
            
            const total = await User.countDocuments(searchQuery);
            
            return {
                success: true,
                users,
                pagination: {
                    page,
                    limit,
                    total,
                    pages: Math.ceil(total / limit)
                }
            };
        } catch (error) {
            throw new Error(error.message || 'Failed to search users');
        }
    }

    // Deactivate user account
    async deactivateUser(userId) {
        try {
            const user = await User.findByIdAndUpdate(
                userId,
                { isActive: false, deactivatedAt: new Date() },
                { new: true }
            ).select('-password -__v');
            
            if (!user) {
                throw new Error('User not found');
            }
            
            return {
                success: true,
                message: 'User account deactivated successfully',
                user
            };
        } catch (error) {
            throw new Error(error.message || 'Failed to deactivate user');
        }
    }

    // Activate user account
    async activateUser(userId) {
        try {
            const user = await User.findByIdAndUpdate(
                userId,
                { isActive: true, $unset: { deactivatedAt: 1 } },
                { new: true }
            ).select('-password -__v');
            
            if (!user) {
                throw new Error('User not found');
            }
            
            return {
                success: true,
                message: 'User account activated successfully',
                user
            };
        } catch (error) {
            throw new Error(error.message || 'Failed to activate user');
        }
    }
}

module.exports = new UserService();
```

---

## server/utils/encryption.js

```javascript
const bcrypt = require('bcrypt');
const crypto = require('crypto');

// ES6+ class for encryption utilities
class EncryptionUtils {
    // Static method for password hashing with salt (ES6+ static methods)
    static async hashPassword(password, saltRounds = 12) {
        try {
            // Generate salt and hash password (modern approach vs legacy callback)
            const salt = await bcrypt.genSalt(saltRounds);
            const hashedPassword = await bcrypt.hash(password, salt);
            return hashedPassword;
        } catch (error) {
            throw new Error('Password hashing failed: ' + error.message);
        }
    }

    // Compare password with hash (ES6+ async/await vs legacy callbacks)
    static async comparePassword(password, hashedPassword) {
        try {
            return await bcrypt.compare(password, hashedPassword);
        } catch (error) {
            throw new Error('Password comparison failed: ' + error.message);
        }
    }

    // Generate random tokens using crypto module
    static generateRandomToken(length = 32) {
        return crypto.randomBytes(length).toString('hex');
    }

    // Generate secure random string for sessions
    static generateSessionId() {
        return crypto.randomBytes(16).toString('hex');
    }

    // AES encryption for sensitive data
    static encryptData(data, secretKey) {
        const algorithm = 'aes-256-cbc';
        const key = crypto.scryptSync(secretKey, 'salt', 32);
        const iv = crypto.randomBytes(16);
        
        const cipher = crypto.createCipher(algorithm, key);
        cipher.setAADTag(iv);
        
        let encrypted = cipher.update(data, 'utf8', 'hex');
        encrypted += cipher.final('hex');
        
        return {
            encrypted,
            iv: iv.toString('hex')
        };
    }

    // AES decryption for sensitive data
    static decryptData(encryptedData, secretKey, iv) {
        const algorithm = 'aes-256-cbc';
        const key = crypto.scryptSync(secretKey, 'salt', 32);
        
        const decipher = crypto.createDecipher(algorithm, key);
        decipher.setAADTag(Buffer.from(iv, 'hex'));
        
        let decrypted = decipher.update(encryptedData, 'hex', 'utf8');
        decrypted += decipher.final('utf8');
        
        return decrypted;
    }
}

module.exports = EncryptionUtils;
```

---

## server/utils/validation.js

```javascript
const validator = require('validator');

// ES6+ validation utilities with modern regex patterns
class ValidationUtils {
    // Email validation (ES6+ static methods)
    static isValidEmail(email) {
        return validator.isEmail(email) && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    }

    // Password strength validation
    static isStrongPassword(password) {
        // At least 8 characters, 1 uppercase, 1 lowercase, 1 number, 1 special char
        const strongPasswordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;
        return strongPasswordRegex.test(password);
    }

    // Name validation (no numbers or special characters)
    static isValidName(name) {
        const nameRegex = /^[a-zA-Z\s]{2,50}$/;
        return nameRegex.test(name.trim());
    }

    // Phone number validation (international format)
    static isValidPhone(phone) {
        const phoneRegex = /^\+?[\d\s\-\(\)]{10,15}$/;
        return phoneRegex.test(phone);
    }

    // Sanitize input to prevent XSS (ES6+ arrow functions)
    static sanitizeInput = (input) => {
        if (typeof input !== 'string') return input;
        
        // Remove HTML tags and escape special characters
        return validator.escape(validator.stripLow(input.trim()));
    }

    // Validate and sanitize user registration data
    static validateRegistrationData(data) {
        const errors = {};
        const { name, email, password, confirmPassword } = data;

        // Name validation
        if (!name || !this.isValidName(name)) {
            errors.name = 'Name must be 2-50 characters and contain only letters';
        }

        // Email validation
        if (!email || !this.isValidEmail(email)) {
            errors.email = 'Please provide a valid email address';
        }

        // Password validation
        if (!password || !this.isStrongPassword(password)) {
            errors.password = 'Password must be at least 8 characters with uppercase, lowercase, number, and special character';
        }

        // Confirm password validation
        if (password !== confirmPassword) {
            errors.confirmPassword = 'Passwords do not match';
        }

        return {
            isValid: Object.keys(errors).length === 0,
            errors,
            sanitizedData: {
                name: this.sanitizeInput(name),
                email: this.sanitizeInput(email?.toLowerCase()),
                password // Don't sanitize password as it might affect special characters
            }
        };
    }

    // Validate login data
    static validateLoginData(data) {
        const errors = {};
        const { email, password } = data;

        if (!email || !this.isValidEmail(email)) {
            errors.email = 'Please provide a valid email address';
        }

        if (!password || password.length < 6) {
            errors.password = 'Password is required';
        }

        return {
            isValid: Object.keys(errors).length === 0,
            errors,
            sanitizedData: {
                email: this.sanitizeInput(email?.toLowerCase()),
                password
            }
        };
    }

    // Validate MongoDB ObjectId
    static isValidObjectId(id) {
        return /^[0-9a-fA-F]{24}$/.test(id);
    }

    // Validate JWT token format
    static isValidJWTFormat(token) {
        const jwtRegex = /^[A-Za-z0-9-_]+\.[A-Za-z0-9-_]+\.[A-Za-z0-9-_]*$/;
        return jwtRegex.test(token);
    }
}

module.exports = ValidationUtils;
```

---

## server/utils/logger.js

```javascript
const winston = require('winston');
const path = require('path');

// ES6+ destructuring for winston formats
const { combine, timestamp, printf, colorize, errors } = winston.format;

// Custom log format using ES6+ template literals
const logFormat = printf(({ level, message, timestamp, stack }) => {
    return `${timestamp} [${level}]: ${stack || message}`;
});

// Create logger instance with ES6+ configuration
const logger = winston.createLogger({
    level: process.env.LOG_LEVEL || 'info',
    format: combine(
        timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
        errors({ stack: true }), // Log stack traces for errors
        logFormat
    ),
    defaultMeta: { service: 'fullstack-app' },
    transports: [
        // Error log file
        new winston.transports.File({
            filename: path.join(__dirname, '../logs/error.log'),
            level: 'error',
            maxsize: 5242880, // 5MB
            maxFiles: 5
        }),
        
        // Combined log file
        new winston.transports.File({
            filename: path.join(__dirname, '../logs/combined.log'),
            maxsize: 5242880, // 5MB
            maxFiles: 5
        })
    ]
});

// Add console transport for development (ES6+ conditional)
if (process.env.NODE_ENV !== 'production') {
    logger.add(new winston.transports.Console({
        format: combine(
            colorize(),
            timestamp({ format: 'HH:mm:ss' }),
            logFormat
        )
    }));
}

// ES6+ class for logging utilities
class LoggerUtils {
    // Static methods for different log levels
    static info(message, meta = {}) {
        logger.info(message, meta);
    }

    static error(message, error = null, meta = {}) {
        logger.error(message, { error: error?.stack || error, ...meta });
    }

    static warn(message, meta = {}) {
        logger.warn(message, meta);
    }

    static debug(message, meta = {}) {
        logger.debug(message, meta);
    }

    // Log HTTP requests (middleware helper)
    static logRequest(req, res, responseTime) {
        const logData = {
            method: req.method,
            url: req.originalUrl,
            statusCode: res.statusCode,
            responseTime: `${responseTime}ms`,
            userAgent: req.get('User-Agent'),
            ip: req.ip,
            userId: req.user?.id || 'anonymous'
        };

        if (res.statusCode >= 400) {
            this.error('HTTP Request Failed', null, logData);
        } else {
            this.info('HTTP Request', logData);
        }
    }

    // Log authentication events
    static logAuth(event, userId, details = {}) {
        this.info(`Auth Event: ${event}`, {
            userId,
            event,
            ...details
        });
    }

    // Log database operations
    static logDatabase(operation, collection, details = {}) {
        this.debug(`DB Operation: ${operation}`, {
            operation,
            collection,
            ...details
        });
    }
}

module.exports = { logger, LoggerUtils };
```

---

## server/utils/helpers.js

```javascript
const crypto = require('crypto');
const jwt = require('jsonwebtoken');

// ES6+ helper utilities class
class Helpers {
    // Generate unique identifier using ES6+ static methods
    static generateUniqueId() {
        return crypto.randomBytes(16).toString('hex') + Date.now().toString(36);
    }

    // Format response object (ES6+ object shorthand)
    static formatResponse(success, message, data = null, errors = null) {
        return {
            success,
            message,
            data,
            errors,
            timestamp: new Date().toISOString()
        };
    }

    // Success response helper
    static successResponse(message, data = null) {
        return this.formatResponse(true, message, data);
    }

    // Error response helper
    static errorResponse(message, errors = null) {
        return this.formatResponse(false, message, null, errors);
    }

    // Pagination helper using ES6+ default parameters
    static paginate(page = 1, limit = 10, total = 0) {
        const offset = (page - 1) * limit;
        const totalPages = Math.ceil(total / limit);
        
        return {
            page: parseInt(page),
            limit: parseInt(limit),
            offset,
            total,
            totalPages,
            hasNext: page < totalPages,
            hasPrev: page > 1
        };
    }

    // Generate JWT tokens (ES6+ destructuring)
    static generateTokens(payload) {
        const accessToken = jwt.sign(
            payload,
            process.env.JWT_ACCESS_SECRET,
            { expiresIn: process.env.JWT_ACCESS_EXPIRE || '15m' }
        );

        const refreshToken = jwt.sign(
            payload,
            process.env.JWT_REFRESH_SECRET,
            { expiresIn: process.env.JWT_REFRESH_EXPIRE || '7d' }
        );

        return { accessToken, refreshToken };
    }

    // Verify JWT token
    static verifyToken(token, secret) {
        try {
            return jwt.verify(token, secret);
        } catch (error) {
            throw new Error('Invalid token: ' + error.message);
        }
    }

    // Generate random OTP
    static generateOTP(length = 6) {
        const digits = '0123456789';
        let otp = '';
        
        for (let i = 0; i < length; i++) {
            otp += digits[Math.floor(Math.random() * 10)];
        }
        
        return otp;
    }

    // Calculate time difference in minutes
    static getTimeDifferenceInMinutes(date1, date2 = new Date()) {
        const diffInMs = Math.abs(date2 - date1);
        return Math.floor(diffInMs / (1000 * 60));
    }

    // Deep clone object (ES6+ JSON methods)
    static deepClone(obj) {
        return JSON.parse(JSON.stringify(obj));
    }

    // Remove sensitive fields from user object
    static sanitizeUser(user) {
        const { password, refreshToken, __v, ...sanitizedUser } = user.toObject ? user.toObject() : user;
        return sanitizedUser;
    }

    // Convert string to slug (URL-friendly)
    static createSlug(str) {
        return str
            .toLowerCase()
            .trim()
            .replace(/[^\w\s-]/g, '') // Remove special characters
            .replace(/[\s_-]+/g, '-') // Replace spaces and underscores with hyphens
            .replace(/^-+|-+$/g, ''); // Remove leading/trailing hyphens
    }

    // Validate environment variables
    static validateEnvVars(requiredVars) {
        const missing = requiredVars.filter(varName => !process.env[varName]);
        
        if (missing.length > 0) {
            throw new Error(`Missing required environment variables: ${missing.join(', ')}`);
        }
    }

    // Generate secure random string
    static generateSecureRandom(length = 32) {
        return crypto.randomBytes(Math.ceil(length / 2))
            .toString('hex')
            .slice(0, length);
    }

    // Delay execution (for rate limiting, etc.)
    static delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    // Check if string is JSON
    static isValidJSON(str) {
        try {
            JSON.parse(str);
            return true;
        } catch {
            return false;
        }
    }

    // Format file size in human readable format
    static formatFileSize(bytes) {
        if (bytes === 0) return '0 Bytes';
        
        const k = 1024;
        const sizes = ['Bytes', 'KB', 'MB', 'GB'];
        const i = Math.floor(Math.log(bytes) / Math.log(k));
        
        return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
    }
}

module.exports = Helpers;
```


## server/security/rateLimiter.js

```javascript
const rateLimit = require('express-rate-limit');
const RedisStore = require('rate-limit-redis');
const redis = require('redis');

// Redis client for rate limiting (optional, falls back to memory)
let redisClient;
try {
    redisClient = redis.createClient({
        host: process.env.REDIS_HOST || 'localhost',
        port: process.env.REDIS_PORT || 6379
    });
} catch (error) {
    console.warn('Redis not available, using memory store for rate limiting');
}

// General API rate limiter
const apiLimiter = rateLimit({
    store: redisClient ? new RedisStore({ client: redisClient }) : undefined,
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // Limit each IP to 100 requests per windowMs
    message: {
        error: 'Too many requests from this IP, please try again later.',
        retryAfter: '15 minutes'
    },
    standardHeaders: true, // Return rate limit info in headers
    legacyHeaders: false, // Disable X-RateLimit-* headers
});

// Strict limiter for authentication routes
const authLimiter = rateLimit({
    store: redisClient ? new RedisStore({ client: redisClient }) : undefined,
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 5, // Limit each IP to 5 requests per windowMs
    message: {
        error: 'Too many authentication attempts, please try again later.',
        retryAfter: '15 minutes'
    },
    skipSuccessfulRequests: true, // Don't count successful requests
});

// Password reset limiter
const passwordResetLimiter = rateLimit({
    windowMs: 60 * 60 * 1000, // 1 hour
    max: 3, // Limit each IP to 3 password reset requests per hour
    message: {
        error: 'Too many password reset attempts, please try again later.',
        retryAfter: '1 hour'
    },
});

// Account creation limiter
const createAccountLimiter = rateLimit({
    windowMs: 60 * 60 * 1000, // 1 hour
    max: 3, // Limit each IP to 3 account creation requests per hour
    message: {
        error: 'Too many accounts created from this IP, please try again later.',
        retryAfter: '1 hour'
    },
});

module.exports = {
    apiLimiter,
    authLimiter,
    passwordResetLimiter,
    createAccountLimiter
};
```

---

## server/security/sanitizer.js

```javascript
const validator = require('validator');
const DOMPurify = require('isomorphic-dompurify');

// ES6+ class for input sanitization
class Sanitizer {
    // Sanitize string input to prevent XSS
    static sanitizeString(input) {
        if (!input || typeof input !== 'string') return '';
        
        // Trim whitespace and remove HTML tags
        const trimmed = input.trim();
        const escaped = validator.escape(trimmed);
        
        return DOMPurify.sanitize(escaped);
    }

    // Sanitize email input
    static sanitizeEmail(email) {
        if (!email || typeof email !== 'string') return '';
        
        const trimmed = email.trim().toLowerCase();
        return validator.isEmail(trimmed) ? validator.normalizeEmail(trimmed) : '';
    }

    // Sanitize phone number
    static sanitizePhone(phone) {
        if (!phone || typeof phone !== 'string') return '';
        
        // Remove all non-digit characters except + for country code
        return phone.replace(/[^\d+]/g, '');
    }

    // Sanitize URL input
    static sanitizeUrl(url) {
        if (!url || typeof url !== 'string') return '';
        
        const trimmed = url.trim();
        return validator.isURL(trimmed) ? trimmed : '';
    }

    // Sanitize object inputs recursively
    static sanitizeObject(obj) {
        if (!obj || typeof obj !== 'object') return {};
        
        const sanitized = {};
        
        // ES6+ Object.entries for iteration
        for (const [key, value] of Object.entries(obj)) {
            if (typeof value === 'string') {
                sanitized[key] = this.sanitizeString(value);
            } else if (typeof value === 'object' && value !== null) {
                sanitized[key] = this.sanitizeObject(value);
            } else {
                sanitized[key] = value;
            }
        }
        
        return sanitized;
    }

    // Validate and sanitize user input for registration
    static sanitizeUserRegistration(userData) {
        return {
            name: this.sanitizeString(userData.name),
            email: this.sanitizeEmail(userData.email),
            phone: userData.phone ? this.sanitizePhone(userData.phone) : '',
            bio: userData.bio ? this.sanitizeString(userData.bio) : ''
        };
    }

    // Validate and sanitize search queries
    static sanitizeSearchQuery(query) {
        if (!query || typeof query !== 'string') return '';
        
        // Remove potential SQL injection patterns
        const dangerous = /(\b(SELECT|INSERT|UPDATE|DELETE|DROP|CREATE|ALTER|EXEC|UNION|SCRIPT)\b)/gi;
        let sanitized = query.replace(dangerous, '');
        
        return this.sanitizeString(sanitized);
    }
}

// Express middleware for request sanitization
const sanitizeMiddleware = (req, res, next) => {
    try {
        // Sanitize request body
        if (req.body && typeof req.body === 'object') {
            req.body = Sanitizer.sanitizeObject(req.body);
        }
        
        // Sanitize query parameters
        if (req.query && typeof req.query === 'object') {
            req.query = Sanitizer.sanitizeObject(req.query);
        }
        
        next();
    } catch (error) {
        console.error('Sanitization error:', error);
        res.status(400).json({ error: 'Invalid input data' });
    }
};

module.exports = {
    Sanitizer,
    sanitizeMiddleware
};
```

---

## server/security/encryption.js

```javascript
const crypto = require('crypto');
const bcrypt = require('bcryptjs');

// ES6+ class for encryption utilities
class Encryption {
    // Generate salt for password hashing
    static async generateSalt(rounds = 12) {
        return await bcrypt.genSalt(rounds);
    }

    // Hash password with salt
    static async hashPassword(password, salt = null) {
        if (!salt) {
            salt = await this.generateSalt();
        }
        return await bcrypt.hash(password, salt);
    }

    // Compare password with hash
    static async comparePassword(password, hash) {
        return await bcrypt.compare(password, hash);
    }

    // Generate random token
    static generateRandomToken(length = 32) {
        return crypto.randomBytes(length).toString('hex');
    }

    // Generate secure session ID
    static generateSessionId() {
        return crypto.randomBytes(32).toString('hex');
    }

    // Encrypt sensitive data
    static encrypt(text, key = process.env.ENCRYPTION_KEY) {
        if (!key) throw new Error('Encryption key is required');
        
        const iv = crypto.randomBytes(16);
        const cipher = crypto.createCipher('aes-256-cbc', key);
        
        let encrypted = cipher.update(text, 'utf8', 'hex');
        encrypted += cipher.final('hex');
        
        return iv.toString('hex') + ':' + encrypted;
    }

    // Decrypt sensitive data
    static decrypt(encryptedText, key = process.env.ENCRYPTION_KEY) {
        if (!key) throw new Error('Encryption key is required');
        
        const parts = encryptedText.split(':');
        const iv = Buffer.from(parts[0], 'hex');
        const encrypted = parts[1];
        
        const decipher = crypto.createDecipher('aes-256-cbc', key);
        
        let decrypted = decipher.update(encrypted, 'hex', 'utf8');
        decrypted += decipher.final('utf8');
        
        return decrypted;
    }

    // Generate CSRF token
    static generateCSRFToken() {
        return crypto.randomBytes(24).toString('hex');
    }

    // Validate CSRF token
    static validateCSRFToken(token, sessionToken) {
        return token === sessionToken;
    }
}

module.exports = Encryption;
```

---

## server/app.js

```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const session = require('express-session');
const MongoStore = require('connect-mongo');
const passport = require('passport');
const path = require('path');
require('dotenv').config();

// Import middleware
const { apiLimiter } = require('./security/rateLimiter');
const { sanitizeMiddleware } = require('./security/sanitizer');
const errorHandler = require('./middleware/errorHandler');
const logger = require('./middleware/logger');

// Import routes
const authRoutes = require('./routes/authRoutes');
const userRoutes = require('./routes/userRoutes');
const apiRoutes = require('./routes/apiRoutes');
const viewRoutes = require('./routes/viewRoutes');

// Import configurations
require('./config/passport');

const app = express();

// Security middleware
app.use(helmet()); // Set security headers
app.use(cors({
    origin: process.env.FRONTEND_URL || 'http://localhost:3000',
    credentials: true
}));

// Rate limiting
app.use('/api', apiLimiter);

// Body parsing middleware
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true, limit: '10mb' }));

// Input sanitization
app.use(sanitizeMiddleware);

// Session configuration
app.use(session({
    secret: process.env.SESSION_SECRET || 'your-secret-key',
    resave: false,
    saveUninitialized: false,
    store: MongoStore.create({
        mongoUrl: process.env.MONGODB_URI || 'mongodb://localhost:27017/fullstack_app'
    }),
    cookie: {
        secure: process.env.NODE_ENV === 'production', // HTTPS in production
        httpOnly: true,
        maxAge: 24 * 60 * 60 * 1000 // 24 hours
    }
}));

// Passport middleware
app.use(passport.initialize());
app.use(passport.session());

// Static files
app.use(express.static(path.join(__dirname, '../client/build')));

// View engine setup (EJS)
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// Request logging
app.use(logger);

// API Routes
app.use('/api/auth', authRoutes);
app.use('/api/users', userRoutes);
app.use('/api', apiRoutes);

// View Routes (for server-side rendering)
app.use('/', viewRoutes);

// Serve React app for client-side routing
app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../client/build/index.html'));
});

// Error handling middleware
app.use(errorHandler);

module.exports = app;
```

---

## server/server.js

```javascript
const app = require('./app');
const connectDB = require('./config/database');

// Load environment variables
require('dotenv').config();

const PORT = process.env.PORT || 5000;

// Connect to databases
connectDB();

// Start server
const server = app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
    console.log(`Environment: ${process.env.NODE_ENV || 'development'}`);
});

// Graceful shutdown
process.on('SIGTERM', () => {
    console.log('SIGTERM received. Shutting down gracefully...');
    server.close(() => {
        console.log('Process terminated');
    });
});

process.on('SIGINT', () => {
    console.log('SIGINT received. Shutting down gracefully...');
    server.close(() => {
        console.log('Process terminated');
    });
});

module.exports = server;
```

---

## server/package.json

```json
{
  "name": "fullstack-webapp-server",
  "version": "1.0.0",
  "description": "Full-stack web application backend",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "jest",
    "test:watch": "jest --watch"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.3",
    "mysql2": "^3.2.0",
    "sequelize": "^6.30.0",
    "bcryptjs": "^2.4.3",
    "jsonwebtoken": "^9.0.0",
    "passport": "^0.6.0",
    "passport-local": "^1.0.0",
    "passport-google-oauth20": "^2.0.0",
    "passport-jwt": "^4.0.1",
    "express-session": "^1.17.3",
    "connect-mongo": "^5.0.0",
    "express-rate-limit": "^6.7.0",
    "rate-limit-redis": "^3.0.1",
    "redis": "^4.6.5",
    "cors": "^2.8.5",
    "helmet": "^6.1.5",
    "express-validator": "^6.15.0",
    "validator": "^13.9.0",
    "isomorphic-dompurify": "^2.0.0",
    "dotenv": "^16.0.3",
    "ejs": "^3.1.9",
    "nodemailer": "^6.9.1",
    "crypto": "^1.0.1",
    "winston": "^3.8.2"
  },
  "devDependencies": {
    "nodemon": "^2.0.22",
    "jest": "^29.5.0",
    "supertest": "^6.3.3"
  },
  "keywords": [
    "fullstack",
    "nodejs",
    "express",
    "mongodb",
    "mysql",
    "authentication",
    "jwt",
    "oauth"
  ],
  "author": "Your Name",
  "license": "MIT"
}
```

---

## database/migrations/001_create_users_table.sql

```sql
-- MySQL migration for users table
-- Run this migration first

CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255), -- Nullable for OAuth users
    phone VARCHAR(20),
    bio TEXT,
    avatar VARCHAR(500),
    role ENUM('user', 'admin') DEFAULT 'user',
    provider ENUM('local', 'google') DEFAULT 'local',
    provider_id VARCHAR(255), -- For OAuth providers
    email_verified BOOLEAN DEFAULT FALSE,
    email_verification_token VARCHAR(255),
    password_reset_token VARCHAR(255),
    password_reset_expires DATETIME,
    login_count INT DEFAULT 0,
    last_login DATETIME,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Indexes for performance
    INDEX idx_email (email),
    INDEX idx_provider (provider, provider_id),
    INDEX idx_email_verification (email_verification_token),
    INDEX idx_password_reset (password_reset_token),
    INDEX idx_created_at (created_at)
);
```

---

## database/migrations/002_create_sessions_table.sql

```sql
-- MySQL migration for sessions table
-- Run this migration second

CREATE TABLE IF NOT EXISTS sessions (
    session_id VARCHAR(128) PRIMARY KEY,
    user_id INT,
    expires DATETIME NOT NULL,
    data TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Foreign key constraint
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    
    -- Indexes
    INDEX idx_user_id (user_id),
    INDEX idx_expires (expires)
);
```

---

## database/migrations/003_create_refresh_tokens_table.sql

```sql
-- MySQL migration for refresh tokens table
-- Run this migration third

CREATE TABLE IF NOT EXISTS refresh_tokens (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    token VARCHAR(255) UNIQUE NOT NULL,
    expires_at DATETIME NOT NULL,
    is_revoked BOOLEAN DEFAULT FALSE,
    replaced_by_token VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Foreign key constraint
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    
    -- Indexes
    INDEX idx_user_id (user_id),
    INDEX idx_token (token),
    INDEX idx_expires_at (expires_at),
    INDEX idx_is_revoked (is_revoked)
);
```

---

## database/seeders/users_seeder.sql

```sql
-- Seed data for users table
-- Default admin user (password: admin123)

INSERT INTO users (
    name, 
    email, 
    password, 
    role, 
    email_verified, 
    is_active
) VALUES (
    'Admin User',
    'admin@example.com',
    '$2a$12$LQv3c1yqBWVHxkd0LHAkCOYz6TtxMQJqhN8/LewgYGZLWgUON3ZWO', -- bcrypt hash of 'admin123'
    'admin',
    TRUE,
    TRUE
);

-- Default regular user (password: user123)
INSERT INTO users (
    name, 
    email, 
    password, 
    role, 
    email_verified, 
    is_active
) VALUES (
    'Demo User',
    'user@example.com',
    '$2a$12$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', -- bcrypt hash of 'user123'
    'user',
    TRUE,
    TRUE
);
```

---

## database/seeders/default_data.sql

```sql
-- Additional seed data for the application

-- Insert sample data for testing
INSERT INTO users (name, email, password, role, email_verified, is_active, bio) VALUES
('John Doe', 'john@example.com', '$2a$12$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', 'user', TRUE, TRUE, 'Software developer passionate about full-stack development'),
('Jane Smith', 'jane@example.com', '$2a$12$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', 'user', TRUE, TRUE, 'UI/UX designer with a love for creating beautiful interfaces'),
('Bob Johnson', 'bob@example.com', '$2a$12$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', 'user', FALSE, TRUE, 'DevOps engineer focused on cloud infrastructure');

-- Note: All sample passwords are 'user123' for testing purposes
-- In production, users should set their own secure passwords
```

---

## database/init.sql

```sql
-- Database initialization script
-- Run this script to set up the database

-- Create database
CREATE DATABASE IF NOT EXISTS fullstack_app;
USE fullstack_app;

-- Source all migration files
SOURCE 001_create_users_table.sql;
SOURCE 002_create_sessions_table.sql;
SOURCE 003_create_refresh_tokens_table.sql;

-- Source seed data
SOURCE seeders/users_seeder.sql;
SOURCE seeders/default_data.sql;

-- Create database user (optional)
-- CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'secure_password';
-- GRANT SELECT, INSERT, UPDATE, DELETE ON fullstack_app.* TO 'app_user'@'localhost';
-- FLUSH PRIVILEGES;

-- Display success message
SELECT 'Database initialization completed successfully!' AS message;
```

---

## docs/API.md

```markdown
# API Documentation

## Authentication Endpoints

### POST /api/auth/register
Register a new user account.

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "User registered successfully",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

### POST /api/auth/login
Login with email and password.

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "token": "jwt_token_here",
  "refreshToken": "refresh_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com"
  }
}


### GET /api/auth/google
Initiate Google OAuth login.

### POST /api/auth/refresh
Refresh JWT token using refresh token.

**Request Body:**
```json
{
  "refreshToken": "refresh_token_here"
}

### POST /api/auth/logout
Logout user and invalidate tokens.

## User Endpoints

### GET /api/users/profile
Get current user profile (requires authentication).

**Headers:**

Authorization: Bearer jwt_token_here
```

**Response:**
```json
{
  "success": true,
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+1234567890",
    "bio": "User bio here"
  }
}

### PUT /api/users/profile
Update user profile (requires authentication).

**Request Body:**
```json
{
  "name": "John Smith",
  "phone": "+1234567890",
  "bio": "Updated bio"
}

## Dashboard Endpoints

### GET /api/dashboard/stats
Get dashboard statistics (requires authentication).

**Response:**
```json
{
  "success": true,
  "stats": {
    "totalUsers": 150,
    "activeSessions": 25,
    "loginCount": 1250
  }
}

## Error Responses

All endpoints return errors in the following format:

```json
{
  "success": false,
  "message": "Error message here",
  "errors": ["Detailed error 1", "Detailed error 2"]
}

## Rate Limiting

- Authentication endpoints: 5 requests per 15 minutes
- General API endpoints: 100 requests per 15 minutes
- File upload endpoints: 10 requests per hour

## Status Codes

- 200: Success
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error
```
---

## docs/SETUP.md

```markdown
# Setup Guide

## Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- MongoDB (local or cloud instance)
- MySQL (v8.0 or higher)
- Git

## Installation Steps

### 1. Clone the Repository

```bash
git clone <repository-url>
cd fullstack-webapp
```

### 2. Backend Setup

```bash
cd server
npm install
```

### 3. Frontend Setup

```bash
cd client
npm install
```

### 4. Environment Configuration

Copy `.env.example` to `.env` and configure:

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

```env
# Server Configuration
NODE_ENV=development
PORT=5000
CLIENT_URL=http://localhost:3000

# Database Configuration
MONGODB_URI=mongodb://localhost:27017/fullstack_app
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_password
MYSQL_DATABASE=fullstack_app

# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRE=7d
JWT_REFRESH_SECRET=your_refresh_token_secret
JWT_REFRESH_EXPIRE=30d

# OAuth Configuration
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret

# Email Configuration (for password reset)
EMAIL_FROM=noreply@yourdomain.com
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password

# Security
SESSION_SECRET=your_session_secret
BCRYPT_ROUNDS=12
```

### 5. Database Setup

#### MongoDB Setup
Make sure MongoDB is running locally or use a cloud service like MongoDB Atlas.

#### MySQL Setup
1. Create database:
```sql
CREATE DATABASE fullstack_app;
```

2. Run migrations:
```bash
cd server
npm run migrate
```

3. Seed database (optional):
```bash
npm run seed
```

### 6. Running the Application

#### Development Mode

Terminal 1 (Backend):
```bash
cd server
npm run dev
```

Terminal 2 (Frontend):
```bash
cd client
npm start
```

#### Production Mode

Build frontend:
```bash
cd client
npm run build
```

Start production server:
```bash
cd server
npm start


### 7. Verify Installation

- Frontend: http://localhost:3000
- Backend API: http://localhost:5000/api
- Health check: http://localhost:5000/api/health

## Troubleshooting

### Common Issues

1. **Port already in use**
   - Change PORT in .env file
   - Kill process: `lsof -ti:5000 | xargs kill -9`

2. **Database connection failed**
   - Verify MongoDB/MySQL is running
   - Check connection strings in .env

3. **Module not found errors**
   - Delete node_modules and package-lock.json
   - Run `npm install` again

4. **CORS errors**
   - Verify CLIENT_URL in backend .env
   - Check CORS configuration in app.js

### Development Tips

- Use `npm run dev` for auto-restart on changes
- Check browser console for frontend errors
- Check server logs for backend errors
- Use MongoDB Compass for database inspection
- Use Postman for API testing
```

---

## docs/DEPLOYMENT.md

```markdown
# Deployment Guide

## Production Deployment

### Option 1: Docker Deployment

#### Prerequisites
- Docker and Docker Compose installed
- Domain name (optional)
- SSL certificate (for HTTPS)

#### Steps

1. **Build and run with Docker Compose:**
```bash
docker-compose up -d --build
```

2. **Environment Configuration:**
Update `docker-compose.yml` environment variables for production.

3. **SSL Setup (with Nginx):**
```yaml
# Add to docker-compose.yml
nginx:
  image: nginx:alpine
  ports:
    - "80:80"
    - "443:443"
  volumes:
    - ./nginx.conf:/etc/nginx/nginx.conf
    - ./ssl:/etc/ssl/certs
```

### Option 2: Manual Deployment

#### Server Requirements
- Ubuntu 20.04+ or CentOS 8+
- Node.js 18+
- MongoDB and MySQL
- Nginx (reverse proxy)
- PM2 (process manager)

#### Steps

1. **Server Setup:**
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2
sudo npm install -g pm2

# Install Nginx
sudo apt install nginx -y
```

2. **Application Deployment:**
```bash
# Clone repository
git clone <your-repo>
cd fullstack-webapp

# Install dependencies
cd server && npm install --production
cd ../client && npm install && npm run build

# Copy built files to server directory
sudo cp -r build/* /var/www/html/
```

3. **PM2 Configuration:**
```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'fullstack-app',
    script: './server/server.js',
    instances: 'max',
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'development'
    },
    env_production: {
      NODE_ENV: 'production',
      PORT: 5000
    }
  }]
};
```

4. **Start Application:**
```bash
pm2 start ecosystem.config.js --env production
pm2 save
pm2 startup
```

5. **Nginx Configuration:**
```nginx
# /etc/nginx/sites-available/fullstack-app
server {
    listen 80;
    server_name yourdomain.com;

    # Frontend
    location / {
        root /var/www/html;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

6. **Enable Site:**
```bash
sudo ln -s /etc/nginx/sites-available/fullstack-app /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### Option 3: Cloud Deployment

#### Heroku Deployment

1. **Prepare for Heroku:**
```json
// Add to server/package.json
{
  "scripts": {
    "heroku-postbuild": "cd ../client && npm install && npm run build"
  }
}
```

2. **Deploy:**
```bash
heroku create your-app-name
heroku config:set NODE_ENV=production
heroku config:set MONGODB_URI=your_mongodb_uri
git push heroku main
```

#### AWS/DigitalOcean

1. **Create server instance**
2. **Follow manual deployment steps**
3. **Configure security groups/firewall**
4. **Set up load balancer (optional)**

### Database Deployment

#### MongoDB Atlas (Cloud)
1. Create cluster at mongodb.com
2. Get connection string
3. Update MONGODB_URI in production env

#### MySQL Cloud Options
- AWS RDS
- Google Cloud SQL
- DigitalOcean Managed Databases

### Security Checklist

- [ ] Environment variables secured
- [ ] SSL certificate installed
- [ ] Firewall configured
- [ ] Database access restricted
- [ ] Rate limiting enabled
- [ ] Security headers configured
- [ ] CORS properly configured
- [ ] Sensitive data encrypted
- [ ] Regular backups scheduled
- [ ] Monitoring setup

### Monitoring

#### PM2 Monitoring
```bash
pm2 monit
pm2 logs
```

#### Health Checks
```bash
# Add health check endpoint
curl https://yourdomain.com/api/health
```

### Backup Strategy

#### Database Backups
```bash
# MongoDB
mongodump --uri="mongodb://..." --out=/backup/mongo/

# MySQL
mysqldump -u user -p database > backup.sql
```

#### Application Backups
```bash
# Automated backup script
#!/bin/bash
tar -czf /backup/app-$(date +%Y%m%d).tar.gz /path/to/app
```

---

## .env.example

```env
# Server Configuration
NODE_ENV=development
PORT=5000
CLIENT_URL=http://localhost:3000

# Database Configuration - MongoDB
MONGODB_URI=mongodb://localhost:27017/fullstack_app
MONGODB_TEST_URI=mongodb://localhost:27017/fullstack_app_test

# Database Configuration - MySQL
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_mysql_password
MYSQL_DATABASE=fullstack_app
MYSQL_TEST_DATABASE=fullstack_app_test

# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key_here_make_it_long_and_secure
JWT_EXPIRE=7d
JWT_REFRESH_SECRET=your_refresh_token_secret_different_from_jwt_secret
JWT_REFRESH_EXPIRE=30d

# Session Configuration
SESSION_SECRET=your_session_secret_key_for_express_sessions

# OAuth Configuration - Google
GOOGLE_CLIENT_ID=your_google_oauth_client_id
GOOGLE_CLIENT_SECRET=your_google_oauth_client_secret
GOOGLE_CALLBACK_URL=http://localhost:5000/auth/google/callback

# Email Configuration (for password reset, notifications)
EMAIL_FROM=noreply@yourdomain.com
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_email_app_password

# Security Configuration
BCRYPT_ROUNDS=12
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
AUTH_RATE_LIMIT_MAX=5

# CORS Configuration
CORS_ORIGIN=http://localhost:3000
CORS_CREDENTIALS=true

# File Upload Configuration
MAX_FILE_SIZE=5242880
UPLOAD_PATH=./uploads
ALLOWED_FILE_TYPES=jpg,jpeg,png,gif,pdf,doc,docx

# Redis Configuration (optional - for session store)
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Logging Configuration
LOG_LEVEL=info
LOG_FILE=./logs/app.log

# API Configuration
API_VERSION=v1
API_PREFIX=/api

# Frontend Configuration (for server-side rendering)
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_GOOGLE_CLIENT_ID=your_google_oauth_client_id
```

---

## .gitignore

```gitignore
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment variables
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Logs
logs/
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
lerna-debug.log*

# Runtime data
pids/
*.pid
*.seed
*.pid.lock

# Coverage directory used by tools like istanbul
coverage/
*.lcov

# nyc test coverage
.nyc_output/

# node-waf configuration
.lock-wscript

# Compiled binary addons
build/Release/

# Dependency directories
jspm_packages/

# Optional npm cache directory
.npm

# Optional eslint cache
.eslintcache

# Optional REPL history
.node_repl_history

# Output of 'npm pack'
*.tgz

# Yarn Integrity file
.yarn-integrity

# parcel-bundler cache
.cache
.parcel-cache

# next.js build output
.next

# nuxt.js build output
.nuxt

# vuepress build output
.vuepress/dist

# Serverless directories
.serverless

# FuseBox cache
.fusebox/

# DynamoDB Local files
.dynamodb/

# TernJS port file
.tern-port

# React build files
client/build/
build/

# Uploads directory
uploads/
public/uploads/

# Database files
*.sqlite
*.db

# IDE files
.vscode/
.idea/
*.swp
*.swo
*~

# OS generated files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Temporary files
tmp/
temp/

# SSL certificates
*.pem
*.key
*.crt

# Docker
.dockerignore

# Testing
coverage/
.nyc_output/

# Storybook
storybook-static/

# TypeScript
*.tsbuildinfo

# ESLint
.eslintcache

# Stylelint
.stylelintcache

# Microbundle cache
.rpt2_cache/
.rts2_cache_cjs/
.rts2_cache_es/
.rts2_cache_umd/

# Optional REPL history
.node_repl_history

# Output of 'npm pack'
*.tgz

# Yarn Integrity file
.yarn-integrity

# dotenv environment variables file
.env.test

# parcel-bundler cache (https://parceljs.org/)
.cache
.parcel-cache

# Next.js build output
.next

# Nuxt.js build / generate output
.nuxt
dist

# Storybook build outputs
.out
.storybook-out

# Temporary folders
tmp/
temp/

# Editor directories and files
.vscode/
!.vscode/extensions.json
.idea
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?
```

---

## docker-compose.yml

```yaml
version: '3.8'

services:
  # Frontend Service
  client:
    build:
      context: ./client
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - REACT_APP_API_URL=http://localhost:5000/api
      - REACT_APP_GOOGLE_CLIENT_ID=${GOOGLE_CLIENT_ID}
    volumes:
      - ./client:/app
      - /app/node_modules
    depends_on:
      - server
    networks:
      - fullstack-network

  # Backend Service
  server:
    build:
      context: ./server
      dockerfile: Dockerfile
    ports:
      - "5000:5000"
    environment:
      - NODE_ENV=development
      - PORT=5000
      - CLIENT_URL=http://localhost:3000
      - MONGODB_URI=mongodb://mongo:27017/fullstack_app
      - MYSQL_HOST=mysql
      - MYSQL_PORT=3306
      - MYSQL_USER=root
      - MYSQL_PASSWORD=rootpassword
      - MYSQL_DATABASE=fullstack_app
      - JWT_SECRET=${JWT_SECRET}
      - JWT_REFRESH_SECRET=${JWT_REFRESH_SECRET}
      - SESSION_SECRET=${SESSION_SECRET}
      - GOOGLE_CLIENT_ID=${GOOGLE_CLIENT_ID}
      - GOOGLE_CLIENT_SECRET=${GOOGLE_CLIENT_SECRET}
    volumes:
      - ./server:/app
      - /app/node_modules
      - uploads_data:/app/uploads
    depends_on:
      - mongo
      - mysql
    networks:
      - fullstack-network

  # MongoDB Service
  mongo:
    image: mongo:6.0
    container_name: fullstack_mongodb
    restart: unless-stopped
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=adminpassword
      - MONGO_INITDB_DATABASE=fullstack_app
    volumes:
      - mongo_data:/data/db
      - ./database/init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js:ro
    networks:
      - fullstack-network

  # MySQL Service
  mysql:
    image: mysql:8.0
    container_name: fullstack_mysql
    restart: unless-stopped
    ports:
      - "3306:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
      - MYSQL_DATABASE=fullstack_app
      - MYSQL_USER=appuser
      - MYSQL_PASSWORD=apppassword
    volumes:
      - mysql_data:/var/lib/mysql
      - ./database/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    networks:
      - fullstack-network

  # Redis Service (optional - for session storage)
  redis:
    image: redis:7-alpine
    container_name: fullstack_redis
    restart: unless-stopped
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - fullstack-network

  # Nginx Reverse Proxy
  nginx:
    image: nginx:alpine
    container_name: fullstack_nginx
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/ssl/certs:ro
    depends_on:
      - client
      - server
    networks:
      - fullstack-network

# Volumes for persistent data
volumes:
  mongo_data:
    driver: local
  mysql_data:
    driver: local
  redis_data:
    driver: local
  uploads_data:
    driver: local

# Networks
networks:
  fullstack-network:
    driver: bridge

# Development override (use with docker-compose -f docker-compose.yml -f docker-compose.dev.yml up)
# docker-compose.dev.yml would contain development-specific configurations
```

---

## Dockerfile

```dockerfile
# Multi-stage Dockerfile for production builds
FROM node:18-alpine AS base

# Set working directory
WORKDIR /app

# Install dependencies only when needed
FROM base AS deps
# Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine
RUN apk add --no-cache libc6-compat

# Copy package files
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force

# Development stage
FROM base AS dev
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "run", "dev"]

# Build stage for frontend
FROM base AS builder-client
WORKDIR /app
COPY client/package*.json ./
RUN npm ci
COPY client/ ./
RUN npm run build

# Build stage for backend
FROM base AS builder-server
WORKDIR /app
COPY server/package*.json ./
RUN npm ci --only=production
COPY server/ ./

# Production stage
FROM node:18-alpine AS production

# Create app directory
WORKDIR /app

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodeuser -u 1001

# Copy built application
COPY --from=builder-server --chown=nodeuser:nodejs /app ./
COPY --from=builder-client /app/build ./public

# Copy production dependencies
COPY --from=deps --chown=nodeuser:nodejs /app/node_modules ./node_modules

# Create uploads directory
RUN mkdir -p uploads && chown nodeuser:nodejs uploads

# Switch to non-root user
USER nodeuser

# Expose port
EXPOSE 5000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node health-check.js

# Start the application
CMD ["node", "server.js"]

# Alternative multi-service Dockerfile for full-stack
FROM node:18-alpine AS full-stack

WORKDIR /app

# Install dependencies for both client and server
COPY package*.json ./
COPY client/package*.json ./client/
COPY server/package*.json ./server/

# Install all dependencies
RUN npm install && \
    cd client && npm install && \
    cd ../server && npm install

# Copy source code
COPY . .

# Build client
RUN cd client && npm run build

# Expose ports
EXPOSE 3000 5000

# Start both services (for development only)
CMD ["npm", "run", "dev:full"]
```

---

## README.md

```markdown
# Full-Stack Web Application

A complete full-stack web application built with React, Node.js, Express, MongoDB, MySQL, and comprehensive authentication system including Google OAuth, JWT tokens, and session management.

## 🚀 Features

### Frontend
- **React 18** with modern hooks and functional components
- **Redux Toolkit** for state management
- **React Router** for client-side routing
- **Responsive design** with Bootstrap + Tailwind CSS
- **Authentication flows** (login, register, password reset)
- **Protected routes** and role-based access
- **Real-time updates** and notifications

### Backend
- **Node.js & Express.js** RESTful API
- **MVC architecture** with clean code separation
- **Dual database support** (MongoDB + MySQL)
- **Comprehensive authentication** (Local + Google OAuth)
- **JWT & Refresh tokens** with secure storage
- **Rate limiting** and security middleware
- **Input validation** and sanitization
- **Error handling** and logging

### Security Features
- Password hashing with bcrypt + salt
- JWT access and refresh token rotation
- Google OAuth 2.0 integration
- Rate limiting and DDoS protection
- CORS configuration
- Input sanitization
- Security headers with Helmet
- Session management

## 📋 Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- MongoDB (local or cloud)
- MySQL (v8.0 or higher)
- Git

## 🛠️ Installation

### 1. Clone Repository
```bash
git clone <repository-url>
cd fullstack-webapp
```

### 2. Install Dependencies
```bash
# Install server dependencies
cd server
npm install

# Install client dependencies
cd ../client
npm install
```

### 3. Environment Setup
```bash
# Copy environment template
cp .env.example .env

# Edit .env with your configuration
nano .env
```

### 4. Database Setup
```bash
# Start MongoDB (if local)
mongod

# Start MySQL and create database
mysql -u root -p
CREATE DATABASE fullstack_app;

# Run migrations
cd server
npm run migrate
```

### 5. Start Application
```bash
# Start backend (Terminal 1)
cd server
npm run dev

# Start frontend (Terminal 2)
cd client
npm start
```

Visit:
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000/api

## 🐳 Docker Setup

### Quick Start with Docker Compose
```bash
# Build and start all services
docker-compose up -d --build

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### Services Included
- **Frontend**: React app (port 3000)
- **Backend**: Node.js API (port 5000)
- **MongoDB**: Database (port 27017)
- **MySQL**: Database (port 3306)
- **Redis**: Session store (port 6379)
- **Nginx**: Reverse proxy (port 80/443)

## 📚 Project Structure

```
fullstack-webapp/
├── client/                     # React Frontend
│   ├── public/                # Static files
│   ├── src/
│   │   ├── components/        # React components
│   │   ├── pages/            # Page components
│   │   ├── hooks/            # Custom hooks
│   │   ├── services/         # API services
│   │   ├── store/            # Redux store
│   │   └── styles/           # CSS/Sass files
│   └── package.json
├── server/                     # Node.js Backend
│   ├── controllers/          # Route controllers
│   ├── models/               # Database models
│   ├── routes/               # API routes
│   ├── middleware/           # Custom middleware
│   ├── config/               # Configuration
│   ├── services/             # Business logic
│   └── utils/                # Utilities
├── database/                   # Database scripts
├── docs/                      # Documentation
├── docker-compose.yml         # Docker configuration
└── README.md
```

## 🔧 Available Scripts

### Server Scripts
```bash
npm run dev          # Start development server
npm start            # Start production server
npm run migrate      # Run database migrations
npm run seed         # Seed database with sample data
npm test             # Run tests
npm run lint         # Run ESLint
```

### Client Scripts
```bash
npm start            # Start development server
npm run build        # Build for production
npm test             # Run tests
npm run eject        # Eject from Create React App
```

## 🔐 Authentication

### Local Authentication
- Email/password registration and login
- Password hashing with bcrypt
- Email verification (optional)
- Password reset functionality

### Google OAuth
- One-click Google sign-in
- Automatic account creation
- Profile information sync

### JWT Tokens
- Access tokens (short-lived)
- Refresh tokens (long-lived)
- Automatic token rotation
- Secure HTTP-only cookies

## 🛡️ Security

### Implemented Security Measures
- **Rate Limiting**: Prevents brute force attacks
- **Input Validation**: Server-side validation with express-validator
- **SQL Injection Protection**: Parameterized queries
- **XSS Protection**: Input sanitization
- **CSRF Protection**: CSRF tokens for forms
- **Security Headers**: Helmet middleware
- **CORS Configuration**: Controlled cross-origin requests

## 📖 API Documentation

Detailed API documentation is available in [`docs/API.md`](docs/API.md).

### Quick API Overview
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `GET /api/auth/google` - Google OAuth
- `GET /api/users/profile` - Get user profile
- `PUT /api/users/profile` - Update profile
- `GET /api/dashboard/stats` - Dashboard data

## 🚀 Deployment

### Production Deployment Options

1. **Docker Deployment** (Recommended)
   ```bash
   docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
   ```

2. **Manual Deployment**
   - See [`docs/DEPLOYMENT.md`](docs/DEPLOYMENT.md) for detailed instructions

3. **Cloud Platforms**
   - Heroku
   - AWS EC2/ECS
   - DigitalOcean
   - Google Cloud Platform

## 🧪 Testing

### Running Tests
```bash
# Backend tests
cd server
npm test

# Frontend tests
cd client
npm test

# E2E tests
npm run test:e2e
```

### Test Coverage
- Unit tests for utilities and services
- Integration tests for API endpoints
- Component tests for React components
- E2E tests for user workflows

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request


## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Troubleshooting

### Common Issues

1. **Port already in use**
   ```bash
   # Kill process using port 3000 (frontend)
   sudo lsof -t -i tcp:3000 | xargs kill -9
   
   # Kill process using port 5000 (backend)
   sudo lsof -t -i tcp:5000 | xargs kill -9
   
   # Or change ports in .env file
   REACT_APP_PORT=3001
   SERVER_PORT=5001
   ```

2. **Database connection errors**
   ```bash
   # MongoDB connection issues
   - Check if MongoDB is running: sudo systemctl status mongod
   - Verify connection string in .env
   - Ensure database exists: use fullstack_app
   
   # MySQL connection issues
   - Check MySQL service: sudo systemctl status mysql
   - Verify credentials in .env
   - Create database: CREATE DATABASE fullstack_app;
   ```

3. **JWT Token errors**
   ```bash
   # Clear browser localStorage/cookies
   localStorage.clear();
   
   # Regenerate JWT secrets
   JWT_SECRET=your_new_strong_secret_key_here
   JWT_REFRESH_SECRET=your_new_refresh_secret_key_here
   ```

4. **Google OAuth not working**
   ```bash
   # Check Google Console settings
   - Verify redirect URIs: http://localhost:3000/auth/google/callback
   - Ensure OAuth credentials are correct in .env
   - Check domain verification
   ```

5. **CORS errors**
   ```bash
   # Update cors configuration in server/app.js
   app.use(cors({
     origin: process.env.CLIENT_URL || 'http://localhost:3000',
     credentials: true
   }));
   ```

6. **Package installation issues**
   ```bash
   # Clear npm cache
   npm cache clean --force
   
   # Delete node_modules and reinstall
   rm -rf node_modules package-lock.json
   npm install
   
   # Use specific Node version
   nvm use 18.16.0
   ```

### Environment Variables Issues

1. **Missing .env file**
   ```bash
   cp .env.example .env
   # Fill in all required variables
   ```

2. **Invalid environment variables**
   ```bash
   # Check required variables are set
   echo $JWT_SECRET
   echo $MONGODB_URI
   echo $MYSQL_HOST
   ```

### Development Issues

1. **Hot reload not working**
   ```bash
   # Frontend
   echo "FAST_REFRESH=false" >> .env
   
   # Backend - use nodemon
   npm install -g nodemon
   nodemon server.js
   ```

2. **Build errors**
   ```bash
   # Frontend build
   npm run build
   
   # Check for TypeScript errors
   npm run type-check
   
   # Backend production
   NODE_ENV=production npm start
   ```

### Performance Issues

1. **Slow API responses**
   ```bash
   # Add database indexes
   db.users.createIndex({ email: 1 })
   
   # Enable query logging
   DEBUG=app:* npm start
   ```

2. **Memory leaks**
   ```bash
   # Monitor memory usage
   node --inspect server.js
   
   # Use PM2 for production
   pm2 start server.js --name "fullstack-app"
   ```

## 🔧 Quick Fixes

### Reset Application State
```bash
# Full reset script
#!/bin/bash
echo "Resetting FullStack App..."

# Stop all processes
pkill -f "node"
pkill -f "react-scripts"

# Clear databases
mongo fullstack_app --eval "db.dropDatabase()"
mysql -u root -p -e "DROP DATABASE IF EXISTS fullstack_app; CREATE DATABASE fullstack_app;"

# Clear caches
rm -rf client/node_modules server/node_modules
rm -rf client/build
npm cache clean --force

# Reinstall dependencies
cd client && npm install
cd ../server && npm install

echo "Reset complete! Run 'npm run dev' to start."
```

### Health Check Script
```bash
#!/bin/bash
echo "Health Check - FullStack App"

# Check ports
echo "Checking ports..."
lsof -i :3000 && echo "✅ Frontend running" || echo "❌ Frontend not running"
lsof -i :5000 && echo "✅ Backend running" || echo "❌ Backend not running"

# Check databases
echo "Checking databases..."
mongo --eval "db.runCommand('ping')" > /dev/null && echo "✅ MongoDB connected" || echo "❌ MongoDB not connected"
mysql -u root -p -e "SELECT 1" > /dev/null && echo "✅ MySQL connected" || echo "❌ MySQL not connected"

# Check environment
echo "Checking environment..."
[ -f .env ] && echo "✅ .env file exists" || echo "❌ .env file missing"
[ -n "$JWT_SECRET" ] && echo "✅ JWT_SECRET set" || echo "❌ JWT_SECRET missing"

echo "Health check complete!"
```

## 📞 Support

### Getting Help

1. **Check the logs**
   ```bash
   # Backend logs
   tail -f server/logs/app.log
   
   # Frontend console
   Open browser DevTools → Console
   ```

2. **Community Support**
   - Create an issue on GitHub
   - Check existing issues and discussions
   - Provide error logs and environment details

3. **Documentation**
   - [`docs/SETUP.md`](docs/SETUP.md) - Setup instructions
   - [`docs/API.md`](docs/API.md) - API documentation
   - [`docs/DEPLOYMENT.md`](docs/DEPLOYMENT.md) - Deployment guide

### Reporting Bugs

When reporting bugs, please include:
- Operating system and version
- Node.js version (`node --version`)
- npm version (`npm --version`)
- Browser version (for frontend issues)
- Complete error messages
- Steps to reproduce

### Feature Requests

For feature requests:
- Describe the feature clearly
- Explain the use case
- Provide examples if possible
- Check if similar features exist

## 🚀 Performance Tips

### Frontend Optimization
```javascript
// Lazy loading components
const Dashboard = React.lazy(() => import('./components/Dashboard/Dashboard'));

// Memoization for expensive operations
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

// Virtual scrolling for large lists
import { FixedSizeList as List } from 'react-window';
```

### Backend Optimization
```javascript
// Database connection pooling
mongoose.connect(uri, {
  maxPoolSize: 10,
  bufferMaxEntries: 0
});

// Caching with Redis
const redis = require('redis');
const client = redis.createClient();

// Compression middleware
app.use(compression());
```

## 🔒 Security Best Practices

### Production Security Checklist
- [ ] Change all default passwords
- [ ] Use strong JWT secrets
- [ ] Enable HTTPS only
- [ ] Configure proper CORS
- [ ] Set up rate limiting
- [ ] Enable security headers
- [ ] Validate all inputs
- [ ] Use parameterized queries
- [ ] Implement proper logging
- [ ] Regular security updates

### Monitoring & Logging
```javascript
// Production logging
const winston = require('winston');
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});
```

---

## 🎉 Congratulations!

You now have a complete full-stack web application with:
- ✅ Modern React frontend with Redux
- ✅ Secure Node.js backend with Express
- ✅ Multiple database support (MongoDB & MySQL)
- ✅ JWT and OAuth authentication
- ✅ Comprehensive security measures
- ✅ Production-ready deployment options

### Next Steps
1. Customize the styling and branding
2. Add your specific business logic
3. Implement additional features
4. Deploy to production
5. Set up monitoring and analytics

### Happy Coding! 🚀

Remember to star ⭐ the repository if you find it helpful!

---

*Built with ❤️ using modern web technologies*
