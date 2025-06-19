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

