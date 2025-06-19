# Complete Flutter MVC Pattern App - Comprehensive Guide

## Project Overview
This is a complete Flutter application following the MVC (Model-View-Controller) pattern. The app demonstrates a task management system with user authentication, data persistence, networking, state management, and all essential Flutter concepts.

**Why MVC Pattern?**
- **Separation of Concerns**: Each layer has a specific responsibility
- **Maintainability**: Code is organized and easy to modify
- **Testability**: Each component can be tested independently
- **Scalability**: Easy to add new features without affecting existing code

**Without MVC**: All code would be mixed together, making it hard to debug, test, and maintain.

---

## File Structure

```
flutter_mvc_app/
├── lib/
│   ├── main.dart                     # 1. App entry point
│   ├── models/                       # Data models (M in MVC)
│   │   ├── user_model.dart          # 2. User data structure
│   │   ├── task_model.dart          # 3. Task data structure
│   │   └── api_response_model.dart  # 4. API response wrapper
│   ├── views/                        # UI components (V in MVC)
│   │   ├── screens/                 # Full screen widgets
│   │   │   ├── splash_screen.dart   # 5. App loading screen
│   │   │   ├── login_screen.dart    # 6. User authentication
│   │   │   ├── home_screen.dart     # 7. Main dashboard
│   │   │   └── task_detail_screen.dart # 8. Task details
│   │   ├── widgets/                 # Reusable UI components
│   │   │   ├── custom_button.dart   # 9. Reusable button
│   │   │   ├── custom_textfield.dart # 10. Reusable input field
│   │   │   ├── task_card.dart       # 11. Task display card
│   │   │   └── loading_widget.dart  # 12. Loading indicator
│   │   └── themes/                  # UI styling
│   │       └── app_theme.dart       # 13. App-wide theme
│   ├── controllers/                  # Business logic (C in MVC)
│   │   ├── auth_controller.dart     # 14. Authentication logic
│   │   ├── task_controller.dart     # 15. Task management logic
│   │   └── theme_controller.dart    # 16. Theme management
│   ├── services/                     # External services
│   │   ├── api_service.dart         # 17. HTTP requests
│   │   ├── database_service.dart    # 18. Local database
│   │   ├── storage_service.dart     # 19. Local storage
│   │   └── notification_service.dart # 20. Push notifications
│   ├── utils/                        # Utility functions
│   │   ├── constants.dart           # 21. App constants
│   │   ├── validators.dart          # 22. Input validation
│   │   ├── helpers.dart             # 23. Helper functions
│   │   └── routes.dart              # 24. Navigation routes
│   └── config/                       # Configuration
│       └── app_config.dart          # 25. App configuration
├── pubspec.yaml                      # 26. Dependencies
```

---

## 1. lib/main.dart
**Purpose**: App entry point, initializes app and sets up global configurations
**Problem without it**: No starting point for the application

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:flutter/services.dart';
import 'controllers/auth_controller.dart';
import 'controllers/task_controller.dart';
import 'controllers/theme_controller.dart';
import 'services/database_service.dart';
import 'services/notification_service.dart';
import 'utils/routes.dart';
import 'views/themes/app_theme.dart';
import 'config/app_config.dart';

// Main function - Entry point of the application
void main() async {
  // Ensures Flutter framework is initialized before running app
  WidgetsFlutterBinding.ensureInitialized();
  
  // Initialize app configuration
  await AppConfig.initialize();
  
  // Initialize local database
  await DatabaseService.initialize();
  
  // Initialize notification service
  await NotificationService.initialize();
  
  // Set preferred device orientations
  await SystemChrome.setPreferredOrientations([
    DeviceOrientation.portraitUp,
    DeviceOrientation.portraitDown,
  ]);
  
  // Run the app
  runApp(MyApp());
}

// Root widget of the application
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // MultiProvider wraps the app to provide global state management
    return MultiProvider(
      providers: [
        // Authentication state provider
        ChangeNotifierProvider(create: (_) => AuthController()),
        
        // Task management state provider
        ChangeNotifierProvider(create: (_) => TaskController()),
        
        // Theme management state provider
        ChangeNotifierProvider(create: (_) => ThemeController()),
      ],
      child: Consumer<ThemeController>(
        builder: (context, themeController, child) {
          return MaterialApp.router(
            title: 'Flutter MVC Demo',
            debugShowCheckedModeBanner: false,
            theme: AppTheme.lightTheme,
            darkTheme: AppTheme.darkTheme,
            themeMode: themeController.themeMode,
            routerConfig: AppRouter.router,
          );
        },
      ),
    );
  }
}
```

---

## 2. lib/models/user_model.dart
**Purpose**: User data structure and serialization methods
**Problem without it**: No structured way to handle user data, prone to errors

```dart
// User data model
// This represents the user entity in our application
class UserModel {
  final String id;           // Unique user identifier
  final String name;         // User's full name
  final String email;        // User's email address
  final String? avatar;      // Optional profile picture URL
  final DateTime createdAt;  // Account creation date
  final DateTime updatedAt;  // Last update date
  
  // Constructor with required and optional parameters
  UserModel({
    required this.id,
    required this.name,
    required this.email,
    this.avatar,
    required this.createdAt,
    required this.updatedAt,
  });
  
  // Factory constructor to create UserModel from JSON
  // Problem without this: Manual parsing leads to errors
  factory UserModel.fromJson(Map<String, dynamic> json) {
    return UserModel(
      id: json['id'] ?? '',
      name: json['name'] ?? '',
      email: json['email'] ?? '',
      avatar: json['avatar'],
      createdAt: DateTime.parse(json['created_at'] ?? DateTime.now().toIso8601String()),
      updatedAt: DateTime.parse(json['updated_at'] ?? DateTime.now().toIso8601String()),
    );
  }
  
  // Method to convert UserModel to JSON
  // Problem without this: Can't send data to API
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'email': email,
      'avatar': avatar,
      'created_at': createdAt.toIso8601String(),
      'updated_at': updatedAt.toIso8601String(),
    };
  }
  
  // Copy method for creating modified instances
  // Problem without this: Hard to update user data immutably
  UserModel copyWith({
    String? id,
    String? name,
    String? email,
    String? avatar,
    DateTime? createdAt,
    DateTime? updatedAt,
  }) {
    return UserModel(
      id: id ?? this.id,
      name: name ?? this.name,
      email: email ?? this.email,
      avatar: avatar ?? this.avatar,
      createdAt: createdAt ?? this.createdAt,
      updatedAt: updatedAt ?? this.updatedAt,
    );
  }
  
  // Override toString for debugging
  @override
  String toString() {
    return 'UserModel(id: $id, name: $name, email: $email)';
  }
  
  // Override equality operator
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is UserModel && other.id == id;
  }
  
  // Override hashCode
  @override
  int get hashCode => id.hashCode;
}
```

---

## 3. lib/models/task_model.dart
**Purpose**: Task data structure and business logic
**Problem without it**: No consistent way to handle task data

```dart
import 'package:uuid/uuid.dart';

// Task priority levels
enum TaskPriority { low, medium, high, urgent }

// Task status types
enum TaskStatus { pending, inProgress, completed, cancelled }

// Task data model
class TaskModel {
  final String id;              // Unique task identifier
  final String title;           // Task title
  final String description;     // Task description
  final TaskPriority priority;  // Task priority level
  final TaskStatus status;      // Current task status
  final DateTime createdAt;     // Creation timestamp
  final DateTime updatedAt;     // Last update timestamp
  final DateTime? dueDate;      // Optional due date
  final String userId;          // Owner user ID
  
  // Constructor
  TaskModel({
    required this.id,
    required this.title,
    required this.description,
    required this.priority,
    required this.status,
    required this.createdAt,
    required this.updatedAt,
    this.dueDate,
    required this.userId,
  });
  
  // Factory constructor for creating new tasks
  // Problem without this: Complex task creation logic scattered
  factory TaskModel.create({
    required String title,
    required String description,
    required String userId,
    TaskPriority priority = TaskPriority.medium,
    DateTime? dueDate,
  }) {
    final now = DateTime.now();
    return TaskModel(
      id: Uuid().v4(),
      title: title,
      description: description,
      priority: priority,
      status: TaskStatus.pending,
      createdAt: now,
      updatedAt: now,
      dueDate: dueDate,
      userId: userId,
    );
  }
  
  // Create from JSON
  factory TaskModel.fromJson(Map<String, dynamic> json) {
    return TaskModel(
      id: json['id'] ?? '',
      title: json['title'] ?? '',
      description: json['description'] ?? '',
      priority: _parsePriority(json['priority']),
      status: _parseStatus(json['status']),
      createdAt: DateTime.parse(json['created_at'] ?? DateTime.now().toIso8601String()),
      updatedAt: DateTime.parse(json['updated_at'] ?? DateTime.now().toIso8601String()),
      dueDate: json['due_date'] != null ? DateTime.parse(json['due_date']) : null,
      userId: json['user_id'] ?? '',
    );
  }
  
  // Convert to JSON
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'title': title,
      'description': description,
      'priority': priority.name,
      'status': status.name,
      'created_at': createdAt.toIso8601String(),
      'updated_at': updatedAt.toIso8601String(),
      'due_date': dueDate?.toIso8601String(),
      'user_id': userId,
    };
  }
  
  // Helper method to parse priority from string
  static TaskPriority _parsePriority(String? priority) {
    switch (priority?.toLowerCase()) {
      case 'low': return TaskPriority.low;
      case 'medium': return TaskPriority.medium;
      case 'high': return TaskPriority.high;
      case 'urgent': return TaskPriority.urgent;
      default: return TaskPriority.medium;
    }
  }
  
  // Helper method to parse status from string
  static TaskStatus _parseStatus(String? status) {
    switch (status?.toLowerCase()) {
      case 'pending': return TaskStatus.pending;
      case 'in_progress': return TaskStatus.inProgress;
      case 'completed': return TaskStatus.completed;
      case 'cancelled': return TaskStatus.cancelled;
      default: return TaskStatus.pending;
    }
  }
  
  // Copy with modifications
  TaskModel copyWith({
    String? title,
    String? description,
    TaskPriority? priority,
    TaskStatus? status,
    DateTime? dueDate,
    DateTime? updatedAt,
  }) {
    return TaskModel(
      id: id,
      title: title ?? this.title,
      description: description ?? this.description,
      priority: priority ?? this.priority,
      status: status ?? this.status,
      createdAt: createdAt,
      updatedAt: updatedAt ?? DateTime.now(),
      dueDate: dueDate ?? this.dueDate,
      userId: userId,
    );
  }
  
  // Check if task is overdue
  bool get isOverdue {
    if (dueDate == null) return false;
    return DateTime.now().isAfter(dueDate!) && status != TaskStatus.completed;
  }
  
  // Get priority color
  String get priorityColor {
    switch (priority) {
      case TaskPriority.low: return '#4CAF50';      // Green
      case TaskPriority.medium: return '#FF9800';   // Orange
      case TaskPriority.high: return '#F44336';     // Red
      case TaskPriority.urgent: return '#9C27B0';   // Purple
    }
  }
  
  @override
  String toString() {
    return 'TaskModel(id: $id, title: $title, status: $status)';
  }
  
  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is TaskModel && other.id == id;
  }
  
  @override
  int get hashCode => id.hashCode;
}
```

---

## 4. lib/models/api_response_model.dart
**Purpose**: Standardized API response wrapper
**Problem without it**: Inconsistent API response handling

```dart
// Generic API response wrapper
// This standardizes how we handle all API responses
class ApiResponse<T> {
  final bool success;        // Whether the request was successful
  final String message;      // Response message
  final T? data;            // Response data (generic type)
  final int? statusCode;    // HTTP status code
  final Map<String, dynamic>? errors; // Validation errors
  
  // Constructor
  ApiResponse({
    required this.success,
    required this.message,
    this.data,
    this.statusCode,
    this.errors,
  });
  
  // Factory constructor for successful responses
  // Problem without this: Repetitive success response creation
  factory ApiResponse.success({
    required String message,
    T? data,
    int? statusCode,
  }) {
    return ApiResponse<T>(
      success: true,
      message: message,
      data: data,
      statusCode: statusCode ?? 200,
    );
  }
  
  // Factory constructor for error responses
  // Problem without this: Inconsistent error handling
  factory ApiResponse.error({
    required String message,
    int? statusCode,
    Map<String, dynamic>? errors,
  }) {
    return ApiResponse<T>(
      success: false,
      message: message,
      statusCode: statusCode ?? 400,
      errors: errors,
    );
  }
  
  // Create from JSON response
  factory ApiResponse.fromJson(
    Map<String, dynamic> json,
    T Function(Map<String, dynamic>)? fromJsonT,
  ) {
    return ApiResponse<T>(
      success: json['success'] ?? false,
      message: json['message'] ?? '',
      data: json['data'] != null && fromJsonT != null 
          ? fromJsonT(json['data']) 
          : json['data'],
      statusCode: json['status_code'],
      errors: json['errors'],
    );
  }
  
  // Convert to JSON
  Map<String, dynamic> toJson() {
    return {
      'success': success,
      'message': message,
      'data': data,
      'status_code': statusCode,
      'errors': errors,
    };
  }
  
  // Check if response has validation errors
  bool get hasErrors => errors != null && errors!.isNotEmpty;
  
  // Get first error message
  String? get firstError {
    if (!hasErrors) return null;
    final firstKey = errors!.keys.first;
    final firstValue = errors![firstKey];
    return firstValue is List ? firstValue.first : firstValue.toString();
  }
  
  @override
  String toString() {
    return 'ApiResponse(success: $success, message: $message, statusCode: $statusCode)';
  }
}

// Specialized response for paginated data
// Problem without this: No standard way to handle paginated responses
class PaginatedResponse<T> extends ApiResponse<List<T>> {
  final int currentPage;     // Current page number
  final int totalPages;      // Total number of pages
  final int totalItems;      // Total number of items
  final int itemsPerPage;    // Items per page
  final bool hasNextPage;    // Whether there's a next page
  final bool hasPreviousPage; // Whether there's a previous page
  
  PaginatedResponse({
    required bool success,
    required String message,
    List<T>? data,
    int? statusCode,
    required this.currentPage,
    required this.totalPages,
    required this.totalItems,
    required this.itemsPerPage,
    required this.hasNextPage,
    required this.hasPreviousPage,
  }) : super(
    success: success,
    message: message,
    data: data,
    statusCode: statusCode,
  );
  
  // Create from JSON
  factory PaginatedResponse.fromJson(
    Map<String, dynamic> json,
    T Function(Map<String, dynamic>) fromJsonT,
  ) {
    final List<dynamic> dataList = json['data'] ?? [];
    final List<T> items = dataList.map((item) => fromJsonT(item)).toList();
    
    return PaginatedResponse<T>(
      success: json['success'] ?? false,
      message: json['message'] ?? '',
      data: items,
      statusCode: json['status_code'],
      currentPage: json['current_page'] ?? 1,
      totalPages: json['total_pages'] ?? 1,
      totalItems: json['total_items'] ?? 0,
      itemsPerPage: json['items_per_page'] ?? 10,
      hasNextPage: json['has_next_page'] ?? false,
      hasPreviousPage: json['has_previous_page'] ?? false,
    );
  }
}
```

---

## 5. lib/views/screens/splash_screen.dart
**Purpose**: App loading screen with initialization
**Problem without it**: Jarring app startup experience

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:go_router/go_router.dart';
import '../../controllers/auth_controller.dart';
import '../../utils/constants.dart';

// Splash screen - First screen user sees when app launches
class SplashScreen extends StatefulWidget {
  const SplashScreen({Key? key}) : super(key: key);

  @override
  State<SplashScreen> createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen> 
    with SingleTickerProviderStateMixin {
  
  late AnimationController _animationController;
  late Animation<double> _fadeAnimation;
  late Animation<double> _scaleAnimation;
  
  @override
  void initState() {
    super.initState();
    
    // Initialize animations
    // Problem without animations: Static, boring splash screen
    _animationController = AnimationController(
      duration: AppConstants.longAnimation,
      vsync: this,
    );
    
    // Fade in animation
    _fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeInOut,
    ));
    
    // Scale animation
    _scaleAnimation = Tween<double>(
      begin: 0.5,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.elasticOut,
    ));
    
    // Start animations
    _animationController.forward();
    
    // Initialize app and navigate
    _initializeApp();
  }
  
  // Initialize app and check authentication
  Future<void> _initializeApp() async {
    try {
      // Wait for minimum splash duration
      // Problem without this: Splash disappears too quickly
      await Future.delayed(Duration(seconds: 2));
      
      // Check if user is logged in
      final authController = Provider.of<AuthController>(context, listen: false);
      await authController.checkAuthStatus();
      
      // Navigate based on auth status
      if (mounted) {
        if (authController.isAuthenticated) {
          context.go('/home'); // Go to home if authenticated
        } else {
          context.go('/login'); // Go to login if not authenticated
        }
      }
    } catch (e) {
      // Handle initialization errors
      print('Error initializing app: $e');
      if (mounted) {
        context.go('/login'); // Fallback to login screen
      }
    }
  }
  
  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Theme.of(context).primaryColor,
      body: Center(
        child: AnimatedBuilder(
          animation: _animationController,
          builder: (context, child) {
            return FadeTransition(
              opacity: _fadeAnimation,
              child: ScaleTransition(
                scale: _scaleAnimation,
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    // App logo
                    Container(
                      width: 120,
                      height: 120,
                      decoration: BoxDecoration(
                        color: Colors.white,
                        borderRadius: BorderRadius.circular(60),
                        boxShadow: [
                          BoxShadow(
                            color: Colors.black26,
                            blurRadius: 20,
                            offset: Offset(0, 10),
                          ),
                        ],
                      ),
                      child: Icon(
                        Icons.task_alt,
                        size: 60,
                        color: Theme.of(context).primaryColor,
                      ),
                    ),
                    
                    SizedBox(height: 24),
                    
                    // App name
                    Text(
                      'TaskMaster',
                      style: TextStyle(
                        fontSize: 32,
                        fontWeight: FontWeight.bold,
                        color: Colors.white,
                      ),
                    ),
                    
                    SizedBox(height: 8),
                    
                    // App tagline
                    Text(
                      'Organize your tasks efficiently',
                      style: TextStyle(
                        fontSize: 16,
                        color: Colors.white70,
                      ),
                    ),
                    
                    SizedBox(height: 48),
                    
                    // Loading indicator
                    CircularProgressIndicator(
                      valueColor: AlwaysStoppedAnimation<Color>(Colors.white),
                      strokeWidth: 3,
                    ),
                  ],
                ),
              ),
            );
          },
        ),
      ),
    );
  }
}
```

---

## 6. lib/views/screens/login_screen.dart
**Purpose**: User authentication interface
**Problem without it**: No way for users to access the app securely

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:go_router/go_router.dart';
import '../../controllers/auth_controller.dart';
import '../../utils/validators.dart';
import '../../utils/helpers.dart';
import '../widgets/custom_button.dart';
import '../widgets/custom_textfield.dart';
import '../widgets/loading_widget.dart';

// Login screen for user authentication
class LoginScreen extends StatefulWidget {
  const LoginScreen({Key? key}) : super(key: key);

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  // Form key for validation
  final _formKey = GlobalKey<FormState>();
  
  // Text controllers for input fields
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  
  // Password visibility toggle
  bool _obscurePassword = true;
  
  // Form submission state
  bool _isLoading = false;
  
  @override
  void dispose() {
    // Clean up controllers to prevent memory leaks
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }
  
  // Handle login form submission
  Future<void> _handleLogin() async {
    // Validate form first
    if (!_formKey.currentState!.validate()) {
      return;
    }
    
    setState(() => _isLoading = true);
    
    try {
      // Get auth controller
      final authController = Provider.of<AuthController>(context, listen: false);
      
      // Attempt login
      final success = await authController.login(
        email: _emailController.text.trim(),
        password: _passwordController.text,
      );
      
      if (success && mounted) {
        // Show success message
        AppHelpers.showSnackBar(context, 'Login successful!');
        
        // Navigate to home screen
        context.go('/home');
      } else if (mounted) {
        // Show error message
        AppHelpers.showSnackBar(
          context, 
          authController.errorMessage ?? 'Login failed',
          isError: true,
        );
      }
    } catch (e) {
      if (mounted) {
        AppHelpers.showSnackBar(
          context, 
          'An error occurred. Please try again.',
          isError: true,
        );
      }
    } finally {
      if (mounted) {
        setState(() => _isLoading = false);
      }
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: SingleChildScrollView(
          padding: EdgeInsets.all(24),
          child: Form(
            key: _formKey,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                SizedBox(height: 60),
                
                // App logo and title
                Column(
                  children: [
                    Container(
                      width: 80,
                      height: 80,
                      decoration: BoxDecoration(
                        color: Theme.of(context).primaryColor,
                        borderRadius: BorderRadius.circular(40),
                      ),
                      child: Icon(
                        Icons.task_alt,
                        size: 40,
                        color: Colors.white,
                      ),
                    ),
                    
                    SizedBox(height: 16),
                    
                    Text(
                      'Welcome Back',
                      style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    
                    SizedBox(height: 8),
                    
                    Text(
                      'Sign in to continue',
                      style: Theme.of(context).textTheme.bodyLarge?.copyWith(
                        color: Colors.grey,
                      ),
                    ),
                  ],
                ),
                
                SizedBox(height: 48),
                
                // Email field
                CustomTextField(
                  controller: _emailController,
                  label: 'Email',
                  hintText: 'Enter your email',
                  keyboardType: TextInputType.emailAddress,
                  prefixIcon: Icons.email_outlined,
                  validator: Validators.validateEmail,
                ),
                
                SizedBox(height: 16),
                
                // Password field
                CustomTextField(
                  controller: _passwordController,
                  label: 'Password',
                  hintText: 'Enter your password',
                  obscureText: _obscurePassword,
                  prefixIcon: Icons.lock_outline,
                  suffixIcon: IconButton(
                    icon: Icon(
                      _obscurePassword ? Icons.visibility : Icons.visibility_off,
                    ),
                    onPressed: () {
                      setState(() => _obscurePassword = !_obscurePassword);
                    },
                  ),
                  validator: Validators.validatePassword,
                ),
                
                SizedBox(height: 24),
                
                // Login button
                _isLoading
                    ? LoadingWidget()
                    : CustomButton(
                        text: 'Login',
                        onPressed: _handleLogin,
                      ),
                
                SizedBox(height: 16),
                
                // Forgot password link
                TextButton(
                  onPressed: () {
                    // Handle forgot password
                    AppHelpers.showSnackBar(
                      context, 
                      'Forgot password feature coming soon!',
                    );
                  },
                  child: Text('Forgot Password?'),
                ),
                
                SizedBox(height: 24),
                
                // Register link
                Row(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    Text('Don\'t have an account? '),
                    TextButton(
                      onPressed: () {
                        // Handle register navigation
                        AppHelpers.showSnackBar(
                          context, 
                          'Registration feature coming soon!',
                        );
                      },
                      child: Text('Sign Up'),
                    ),
                  ],
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

---

## 7. lib/views/screens/home_screen.dart
**Purpose**: Main dashboard showing user's tasks
**Problem without it**: No central place to view and manage tasks

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:go_router/go_router.dart';
import '../../controllers/auth_controller.dart';
import '../../controllers/task_controller.dart';
import '../../controllers/theme_controller.dart';
import '../../models/task_model.dart';
import '../../utils/helpers.dart';
import '../widgets/task_card.dart';
import '../widgets/loading_widget.dart';

// Home screen - Main dashboard
class HomeScreen extends StatefulWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> with TickerProviderStateMixin {
  late TabController _tabController;
  
  @override
  void initState() {
    super.initState();
    