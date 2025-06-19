# Complete Flutter MVC Application - Production Ready Structure

## Project Structure Overview

```
lib/
├── main.dart                           # App entry point
├── app/
│   ├── app.dart                       # Main app configuration
│   └── routes/
│       ├── app_routes.dart           # Route definitions
│       └── route_generator.dart      # Route generation logic
├── core/
│   ├── constants/
│   │   ├── app_constants.dart        # App-wide constants
│   │   ├── api_constants.dart        # API endpoints
│   │   └── theme_constants.dart      # Theme configuration
│   ├── utils/
│   │   ├── validators.dart           # Input validation utilities
│   │   ├── formatters.dart          # Data formatting utilities
│   │   └── extensions.dart          # Dart extensions
│   └── services/
│       ├── api_service.dart         # HTTP service layer
│       ├── storage_service.dart     # Local storage service
│       └── navigation_service.dart   # Navigation service
├── data/
│   ├── models/
│   │   ├── user_model.dart          # User data model
│   │   ├── product_model.dart       # Product data model
│   │   └── api_response_model.dart  # API response wrapper
│   ├── repositories/
│   │   ├── user_repository.dart     # User data repository
│   │   └── product_repository.dart  # Product data repository
│   └── providers/
│       ├── user_provider.dart       # User state provider
│       ├── product_provider.dart    # Product state provider
│       └── theme_provider.dart      # Theme state provider
├── presentation/
│   ├── controllers/
│   │   ├── home_controller.dart     # Home screen controller
│   │   ├── profile_controller.dart  # Profile screen controller
│   │   └── product_controller.dart  # Product screen controller
│   ├── views/
│   │   ├── screens/
│   │   │   ├── home_screen.dart     # Home screen view
│   │   │   ├── profile_screen.dart  # Profile screen view
│   │   │   ├── product_screen.dart  # Product screen view
│   │   │   └── splash_screen.dart   # Splash screen view
│   │   └── widgets/
│   │       ├── common/
│   │       │   ├── custom_button.dart    # Reusable button widget
│   │       │   ├── custom_text_field.dart # Reusable text field
│   │       │   ├── loading_widget.dart   # Loading indicator
│   │       │   └── error_widget.dart     # Error display widget
│   │       └── specific/
│   │           ├── user_card.dart        # User-specific card widget
│   │           └── product_card.dart     # Product-specific card widget
│   └── themes/
│       ├── app_theme.dart           # Main theme configuration
│       ├── light_theme.dart         # Light theme
│       └── dark_theme.dart          # Dark theme
└── shared/
    ├── enums/
    │   ├── app_state.dart           # Application state enums
    │   └── user_role.dart           # User role enums
    └── mixins/
        └── validation_mixin.dart    # Validation mixin
```

---

## File Implementation

### 1. main.dart
**Purpose**: Application entry point that initializes providers and starts the app
**Benefits**: Clean separation of app initialization from configuration, enables dependency injection setup

```dart
// Entry point of the Flutter application
// Initializes all providers and starts the app with proper configuration
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

// Import app configuration
import 'app/app.dart';

// Import providers for state management
import 'data/providers/user_provider.dart';
import 'data/providers/product_provider.dart';
import 'data/providers/theme_provider.dart';

// Import services
import 'core/services/api_service.dart';
import 'core/services/storage_service.dart';
import 'core/services/navigation_service.dart';

// Import repositories
import 'data/repositories/user_repository.dart';
import 'data/repositories/product_repository.dart';

void main() async {
  // Ensure Flutter binding is initialized before running app
  WidgetsFlutterBinding.ensureInitialized();
  
  // Initialize core services
  final apiService = ApiService();
  final storageService = StorageService();
  final navigationService = NavigationService();
  
  // Initialize repositories with dependency injection
  final userRepository = UserRepository(apiService: apiService);
  final productRepository = ProductRepository(apiService: apiService);
  
  // Run the app with all providers configured
  runApp(
    // MultiProvider enables dependency injection across the entire app
    MultiProvider(
      providers: [
        // Service providers - singletons for the entire app lifecycle
        Provider<ApiService>.value(value: apiService),
        Provider<StorageService>.value(value: storageService),
        Provider<NavigationService>.value(value: navigationService),
        
        // Repository providers - data access layer
        Provider<UserRepository>.value(value: userRepository),
        Provider<ProductRepository>.value(value: productRepository),
        
        // State providers - manage app state with ChangeNotifier
        ChangeNotifierProvider<UserProvider>(
          create: (_) => UserProvider(userRepository: userRepository),
        ),
        ChangeNotifierProvider<ProductProvider>(
          create: (_) => ProductProvider(productRepository: productRepository),
        ),
        ChangeNotifierProvider<ThemeProvider>(
          create: (_) => ThemeProvider(storageService: storageService),
        ),
      ],
      // MyApp widget contains the main app configuration
      child: const MyApp(),
    ),
  );
}
```

---

### 2. app/app.dart
**Purpose**: Main app configuration including theme, routing, and global settings
**Benefits**: Centralizes app-level configuration, makes theme and routing management easier

```dart
// Main application widget that configures the entire app
// Handles theme switching, routing, and global app settings
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

// Import route configuration
import 'routes/route_generator.dart';
import 'routes/app_routes.dart';

// Import theme configuration
import '../presentation/themes/app_theme.dart';

// Import providers for reactive state management
import '../data/providers/theme_provider.dart';

// Import services
import '../core/services/navigation_service.dart';

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // Consumer rebuilds when ThemeProvider state changes
    return Consumer<ThemeProvider>(
      builder: (context, themeProvider, child) {
        return MaterialApp(
          // App metadata
          title: 'Flutter MVC Demo',
          debugShowCheckedModeBanner: false,
          
          // Theme configuration - switches between light/dark automatically
          theme: AppTheme.lightTheme,
          darkTheme: AppTheme.darkTheme,
          themeMode: themeProvider.themeMode, // Reactive theme switching
          
          // Navigation configuration
          navigatorKey: Provider.of<NavigationService>(context, listen: false).navigatorKey,
          
          // Route configuration
          initialRoute: AppRoutes.splash, // Start with splash screen
          onGenerateRoute: RouteGenerator.generateRoute, // Handle route generation
          
          // Global app builder for additional configuration
          builder: (context, child) {
            return MediaQuery(
              // Ensure text doesn't scale beyond reasonable limits
              data: MediaQuery.of(context).copyWith(textScaleFactor: 1.0),
              child: child!,
            );
          },
        );
      },
    );
  }
}
```

---

### 3. app/routes/app_routes.dart
**Purpose**: Centralized route name definitions
**Benefits**: Prevents typos in route names, enables easy route refactoring, improves maintainability

```dart
// Centralized route definitions for the entire application
// Prevents typos and makes route management easier across the app
class AppRoutes {
  // Private constructor to prevent instantiation - this is a utility class
  AppRoutes._();
  
  // Route name constants - using static const for memory efficiency
  static const String splash = '/'; // Root route for splash screen
  static const String home = '/home'; // Main home screen
  static const String profile = '/profile'; // User profile screen
  static const String productList = '/products'; // Product listing screen
  static const String productDetail = '/product-detail'; // Individual product view
  
  // Settings and configuration routes
  static const String settings = '/settings'; // App settings screen
  static const String about = '/about'; // About app screen
  
  // Authentication routes (for future expansion)
  static const String login = '/login'; // User login screen
  static const String register = '/register'; // User registration screen
  
  // Error routes
  static const String notFound = '/404'; // 404 error page
  
  // Helper method to get all routes as a list (useful for debugging)
  static List<String> get allRoutes => [
    splash,
    home,
    profile,
    productList,
    productDetail,
    settings,
    about,
    login,
    register,
    notFound,
  ];
  
  // Helper method to check if a route exists
  static bool routeExists(String route) {
    return allRoutes.contains(route);
  }
}
```

---

### 4. app/routes/route_generator.dart
**Purpose**: Handles route generation and navigation logic
**Benefits**: Centralizes navigation logic, enables parameter passing, handles unknown routes gracefully

```dart
// Route generator that handles all app navigation and route creation
// Provides centralized routing logic with parameter support and error handling
import 'package:flutter/material.dart';

// Import route definitions
import 'app_routes.dart';

// Import screen widgets
import '../../presentation/views/screens/splash_screen.dart';
import '../../presentation/views/screens/home_screen.dart';
import '../../presentation/views/screens/profile_screen.dart';
import '../../presentation/views/screens/product_screen.dart';

// Import models for type-safe parameter passing
import '../../data/models/product_model.dart';

class RouteGenerator {
  // Private constructor - this is a utility class
  RouteGenerator._();
  
  // Main route generation method - called by MaterialApp
  static Route<dynamic> generateRoute(RouteSettings settings) {
    // Extract route name and arguments for processing
    final String routeName = settings.name ?? '';
    final dynamic arguments = settings.arguments;
    
    // Route matching with parameter support
    switch (routeName) {
      // Splash screen - app entry point
      case AppRoutes.splash:
        return _createRoute(
          page: const SplashScreen(),
          settings: settings,
          // No transition for splash screen
          transitionType: _TransitionType.fade,
        );
      
      // Home screen - main app screen
      case AppRoutes.home:
        return _createRoute(
          page: const HomeScreen(),
          settings: settings,
          transitionType: _TransitionType.slideFromRight,
        );
      
      // Profile screen - user profile management
      case AppRoutes.profile:
        return _createRoute(
          page: const ProfileScreen(),
          settings: settings,
          transitionType: _TransitionType.slideFromBottom,
        );
      
      // Product list screen - displays all products
      case AppRoutes.productList:
        return _createRoute(
          page: const ProductScreen(),
          settings: settings,
          transitionType: _TransitionType.slideFromRight,
        );
      
      // Product detail screen - individual product view
      case AppRoutes.productDetail:
        // Type-safe argument extraction
        if (arguments is ProductModel) {
          return _createRoute(
            page: ProductScreen(selectedProduct: arguments),
            settings: settings,
            transitionType: _TransitionType.slideFromRight,
          );
        }
        // Fallback to product list if invalid arguments
        return _createRoute(
          page: const ProductScreen(),
          settings: settings,
          transitionType: _TransitionType.slideFromRight,
        );
      
      // Unknown route handling - prevents app crashes
      default:
        return _createRoute(
          page: const _NotFoundScreen(),
          settings: settings,
          transitionType: _TransitionType.fade,
        );
    }
  }
  
  // Helper method to create routes with custom transitions
  static PageRoute _createRoute({
    required Widget page,
    required RouteSettings settings,
    _TransitionType transitionType = _TransitionType.slideFromRight,
  }) {
    switch (transitionType) {
      case _TransitionType.fade:
        return PageRouteBuilder(
          settings: settings,
          pageBuilder: (context, animation, secondaryAnimation) => page,
          transitionsBuilder: (context, animation, secondaryAnimation, child) {
            // Fade transition implementation
            return FadeTransition(opacity: animation, child: child);
          },
          transitionDuration: const Duration(milliseconds: 300),
        );
      
      case _TransitionType.slideFromRight:
        return PageRouteBuilder(
          settings: settings,
          pageBuilder: (context, animation, secondaryAnimation) => page,
          transitionsBuilder: (context, animation, secondaryAnimation, child) {
            // Slide from right transition
            const begin = Offset(1.0, 0.0);
            const end = Offset.zero;
            final tween = Tween(begin: begin, end: end);
            final offsetAnimation = animation.drive(tween);
            return SlideTransition(position: offsetAnimation, child: child);
          },
          transitionDuration: const Duration(milliseconds: 300),
        );
      
      case _TransitionType.slideFromBottom:
        return PageRouteBuilder(
          settings: settings,
          pageBuilder: (context, animation, secondaryAnimation) => page,
          transitionsBuilder: (context, animation, secondaryAnimation, child) {
            // Slide from bottom transition
            const begin = Offset(0.0, 1.0);
            const end = Offset.zero;
            final tween = Tween(begin: begin, end: end);
            final offsetAnimation = animation.drive(tween);
            return SlideTransition(position: offsetAnimation, child: child);
          },
          transitionDuration: const Duration(milliseconds: 300),
        );
      
      default:
        // Default Material page route
        return MaterialPageRoute(
          builder: (context) => page,
          settings: settings,
        );
    }
  }
}

// Enum for different transition types
enum _TransitionType {
  fade,
  slideFromRight,
  slideFromBottom,
}

// 404 Not Found screen for unknown routes
class _NotFoundScreen extends StatelessWidget {
  const _NotFoundScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Page Not Found'),
        centerTitle: true,
      ),
      body: const Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Error icon
            Icon(
              Icons.error_outline,
              size: 64,
              color: Colors.grey,
            ),
            SizedBox(height: 16),
            // Error message
            Text(
              '404 - Page Not Found',
              style: TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
                color: Colors.grey,
              ),
            ),
            SizedBox(height: 8),
            Text(
              'The requested page could not be found.',
              style: TextStyle(
                fontSize: 16,
                color: Colors.grey,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

### 5. core/constants/app_constants.dart
**Purpose**: Application-wide constants and configuration values
**Benefits**: Centralizes configuration, makes values easy to update, prevents magic numbers/strings

```dart
// Application-wide constants for consistent configuration
// Centralizes all app-level constants for easy maintenance and updates
class AppConstants {
  // Private constructor to prevent instantiation
  AppConstants._();
  
  // App Information
  static const String appName = 'Flutter MVC Demo';
  static const String appVersion = '1.0.0';
  static const String appDescription = 'A complete MVC Flutter application demo';
  
  // Network Configuration
  static const int connectionTimeout = 30000; // 30 seconds in milliseconds
  static const int receiveTimeout = 30000; // 30 seconds in milliseconds
  static const int maxRetryAttempts = 3; // Maximum retry attempts for failed requests
  
  // UI Constants
  static const double defaultPadding = 16.0; // Standard padding throughout app
  static const double smallPadding = 8.0; // Small padding for tight spaces
  static const double largePadding = 24.0; // Large padding for spacious layouts
  
  // Border radius values for consistent UI
  static const double defaultBorderRadius = 8.0;
  static const double smallBorderRadius = 4.0;
  static const double largeBorderRadius = 16.0;
  
  // Animation durations for consistent timing
  static const int shortAnimationDuration = 200; // Quick animations (milliseconds)
  static const int mediumAnimationDuration = 300; // Standard animations (milliseconds)
  static const int longAnimationDuration = 500; // Slow animations (milliseconds)
  
  // Text size constants
  static const double smallTextSize = 12.0;
  static const double normalTextSize = 14.0;
  static const double mediumTextSize = 16.0;
  static const double largeTextSize = 18.0;
  static const double titleTextSize = 20.0;
  static const double headingTextSize = 24.0;
  
  // Icon sizes for consistent iconography
  static const double smallIconSize = 16.0;
  static const double normalIconSize = 24.0;
  static const double largeIconSize = 32.0;
  static const double extraLargeIconSize = 48.0;
  
  // List and grid configuration
  static const int defaultPageSize = 20; // Number of items per page
  static const int maxCacheSize = 100; // Maximum items to cache
  static const double listItemHeight = 80.0; // Standard list item height
  
  // Storage keys for SharedPreferences
  static const String themeKey = 'app_theme_mode';
  static const String languageKey = 'app_language';
  static const String userDataKey = 'user_data';
  static const String firstLaunchKey = 'first_launch';
  
  // Validation constants
  static const int minPasswordLength = 8;
  static const int maxPasswordLength = 128;
  static const int minUsernameLength = 3;
  static const int maxUsernameLength = 50;
  
  // Image and file constants
  static const List<String> supportedImageFormats = ['jpg', 'jpeg', 'png', 'gif'];
  static const int maxImageSizeInMB = 5;
  static const double defaultImageQuality = 0.8;
  
  // Error messages for consistent user feedback
  static const String genericErrorMessage = 'Something went wrong. Please try again.';
  static const String networkErrorMessage = 'Please check your internet connection.';
  static const String timeoutErrorMessage = 'Request timed out. Please try again.';
  static const String notFoundErrorMessage = 'The requested resource was not found.';
  
  // Success messages
  static const String dataLoadedSuccessfully = 'Data loaded successfully';
  static const String dataSavedSuccessfully = 'Data saved successfully';
  static const String dataUpdatedSuccessfully = 'Data updated successfully';
  static const String dataDeletedSuccessfully = 'Data deleted successfully';
  
  // Placeholder text
  static const String noDataAvailable = 'No data available';
  static const String loadingText = 'Loading...';
  static const String searchHint = 'Search...';
  
  // Date format patterns
  static const String dateFormat = 'yyyy-MM-dd';
  static const String timeFormat = 'HH:mm:ss';
  static const String dateTimeFormat = 'yyyy-MM-dd HH:mm:ss';
  static const String displayDateFormat = 'MMM dd, yyyy';
  
  // Environment flags (useful for different build configurations)
  static const bool isDebugMode = true; // Set to false in production
  static const bool enableLogging = true; // Enable detailed logging
  static const bool enableCrashReporting = false; // Enable crash reporting in production
}
```

---

### 6. core/constants/api_constants.dart
**Purpose**: API endpoint definitions and HTTP configuration
**Benefits**: Centralizes API configuration, makes endpoint management easier, supports multiple environments

```dart
// API-related constants and endpoint definitions
// Centralizes all API configuration for easy maintenance and environment management
class ApiConstants {
  // Private constructor to prevent instantiation
  ApiConstants._();
  
  // Base URLs for different environments
  static const String _baseUrlDev = 'https://jsonplaceholder.typicode.com';
  static const String _baseUrlStaging = 'https://staging-api.example.com';
  static const String _baseUrlProd = 'https://api.example.com';
  
  // Current environment configuration
  static const Environment currentEnvironment = Environment.development;
  
  // Get base URL based on current environment
  static String get baseUrl {
    switch (currentEnvironment) {
      case Environment.development:
        return _baseUrlDev;
      case Environment.staging:
        return _baseUrlStaging;
      case Environment.production:
        return _baseUrlProd;
    }
  }
  
  // API version for versioned endpoints
  static const String apiVersion = 'v1';
  
  // Full API base URL with version
  static String get apiBaseUrl => '$baseUrl/$apiVersion';
  
  // User-related endpoints
  static const String usersEndpoint = '/users';
  static const String userProfileEndpoint = '/users/profile';
  static const String userUpdateEndpoint = '/users/update';
  static const String userDeleteEndpoint = '/users/delete';
  
  // Product-related endpoints
  static const String productsEndpoint = '/posts'; // Using posts as mock products
  static const String productDetailEndpoint = '/posts/{id}';
  static const String productSearchEndpoint = '/posts/search';
  static const String productCategoriesEndpoint = '/posts/categories';
  
  // Authentication endpoints (for future implementation)
  static const String loginEndpoint = '/auth/login';
  static const String registerEndpoint = '/auth/register';
  static const String logoutEndpoint = '/auth/logout';
  static const String refreshTokenEndpoint = '/auth/refresh';
  
  // File upload endpoints
  static const String imageUploadEndpoint = '/upload/image';
  static const String fileUploadEndpoint = '/upload/file';
  
  // HTTP headers
  static const Map<String, String> defaultHeaders = {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
  };
  
  // Authentication headers (add token dynamically)
  static Map<String, String> getAuthHeaders(String token) => {
    ...defaultHeaders,
    'Authorization': 'Bearer $token',
  };
  
  // HTTP status codes for consistent handling
  static const int statusOk = 200;
  static const int statusCreated = 201;
  static const int statusNoContent = 204;
  static const int statusBadRequest = 400;
  static const int statusUnauthorized = 401;
  static const int statusForbidden = 403;
  static const int statusNotFound = 404;
  static const int statusInternalServerError = 500;
  static const int statusServiceUnavailable = 503;
  
  // Request timeout configurations
  static const Duration connectTimeout = Duration(seconds: 30);
  static const Duration receiveTimeout = Duration(seconds: 30);
  static const Duration sendTimeout = Duration(seconds: 30);
  
  // Retry configuration
  static const int maxRetryAttempts = 3;
  static const Duration retryDelay = Duration(seconds: 2);
  
  // Pagination parameters
  static const String pageParameter = 'page';
  static const String limitParameter = 'limit';
  static const String sortParameter = 'sort';
  static const String orderParameter = 'order';
  
  // Default pagination values
  static const int defaultPageSize = 20;
  static const int maxPageSize = 100;
  
  // Cache configuration
  static const Duration cacheExpiration = Duration(minutes: 30);
  static const String cacheControlHeader = 'Cache-Control';
  static const String maxAgeDirective = 'max-age=1800'; // 30 minutes
  
  // Helper method to build endpoint URLs
  static String buildEndpoint(String endpoint, {Map<String, dynamic>? pathParams}) {
    String url = baseUrl + endpoint;
    
    // Replace path parameters if provided
    if (pathParams != null) {
      pathParams.forEach((key, value) {
        url = url.replaceAll('{$key}', value.toString());
      });
    }
    
    return url;
  }
  
  // Helper method to build query parameters
  static String buildQueryString(Map<String, dynamic> params) {
    if (params.isEmpty) return '';
    
    final queryParams = params.entries
        .where((entry) => entry.value != null)
        .map((entry) => '${entry.key}=${Uri.encodeComponent(entry.value.toString())}')
        .join('&');
    
    return queryParams.isNotEmpty ? '?$queryParams' : '';
  }
}

// Environment enumeration for different deployment environments
enum Environment {
  development,
  staging,
  production,
}

// Extension to get environment name as string
extension EnvironmentExtension on Environment {
  String get name {
    switch (this) {
      case Environment.development:
        return 'Development';
      case Environment.staging:
        return 'Staging';
      case Environment.production:
        return 'Production';
    }
  }
  
  bool get isProduction => this == Environment.production;
  bool get isDevelopment => this == Environment.development;
  bool get isStaging => this == Environment.staging;
}
```

---

### 7. core/constants/theme_constants.dart
**Purpose**: Theme-related constants and color definitions
**Benefits**: Centralizes design system, ensures consistent theming, supports multiple themes

```dart
// Theme-related constants for consistent design system
// Defines colors, typography, and other design tokens used throughout the app
import 'package:flutter/material.dart';

class ThemeConstants {
  // Private constructor to prevent instantiation
  ThemeConstants._();
  
  // Primary brand colors
  static const Color primaryColor = Color(0xFF2196F3); // Blue
  static const Color primaryColorLight = Color(0xFF64B5F6);
  static const Color primaryColorDark = Color(0xFF1976D2);
  
  // Secondary accent colors
  static const Color secondaryColor = Color(0xFF03DAC6); // Teal
  static const Color secondaryColorLight = Color(0xFF4FE8D6);
  static const Color secondaryColorDark = Color(0xFF018786);
  
  // Error colors
  static const Color errorColor = Color(0xFFB00020);
  static const Color errorColorLight = Color(0xFFEF5350);
  static const Color errorColorDark = Color(0xFFD32F2F);
  
  // Success colors
  static const Color successColor = Color(0xFF4CAF50);
  static const Color successColorLight = Color(0xFF81C784);
  static const Color successColorDark = Color(0xFF388E3C);
  
  // Warning colors
  static const Color warningColor = Color(0xFFFF9800);
  static const Color warningColorLight = Color(0xFFFFB74D);
  static const Color warningColorDark = Color(0xFFF57C00);
  
  // Information colors
  static const Color infoColor = Color(0xFF2196F3);
  static const Color infoColorLight = Color(0xFF64B5F6);
  static const Color infoColorDark = Color(0xFF1976D2);
  
  // Light theme colors
  static const Color lightBackground = Color(0xFFFAFAFA);
  static const Color lightSurface = Color(0xFFFFFFFF);
  static const Color lightOnBackground = Color(0xFF000000);
  static const Color lightOnSurface = Color(0xFF000000);
  static const Color lightOnPrimary = Color(0xFFFFFFFF);
  
  // Dark theme colors
  static const Color darkBackground = Color(0xFF121212);
  static const Color darkSurface = Color(0xFF1E1E1E);
  static const Color darkOnBackground = Color(0xFFFFFFFF);
  static const Color darkOnSurface = Color(0xFFFFFFFF);
  static const Color darkOnPrimary = Color(0xFFFFFFFF);
  
  // Grey scale colors for various UI elements
  static const Color grey50 = Color(0xFFFAFAFA);
  static const Color grey100 = Color(0xFFF5F5F5);
  static const Color grey200 = Color(0xFFEEEEEE);
  static const Color grey300 = Color(0xFFE0E0E0);
  static const Color grey400 = Color(0xFFBDBDBD);
  static const Color grey500 = Color(0xFF9E9E9E);
  static const Color grey600 = Color(0xFF757575);
  static const Color grey700 = Color(0xFF616161);
  static const Color grey800 = Color(0xFF424242);
  static const Color grey900 = Color(0xFF212121);
  
  // Text colors for different contexts
  static const Color textPrimary = Color(0xFF212121);
  static const Color textSecondary = Color(0xFF757575);
  static const Color textDisabled = Color(0xFFBDBDBD);
  static const Color textPrimaryDark = Color(0xFFFFFFFF);
  static const Color textSecondaryDark = Color(0xFFB3B3B3);
  static const Color textDisabledDark = Color(0xFF666666);
  
  // Divider and border colors
  static const Color dividerColor = Color(0xFFE0E0E0);
  static const Color dividerColorDark = Color(0xFF333333);
  static const Color borderColor = Color(0xFFE0E0E0);
  static const Color borderColorDark = Color(0xFF333333);
  
  // Shadow colors
  static const Color shadowColor = Color(0x1F000000);
  static const Color shadowColorDark = Color(0x3F000000);
  
  // Typography scale following Material Design guidelines
  static const TextStyle headline1 = TextStyle(
    fontSize: 96,
    fontWeight: FontWeight.w300,
    letterSpacing: -1.5,
  );
  
  static const TextStyle headline2 = TextStyle(
    fontSize: 60,
    fontWeight: FontWeight.w300,
    letterSpacing: -0.5,
  );
  
  static const TextStyle headline3 = TextStyle(
    fontSize: 48,
    fontWeight: FontWeight.w400,
    letterSpacing: 0,
  );
  
  static const TextStyle headline4 = TextStyle(
    fontSize: 34,
    fontWeight: FontWeight.w400,
    letterSpacing: 0.25,
  );
  
  static const TextStyle headline5 = TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.w400,
    letterSpacing: 0,
  );
  
  static const TextStyle headline6 = TextStyle(
    fontSize: 20,
    fontWeight: FontWeight.w500,
    letterSpacing: 0.15,
  );
  
  static const TextStyle subtitle1 = TextStyle(
    fontSize: 16,
    fontWeight: FontWeight.w400,
    letterSpacing: 0.15,
  );
  
  static const TextStyle subtitle2 = TextStyle(
    fontSize: 14,
    fontWeight: FontWeight.w500,
    letterSpacing: 0.1,
  );
  
  static const TextStyle body1 = TextStyle(
    fontSize: 16,
    fontWeight: FontWeight.w400,
    letterSpacing: 0.5,
  );
  
  static const TextStyle body2 = TextStyle(
    fontSize: 14,
    fontWeight: FontWeight.w400,
    letterSpacing: 0.25,
  );
  
  static const TextStyle button = TextStyle(
    fontSize: 14,
    fontWeight: FontWeight.w500,
    letterSpacing: 1.25,
  );
  
  static const TextStyle caption = TextStyle(
    fontSize: 12,
    fontWeight: FontWeight.w400,
    letterSpacing: 0.4,
  );
  
  static const TextStyle overline = TextStyle(
    fontSize: 10,
    fontWeight: FontWeight.w400,
    letterSpacing: 1.5,
  );
  
  // Elevation values for consistent depth
  static const double elevation0 = 0.0;
  static const double elevation1 = 1.0;
  static const double elevation2 = 2.0;
  static const double elevation3 = 3.0;
  static const double elevation4 = 4.0;
  static const double elevation6 = 6.0;
  static const double elevation8 = 8.0;
  static const double elevation12 = 12.0;
  static const double elevation16 = 16.0;
  static const double elevation24 = 24.0;
  
  // Border radius values for consistent shapes
  static const double radiusSmall = 4.0;
  static const double radiusMedium = 8.0;
  static const double radiusLarge = 12.0;
  static const double radiusExtraLarge = 16.0;
  static const double radiusCircular = 50.0;
  
  // Spacing values for consistent layout
  static const double spaceXSmall = 4.0;
  static const double spaceSmall = 8.0;
  static const double spaceMedium = 16.0;
  static const double spaceLarge = 24.0;
  static const double spaceXLarge = 32.0;
  static const double spaceXXLarge = 48.0;
  
  // Icon sizes for consistent iconography
  static const double iconSizeSmall = 16.0;
  static const double iconSizeMedium = 24.0;
  static const double iconSizeLarge = 32.0;
  static const double iconSizeXLarge = 48.0;
  
  // Button dimensions
  static const double buttonHeightSmall = 32.0;
  static const double buttonHeightMedium = 40.0;
  static const double buttonHeightLarge = 48.0;
  static const double buttonMinWidth = 64.0;
  
  // Input field dimensions
  static const double inputFieldHeight = 48.0;
  static const double inputFieldBorderWidth = 1.0;
  static const double inputFieldBorderWidthFocused = 2.0;
  
  // Card dimensions
  static const double cardElevation = 2.0;
  static const double cardBorderRadius = 8.0;
  
  // AppBar dimensions
  static const double appBarHeight = 56.0;
  static const double appBarElevation = 4.0;
  
  // Bottom navigation dimensions
  static const double bottomNavHeight = 56.0;
  static const double bottomNavElevation = 8.0;
  
  // Drawer dimensions
  static const double drawerWidth = 280.0;
  static const double drawerHeaderHeight = 160.0;
  
  // Animation durations for consistent timing
  static const Duration animationDurationFast = Duration(milliseconds: 150);
  static const Duration animationDurationMedium = Duration(milliseconds: 300);
  static const Duration animationDurationSlow = Duration(milliseconds: 500);
  
  // Transition curves for smooth animations
  static const Curve animationCurveEaseIn = Curves.easeIn;
  static const Curve animationCurveEaseOut = Curves.easeOut;
  static const Curve animationCurveEaseInOut = Curves.easeInOut;
  static const Curve animationCurveElastic = Curves.elasticOut;
  
  // Opacity values for different states
  static const double opacityDisabled = 0.38;
  static const double opacityMedium = 0.60;
  static const double opacityHigh = 0.87;
  static const double opacityFull = 1.0;
  
  // Z-index values for proper layering
  static const int zIndexBackground = 0;
  static const int zIndexContent = 1;
  static const int zIndexAppBar = 2;
  static const int zIndexFab = 3;
  static const int zIndexDrawer = 4;
  static const int zIndexModal = 5;
  static const int zIndexSnackbar = 6;
  static const int zIndexTooltip = 7;
}
```

---

### 8. core/utils/validators.dart
**Purpose**: Input validation utilities for forms and user input
**Benefits**: Centralizes validation logic, ensures consistent validation rules, improves code reusability

```dart
// Input validation utilities for consistent form validation across the app
// Provides reusable validation functions for common input types
import '../constants/app_constants.dart';

class Validators {
  // Private constructor to prevent instantiation
  Validators._();
  
  // Email validation using regex pattern
  static String? validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return 'Email is required';
    }
    
    // RFC 5322 compliant email regex pattern
    final emailRegex = RegExp(
      r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,},
    );
    
    if (!emailRegex.hasMatch(value)) {
      return 'Please enter a valid email address';
    }
    
    return null; // Valid email
  }
  
  // Password validation with multiple criteria
  static String? validatePassword(String? value) {
    if (value == null || value.isEmpty) {
      return 'Password is required';
    }
    
    if (value.length < AppConstants.minPasswordLength) {
      return 'Password must be at least ${AppConstants.minPasswordLength} characters';
    }
    
    if (value.length > AppConstants.maxPasswordLength) {
      return 'Password must not exceed ${AppConstants.maxPasswordLength} characters';
    }
    
    // Check for at least one uppercase letter
    if (!RegExp(r'[A-Z]').hasMatch(value)) {
      return 'Password must contain at least one uppercase letter';
    }
    
    // Check for at least one lowercase letter
    if (!RegExp(r'[a-z]').hasMatch(value)) {
      return 'Password must contain at least one lowercase letter';
    }
    
    // Check for at least one digit
    if (!RegExp(r'\d').hasMatch(value)) {
      return 'Password must contain at least one number';
    }
    
    // Check for at least one special character
    if (!RegExp(r'[!@#$%^&*(),.?":{}|<>]').hasMatch(value)) {
      return 'Password must contain at least one special character';
    }
    
    return null; // Valid password
  }
  
  // Username validation
  static String? validateUsername(String? value) {
    if (value == null || value.isEmpty) {
      return 'Username is required';
    }
    
    if (value.length < AppConstants.minUsernameLength) {
      return 'Username must be at least ${AppConstants.minUsernameLength} characters';
    }
    
    if (value.length > AppConstants.maxUsernameLength) {
      return 'Username must not exceed ${AppConstants.maxUsernameLength} characters';
    }
    
    // Check for valid characters (alphanumeric and underscore only)
    if (!RegExp(r'^[a-zA-Z0-9_]+).hasMatch(value)) {
      return 'Username can only contain letters, numbers, and underscores';
    }
    
    // Username cannot start with a number
    if (RegExp(r'^\d').hasMatch(value)) {
      return 'Username cannot start with a number';
    }
    
    return null; // Valid username
  }
  
  // Phone number validation
  static String? validatePhoneNumber(String? value) {
    if (value == null || value.isEmpty) {
      return 'Phone number is required';
    }
    
    // Remove all non-digit characters for validation
    final digitsOnly = value.replaceAll(RegExp(r'\D'), '');
    
    // Check minimum length (10 digits for most countries)
    if (digitsOnly.length < 10) {
      return 'Phone number must be at least 10 digits';
    }
    
    // Check maximum length (15 digits as per international standard)
    if (digitsOnly.length > 15) {
      return 'Phone number must not exceed 15 digits';
    }
    
    // Basic format validation (allows +, -, (), and spaces)
    if (!RegExp(r'^[\+]?[0-9\-\(\)\s]+).hasMatch(value)) {
      return 'Please enter a valid phone number';
    }
    
    return null; // Valid phone number
  }
  
  // Required field validation
  static String? validateRequired(String? value, {String fieldName = 'Field'}) {
    if (value == null || value.trim().isEmpty) {
      return '$fieldName is required';
    }
    return null; // Valid input
  }
  
  // Minimum length validation
  static String? validateMinLength(String? value, int minLength, {String fieldName = 'Field'}) {
    if (value == null || value.isEmpty) {
      return '$fieldName is required';
    }
    
    if (value.length < minLength) {
      return '$fieldName must be at least $minLength characters';
    }
    
    return null; // Valid length
  }
  
  // Maximum length validation
  static String? validateMaxLength(String? value, int maxLength, {String fieldName = 'Field'}) {
    if (value != null && value.length > maxLength) {
      return '$fieldName must not exceed $maxLength characters';
    }
    
    return null; // Valid length
  }
  
  // Numeric validation
  static String? validateNumeric(String? value, {String fieldName = 'Field'}) {
    if (value == null || value.isEmpty) {
      return '$fieldName is required';
    }
    
    if (double.tryParse(value) == null) {
      return '$fieldName must be a valid number';
    }
    
    return null; // Valid number
  }
  
  // Integer validation
  static String? validateInteger(String? value, {String fieldName = 'Field'}) {
    if (value == null || value.isEmpty) {
      return '$fieldName is required';
    }
    
    if (int.tryParse(value) == null) {
      return '$fieldName must be a valid integer';
    }
    
    return null; // Valid integer
  }
  
  // URL validation
  static String? validateUrl(String? value) {
    if (value == null || value.isEmpty) {
      return 'URL is required';
    }
    
    try {
      final uri = Uri.parse(value);
      if (!uri.hasScheme || (!uri.scheme.startsWith('http'))) {
        return 'Please enter a valid URL (starting with http or https)';
      }
    } catch (e) {
      return 'Please enter a valid URL';
    }
    
    return null; // Valid URL
  }
  
  // Date validation
  static String? validateDate(String? value) {
    if (value == null || value.isEmpty) {
      return 'Date is required';
    }
    
    try {
      DateTime.parse(value);
    } catch (e) {
      return 'Please enter a valid date (YYYY-MM-DD)';
    }
    
    return null; // Valid date
  }
  
  // Age validation
  static String? validateAge(String? value, {int minAge = 0, int maxAge = 150}) {
    if (value == null || value.isEmpty) {
      return 'Age is required';
    }
    
    final age = int.tryParse(value);
    if (age == null) {
      return 'Please enter a valid age';
    }
    
    if (age < minAge) {
      return 'Age must be at least $minAge';
    }
    
    if (age > maxAge) {
      return 'Age must not exceed $maxAge';
    }
    
    return null; // Valid age
  }
  
  // Confirm password validation
  static String? validateConfirmPassword(String? value, String? originalPassword) {
    if (value == null || value.isEmpty) {
      return 'Please confirm your password';
    }
    
    if (value != originalPassword) {
      return 'Passwords do not match';
    }
    
    return null; // Passwords match
  }
  
  // Credit card number validation (basic Luhn algorithm)
  static String? validateCreditCard(String? value) {
    if (value == null || value.isEmpty) {
      return 'Credit card number is required';
    }
    
    final digitsOnly = value.replaceAll(RegExp(r'\D'), '');
    
    if (digitsOnly.length < 13 || digitsOnly.length > 19) {
      return 'Please enter a valid credit card number';
    }
    
    // Luhn algorithm validation
    int sum = 0;
    bool alternate = false;
    
    for (int i = digitsOnly.length - 1; i >= 0; i--) {
      int digit = int.parse(digitsOnly[i]);
      
      if (alternate) {
        digit *= 2;
        if (digit > 9) {
          digit = (digit % 10) + 1;
        }
      }
      
      sum += digit;
      alternate = !alternate;
    }
    
    if (sum % 10 != 0) {
      return 'Please enter a valid credit card number';
    }
    
    return null; // Valid credit card number
  }
  
  // Combine multiple validators
  static String? combineValidators(String? value, List<String? Function(String?)> validators) {
    for (final validator in validators) {
      final result = validator(value);
      if (result != null) {
        return result; // Return first validation error
      }
    }
    return null; // All validations passed
  }
}
```

---

### 9. core/utils/formatters.dart
**Purpose**: Data formatting utilities for consistent data display
**Benefits**: Centralizes formatting logic, ensures consistent data presentation, improves code reusability

```dart
// Data formatting utilities for consistent data presentation across the app
// Provides reusable formatting functions for common data types
import 'package:intl/intl.dart';

class Formatters {
  // Private constructor to prevent instantiation
  Formatters._();
  
  // Date formatting methods
  static String formatDate(DateTime date, {String pattern = 'MMM dd, yyyy'}) {
    try {
      return DateFormat(pattern).format(date);
    } catch (e) {
      return date.toIso8601String().split('T')[0]; // Fallback to ISO date
    }
  }
  
  // Format date with time
  static String formatDateTime(DateTime dateTime, {String pattern = 'MMM dd, yyyy HH:mm'}) {
    try {
      return DateFormat(pattern).format(dateTime);
    } catch (e) {
      return dateTime.toIso8601String(); // Fallback to ISO datetime
    }
  }
  
  // Format time only
  static String formatTime(DateTime time, {bool use24Hour = false}) {
    try {
      final pattern = use24Hour ? 'HH:mm' : 'hh:mm a';
      return DateFormat(pattern).format(time);
    } catch (e) {
      return '${time.hour.toString().padLeft(2, '0')}:${time.minute.toString().padLeft(2, '0')}';
    }
  }
  
  // Relative time formatting (e.g., "2 hours ago")
  static String formatRelativeTime(DateTime dateTime) {
    final now = DateTime.now();
    final difference = now.difference(dateTime);
    
    if (difference.inDays > 365) {
      final years = (difference.inDays / 365).floor();
      return years == 1 ? '1 year ago' : '$years years ago';
    } else if (difference.inDays > 30) {
      final months = (difference.inDays / 30).floor();
      return months == 1 ? '1 month ago' : '$months months ago';
    } else if (difference.inDays > 0) {
      return difference.inDays == 1 ? '1 day ago' : '${difference.inDays} days ago';
    } else if (difference.inHours > 0) {
      return difference.inHours == 1 ? '1 hour ago' : '${difference.inHours} hours ago';
    } else if (difference.inMinutes > 0) {
      return difference.inMinutes == 1 ? '1 minute ago' : '${difference.inMinutes} minutes ago';
    } else {
      return 'Just now';
    }
  }
  
  // Currency formatting
  static String formatCurrency(double amount, {String symbol = '\, int decimalPlaces = 2}) {
    try {
      final formatter = NumberFormat.currency(
        symbol: symbol,
        decimalDigits: decimalPlaces,
      );
      return formatter.format(amount);
    } catch (e) {
      return '$symbol${amount.toStringAsFixed(decimalPlaces)}';
    }
  }
  
  // Number formatting with thousands separator
  static String formatNumber(num number, {int decimalPlaces = 0}) {
    try {
      final formatter = NumberFormat('#,##0.${'0' * decimalPlaces}');
      return formatter.format(number);
    } catch (e) {
      return number.toStringAsFixed(decimalPlaces);
    }
  }
  
  // Percentage formatting
  static String formatPercentage(double value, {int decimalPlaces = 1}) {
    try {
      final formatter = NumberFormat.percentPattern();
      formatter.minimumFractionDigits = decimalPlaces;
      formatter.maximumFractionDigits = decimalPlaces;
      return formatter.format(value);
    } catch (e) {
      return '${(value * 100).toStringAsFixed(decimalPlaces)}%';
    }
  }
  
  // File size formatting (bytes to human readable)
  static String formatFileSize(int bytes) {
    if (bytes < 1024) {
      return '$bytes B';
    } else if (bytes < 1024 * 1024) {
      return '${(bytes / 1024).toStringAsFixed(1)} KB';
    } else if (bytes < 1024 * 1024 * 1024) {
      return '${(bytes / (1024 * 1024)).toStringAsFixed(1)} MB';
    } else {
      return '${(bytes / (1024 * 1024 * 1024)).toStringAsFixed(1)} GB';
    }
  }
  
  // Phone number formatting
  static String formatPhoneNumber(String phoneNumber) {
    // Remove all non-digit characters
    final digitsOnly = phoneNumber.replaceAll(RegExp(r'\D'), '');
    
    if (digitsOnly.length == 10) {
      // Format as (XXX) XXX-XXXX
      return '(${digitsOnly.substring(0, 3)}) ${digitsOnly.substring(3, 6)}-${digitsOnly.substring(6)}';
    } else if (digitsOnly.length == 11 && digitsOnly.startsWith('1')) {
      // Format as +1 (XXX) XXX-XXXX
      return '+1 (${digitsOnly.substring(1, 4)}) ${digitsOnly.substring(4, 7)}-${digitsOnly.substring(7)}';
    } else {
      // Return original if can't format
      return phoneNumber;
    }
  }
  
  // Credit card number formatting
  static String formatCreditCard(String cardNumber) {
    // Remove all non-digit characters
    final digitsOnly = cardNumber.replaceAll(RegExp(r'\D'), '');
    
    // Format as XXXX XXXX XXXX XXXX
    final buffer = StringBuffer();
    for (int i = 0; i < digitsOnly.length; i++) {
      if (i > 0 && i % 4 == 0) {
        buffer.write(' ');
      }
      buffer.write(digitsOnly[i]);
    }
    
    return buffer.toString();
  }
  
  // Capitalize first letter of each word
  static String formatTitle(String text) {
    if (text.isEmpty) return text;
    
    return text.split(' ').map((word) {
      if (word.isEmpty) return word;
      return word[0].toUpperCase() + word.substring(1).toLowerCase();
    }).join(' ');
  }
  
  // Format name (proper case)
  static String formatName(String name) {
    if (name.isEmpty) return name;
    
    return name.split(' ').map((part) {
      if (part.isEmpty) return part;
      return part[0].toUpperCase() + part.substring(1).toLowerCase();
    }).join(' ').trim();
  }
  
  // Truncate text with ellipsis
  static String truncateText(String text, int maxLength, {String ellipsis = '...'}) {
    if (text.length <= maxLength) return text;
    
    return text.substring(0, maxLength - ellipsis.length) + ellipsis;
  }
  
  // Format duration (seconds to human readable)
  static String formatDuration(int seconds) {
    final duration = Duration(seconds: seconds);
    
    if (duration.inHours > 0) {
      return '${duration.inHours}:${(duration.inMinutes % 60).toString().padLeft(2, '0')}:${(duration.inSeconds % 60).toString().padLeft(2, '0')}';
    } else {
      return '${duration.inMinutes}:${(duration.inSeconds % 60).toString().padLeft(2, '0')}';
    }
  }
  
  // Format list to string with proper conjunction
  static String formatList(List<String> items, {String conjunction = 'and'}) {
    if (items.isEmpty) return '';
    if (items.length == 1) return items[0];
    if (items.length == 2) return '${items[0]} $conjunction ${items[1]}';
    
    final allButLast = items.sublist(0, items.length - 1).join(', ');
    return '$allButLast, $conjunction ${items.last}';
  }
  
  // Format address (basic formatting)
  static String formatAddress({
    String? street,
    String? city,
    String? state,
    String? zipCode,
    String? country,
  }) {
    final parts = <String>[];
    
    if (street != null && street.isNotEmpty) parts.add(street);
    if (city != null && city.isNotEmpty) parts.add(city);
    if (state != null && state.isNotEmpty) parts.add(state);
    if (zipCode != null && zipCode.isNotEmpty) parts.add(zipCode);
    if (country != null && country.isNotEmpty) parts.add(country);
    
    return parts.join(', ');
  }
  
  // Remove HTML tags from text
  static String stripHtmlTags(String htmlText) {
    return htmlText.replaceAll(RegExp(r'<[^>]*>'), '');
  }
  
  // Format social security number (basic masking)
  static String formatSSN(String ssn, {bool mask = true}) {
    final digitsOnly = ssn.replaceAll(RegExp(r'\D'), '');
    
    if (digitsOnly.length != 9) return ssn; // Invalid SSN
    
    if (mask) {
      // Show only last 4 digits: XXX-XX-1234
      return 'XXX-XX-${digitsOnly.substring(5)}';
    } else {
      // Show full SSN: 123-45-6789
      return '${digitsOnly.substring(0, 3)}-${digitsOnly.substring(3, 5)}-${digitsOnly.substring(5)}';
    }
  }
  
  // Format version number
  static String formatVersion(int major, int minor, int patch, {String? build}) {
    final version = '$major.$minor.$patch';
    return build != null ? '$version+$build' : version;
  }
}
```

---

## core/utils/extensions.dart
**Purpose**: Provides convenient extension methods on common Dart/Flutter classes to reduce boilerplate and improve code readability.

```dart
// Extension methods to add useful functionality to existing classes
// Benefits: Reduces boilerplate code, improves readability, promotes code reuse

import 'package:flutter/material.dart';

/// Extension on String class to add common string operations
extension StringExtensions on String {
  // Capitalizes first letter of the string
  // Example: "hello".capitalize() returns "Hello"
  String capitalize() {
    if (isEmpty) return this;
    return this[0].toUpperCase() + substring(1).toLowerCase();
  }

  // Checks if string is a valid email format
  // Example: "test@email.com".isValidEmail returns true
  bool get isValidEmail {
    return RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(this);
  }

  // Checks if string is a valid phone number (basic validation)
  // Example: "+1234567890".isValidPhone returns true
  bool get isValidPhone {
    return RegExp(r'^\+?[1-9]\d{1,14}$').hasMatch(this);
  }

  // Removes all whitespace from string
  // Example: "hello world".removeWhitespace returns "helloworld"
  String get removeWhitespace => replaceAll(RegExp(r'\s+'), '');

  // Truncates string to specified length with ellipsis
  // Example: "Hello World".truncate(5) returns "Hello..."
  String truncate(int maxLength) {
    if (length <= maxLength) return this;
    return '${substring(0, maxLength)}...';
  }
}

/// Extension on DateTime class for common date operations
extension DateTimeExtensions on DateTime {
  // Formats date to readable string (e.g., "Jan 15, 2024")
  String get formattedDate {
    const months = [
      'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
      'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
    ];
    return '${months[month - 1]} $day, $year';
  }

  // Checks if date is today
  bool get isToday {
    final now = DateTime.now();
    return year == now.year && month == now.month && day == now.day;
  }

  // Returns time ago string (e.g., "2 hours ago")
  String get timeAgo {
    final now = DateTime.now();
    final difference = now.difference(this);

    if (difference.inDays > 365) {
      return '${(difference.inDays / 365).floor()} year${(difference.inDays / 365).floor() == 1 ? '' : 's'} ago';
    } else if (difference.inDays > 30) {
      return '${(difference.inDays / 30).floor()} month${(difference.inDays / 30).floor() == 1 ? '' : 's'} ago';
    } else if (difference.inDays > 0) {
      return '${difference.inDays} day${difference.inDays == 1 ? '' : 's'} ago';
    } else if (difference.inHours > 0) {
      return '${difference.inHours} hour${difference.inHours == 1 ? '' : 's'} ago';
    } else if (difference.inMinutes > 0) {
      return '${difference.inMinutes} minute${difference.inMinutes == 1 ? '' : 's'} ago';
    } else {
      return 'Just now';
    }
  }
}

/// Extension on BuildContext for easier access to theme and navigation
extension BuildContextExtensions on BuildContext {
  // Quick access to theme data
  ThemeData get theme => Theme.of(this);
  
  // Quick access to text theme
  TextTheme get textTheme => Theme.of(this).textTheme;
  
  // Quick access to color scheme
  ColorScheme get colorScheme => Theme.of(this).colorScheme;
  
  // Quick access to media query
  MediaQueryData get mediaQuery => MediaQuery.of(this);
  
  // Quick access to screen size
  Size get screenSize => MediaQuery.of(this).size;
  
  // Quick access to screen width
  double get screenWidth => MediaQuery.of(this).size.width;
  
  // Quick access to screen height
  double get screenHeight => MediaQuery.of(this).size.height;
  
  // Check if device is tablet (width > 600)
  bool get isTablet => MediaQuery.of(this).size.width > 600;
  
  // Quick navigation methods
  void pop() => Navigator.of(this).pop();
  
  Future<T?> push<T>(Widget page) => Navigator.of(this).push<T>(
    MaterialPageRoute(builder: (_) => page),
  );
  
  Future<T?> pushNamed<T>(String routeName, {Object? arguments}) =>
      Navigator.of(this).pushNamed<T>(routeName, arguments: arguments);
  
  Future<T?> pushReplacement<T>(Widget page) => Navigator.of(this).pushReplacement<T>(
    MaterialPageRoute(builder: (_) => page),
  );
  
  void showSnackBar(String message, {bool isError = false}) {
    ScaffoldMessenger.of(this).showSnackBar(
      SnackBar(
        content: Text(message),
        backgroundColor: isError ? Colors.red : null,
      ),
    );
  }
}

## data/models/api_response_model.dart
**Purpose**: Generic wrapper for API responses that provides consistent error handling and success/failure states across the application.

```dart
// Generic API response wrapper for consistent error handling
// Benefits: Standardized response format, type-safe error handling, easy success/failure checks

/// Generic wrapper class for API responses
/// Provides consistent structure for handling success and error states
/// T represents the type of data returned on success
class ApiResponse<T> {
  // Response data (null if error occurred)
  final T? data;
  
  // Error message (null if request was successful)
  final String? error;
  
  // HTTP status code
  final int? statusCode;
  
  // Additional metadata about the response
  final Map<String, dynamic>? metadata;
  
  // Timestamp when response was created
  final DateTime timestamp;

  /// Private constructor to ensure responses are created through factory methods
  ApiResponse._({
    this.data,
    this.error,
    this.statusCode,
    this.metadata,
    DateTime? timestamp,
  }) : timestamp = timestamp ?? DateTime.now();

  /// Factory constructor for successful responses
  factory ApiResponse.success(
    T? data, {
    int? statusCode,
    Map<String, dynamic>? metadata,
  }) {
    return ApiResponse._(
      data: data,
      statusCode: statusCode ?? 200,
      metadata: metadata,
    );
  }

  /// Factory constructor for error responses
  factory ApiResponse.error(
    String error, {
    int? statusCode,
    Map<String, dynamic>? metadata,
  }) {
    return ApiResponse._(
      error: error,
      statusCode: statusCode ?? 500,
      metadata: metadata,
    );
  }

  /// Factory constructor from JSON response
  /// Useful when API returns standardized response format
  factory ApiResponse.fromJson(
    Map<String, dynamic> json, {
    T Function(dynamic)? fromJsonT,
  }) {
    // Check if response indicates success
    final bool success = json['success'] as bool? ?? false;
    final int statusCode = json['status_code'] as int? ?? (success ? 200 : 500);
    final Map<String, dynamic>? metadata = json['metadata'] as Map<String, dynamic>?;

    if (success) {
      // Parse successful response
      T? data;
      if (json['data'] != null && fromJsonT != null) {
        data = fromJsonT(json['data']);
      } else {
        data = json['data'] as T?;
      }

      return ApiResponse.success(
        data,
        statusCode: statusCode,
        metadata: metadata,
      );
    } else {
      // Parse error response
      final String error = json['message'] as String? ?? 
                          json['error'] as String? ?? 
                          'Unknown error occurred';
      
      return ApiResponse.error(
        error,
        statusCode: statusCode,
        metadata: metadata,
      );
    }
  }

  // --- Computed Properties ---

  /// Check if response represents a successful operation
  bool get isSuccess => error == null;

  /// Check if response represents a failed operation
  bool get isError => error != null;

  /// Check if response has data
  bool get hasData => data != null;

  /// Check if response is loading (neither success nor error)
  /// Useful for showing loading states
  bool get isLoading => error == null && data == null;

  /// Get success status code range (200-299)
  bool get isSuccessStatusCode {
    if (statusCode == null) return false;
    return statusCode! >= 200 && statusCode! < 300;
  }

  /// Get client error status code range (400-499)
  bool get isClientError {
    if (statusCode == null) return false;
    return statusCode! >= 400 && statusCode! < 500;
  }

  /// Get server error status code range (500+)
  bool get isServerError {
    if (statusCode == null) return false;
    return statusCode! >= 500;
  }

  // --- Data Access Methods ---

  /// Get data with null safety
  /// Returns data if available, otherwise returns provided default value
  T? getDataOrNull() => data;

  /// Get data or throw exception if not available
  T getDataOrThrow() {
    if (data == null) {
      throw Exception(error ?? 'No data available');
    }
    return data!;
  }

  /// Get data or return default value
  T getDataOrDefault(T defaultValue) => data ?? defaultValue;

  /// Get error message or return default message
  String getErrorOrDefault(String defaultMessage) => error ?? defaultMessage;

  // --- Transformation Methods ---

  /// Transform the data type of successful response
  /// Useful for converting response data to different types
  ApiResponse<R> map<R>(R Function(T) transform) {
    if (isSuccess && data != null) {
      try {
        final transformedData = transform(data!);
        return ApiResponse.success(
          transformedData,
          statusCode: statusCode,
          metadata: metadata,
        );
      } catch (e) {
        return ApiResponse.error(
          'Data transformation failed: ${e.toString()}',
          statusCode: statusCode,
          metadata: metadata,
        );
      }
    } else {
      return ApiResponse.error(
        error ?? 'No data to transform',
        statusCode: statusCode,
        metadata: metadata,
      );
    }
  }

  /// Transform error message
  ApiResponse<T> mapError(String Function(String) transform) {
    if (isError) {
      return ApiResponse.error(
        transform(error!),
        statusCode: statusCode,
        metadata: metadata,
      );
    }
    return this;
  }

  // --- Async Handling Methods ---

  /// Execute function if response is successful
  Future<void> onSuccess(Future<void> Function(T) callback) async {
    if (isSuccess && data != null) {
      await callback(data!);
    }
  }

  /// Execute function if response is error
  Future<void> onError(Future<void> Function(String) callback) async {
    if (isError) {
      await callback(error!);
    }
  }

  /// Execute appropriate callback based on response state
  Future<R> fold<R>(
    Future<R> Function(String error) onError,
    Future<R> Function(T data) onSuccess,
  ) async {
    if (isError) {
      return await onError(error!);
    } else if (data != null) {
      return await onSuccess(data!);
    } else {
      return await onError('No data available');
    }
  }

  // --- Serialization ---

  /// Convert ApiResponse to JSON
  Map<String, dynamic> toJson({
    Map<String, dynamic> Function(T)? dataToJson,
  }) {
    return {
      'success': isSuccess,
      'data': data != null && dataToJson != null ? dataToJson(data!) : data,
      'error': error,
      'message': error, // Alias for error
      'status_code': statusCode,
      'metadata': metadata,
      'timestamp': timestamp.toIso8601String(),
    };
  }

  // --- Utility Methods ---

  /// Merge metadata from another ApiResponse
  ApiResponse<T> mergeMetadata(Map<String, dynamic> additionalMetadata) {
    final mergedMetadata = <String, dynamic>{
      ...?metadata,
      ...additionalMetadata,
    };

    if (isSuccess) {
      return ApiResponse.success(
        data,
        statusCode: statusCode,
        metadata: mergedMetadata,
      );
    } else {
      return ApiResponse.error(
        error!,
        statusCode: statusCode,
        metadata: mergedMetadata,
      );
    }
  }

  @override
  String toString() {
    if (isSuccess) {
      return 'ApiResponse.success(data: $data, statusCode: $statusCode)';
    } else {
      return 'ApiResponse.error(error: $error, statusCode: $statusCode)';
    }
  }

  // --- Static Helper Methods ---

  /// Create loading state response
  static ApiResponse<T> loading<T>() {
    return ApiResponse<T>._(
      statusCode: null,
      metadata: {'state': 'loading'},
    );
  }

  /// Create response from exception
  static ApiResponse<T> fromException<T>(Exception exception) {
    return ApiResponse.error(
      exception.toString(),
      statusCode: 500,
      metadata: {'exception_type': exception.runtimeType.toString()},
    );
  }

  /// Combine multiple responses into one
  /// Returns success only if all responses are successful
  static ApiResponse<List<T>> combine<T>(List<ApiResponse<T>> responses) {
    final List<T> successData = [];
    final List<String> errors = [];

    for (final response in responses) {
      if (response.isSuccess && response.data != null) {
        successData.add(response.data!);
      } else if (response.isError) {
        errors.add(response.error!);
      }
    }

    if (errors.isEmpty) {
      return ApiResponse.success(successData);
    } else {
      return ApiResponse.error(
        'Multiple errors: ${errors.join(', ')}',
        metadata: {'error_count': errors.length},
      );
    }
  }
}
```

## data/repositories/user_repository.dart
**Purpose**: Repository layer that handles user-related data operations. Abstracts data sources (API, local storage) and provides clean interface to the application.

```dart
// User repository for data operations and caching
// Benefits: Data abstraction, caching strategy, single source of truth for user data

import '../models/user_model.dart';
import '../models/api_response_model.dart';
import '../../core/services/api_service.dart';
import '../../core/services/storage_service.dart';
import '../../core/constants/api_constants.dart';

/// Repository class for user-related data operations
/// Handles both remote API calls and local data caching
/// Provides a clean interface between data sources and business logic
class UserRepository {
  // Dependencies injection
  final ApiService _apiService;
  final StorageService _storageService;

  // Constructor with dependency injection
  UserRepository({
    ApiService? apiService,
    StorageService? storageService,
  }) : _apiService = apiService ?? ApiService(),
       _storageService = storageService ?? StorageService();

  // Cache keys for local storage
  static const String _currentUserKey = 'current_user';
  static const String _usersCacheKey = 'users_cache';
  static const String _cacheTimestampKey = 'users_cache_timestamp';
  static const int _cacheValidityHours = 1; // Cache valid for 1 hour

  // --- Authentication Related Methods ---

  /// Login user with email and password
  /// Returns user data on successful authentication
  Future<ApiResponse<UserModel>> login({
    required String email,
    required String password,
  }) async {
    try {
      // Prepare login request data
      final requestData = {
        'email': email,
        'password': password,
      };

      // Make API call for login
      final response = await _apiService.post<Map<String, dynamic>>(
        ApiConstants.loginEndpoint,
        data: requestData,
      );

      // Handle successful login
      if (response.isSuccess && response.data != null) {
        // Parse user data from response
        final userData = response.data!['user'] as Map<String, dynamic>;
        final user = UserModel.fromJson(userData);
        
        // Save authentication token
        final token = response.data!['token'] as String;
        await _storageService.saveAuthData(token, user.id);
        
        // Cache current user data
        await _cacheCurrentUser(user);
        
        // Set auth token for future API calls
        _apiService.setAuthToken(token);
        
        return ApiResponse.success(user);
      } else {
        return ApiResponse.error(response.error ?? 'Login failed');
      }
    } catch (e) {
      return ApiResponse.error('Login error: ${e.toString()}');
    }
  }

  /// Register new user account
  Future<ApiResponse<UserModel>> register({
    required String firstName,
    required String lastName,
    required String email,
    required String password,
    String? phoneNumber,
  }) async {
    try {
      final requestData = {
        'first_name': firstName,
        'last_name': lastName,
        'email': email,
        'password': password,
        if (phoneNumber != null) 'phone_number': phoneNumber,
      };

      final response = await _apiService.post<Map<String, dynamic>>(
        ApiConstants.registerEndpoint,
        data: requestData,
      );

      if (response.isSuccess && response.data != null) {
        final userData = response.data!['user']

/// Extension on List for common operations
extension ListExtensions<T> on List<T> {
  // Safely get element at index (returns null if index out of bounds)
  T? safeGet(int index) {
    if (index >= 0 && index < length) {
      return this[index];
    }
    return null;
  }
  
  // Add element if it doesn't exist
  void addIfNotExists(T element) {
    if (!contains(element)) {
      add(element);
    }
  }
  
  // Toggle element (add if not exists, remove if exists)
  void toggle(T element) {
    if (contains(element)) {
      remove(element);
    } else {
      add(element);
    }
  }
}

/// Extension on double for responsive design
extension ResponsiveDouble on double {
  // Convert to responsive width based on screen width
  double w(BuildContext context) => 
      this * context.screenWidth / 375; // 375 is base design width
  
  // Convert to responsive height based on screen height
  double h(BuildContext context) => 
      this * context.screenHeight / 812; // 812 is base design height
  
  // Convert to responsive text size
  double sp(BuildContext context) => 
      this * context.screenWidth / 375;
}
```

## core/services/api_service.dart
**Purpose**: Centralized HTTP service layer for all API communications. Handles network requests, error handling, and response parsing.

```dart
// Centralized API service for all network operations
// Benefits: Single point of network logic, consistent error handling, easy to mock for testing

import 'dart:convert';
import 'dart:io';
import 'package:http/http.dart' as http;
import '../constants/api_constants.dart';
import '../../data/models/api_response_model.dart';

/// Service class to handle all HTTP operations
/// Provides a consistent interface for API calls throughout the app
class ApiService {
  // Singleton pattern implementation
  static final ApiService _instance = ApiService._internal();
  factory ApiService() => _instance;
  ApiService._internal();

  // HTTP client instance for making requests
  final http.Client _client = http.Client();
  
  // Default headers for all requests
  Map<String, String> get _defaultHeaders => {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
  };

  /// GET request method
  /// Returns ApiResponse with parsed data or error information
  Future<ApiResponse<T>> get<T>(
    String endpoint, {
    Map<String, String>? headers,
    T Function(Map<String, dynamic>)? fromJson,
  }) async {
    try {
      // Construct full URL from base URL and endpoint
      final url = Uri.parse('${ApiConstants.baseUrl}$endpoint');
      
      // Make HTTP GET request with merged headers
      final response = await _client.get(
        url,
        headers: {..._defaultHeaders, ...?headers},
      );

      // Parse and return response
      return _handleResponse<T>(response, fromJson);
    } catch (e) {
      // Handle any exceptions during the request
      return ApiResponse<T>.error('Network error: ${e.toString()}');
    }
  }

  /// POST request method
  /// Accepts data to be sent in request body
  Future<ApiResponse<T>> post<T>(
    String endpoint, {
    Map<String, dynamic>? data,
    Map<String, String>? headers,
    T Function(Map<String, dynamic>)? fromJson,
  }) async {
    try {
      final url = Uri.parse('${ApiConstants.baseUrl}$endpoint');
      
      final response = await _client.post(
        url,
        headers: {..._defaultHeaders, ...?headers},
        body: data != null ? json.encode(data) : null,
      );

      return _handleResponse<T>(response, fromJson);
    } catch (e) {
      return ApiResponse<T>.error('Network error: ${e.toString()}');
    }
  }

  /// PUT request method for updating resources
  Future<ApiResponse<T>> put<T>(
    String endpoint, {
    Map<String, dynamic>? data,
    Map<String, String>? headers,
    T Function(Map<String, dynamic>)? fromJson,
  }) async {
    try {
      final url = Uri.parse('${ApiConstants.baseUrl}$endpoint');
      
      final response = await _client.put(
        url,
        headers: {..._defaultHeaders, ...?headers},
        body: data != null ? json.encode(data) : null,
      );

      return _handleResponse<T>(response, fromJson);
    } catch (e) {
      return ApiResponse<T>.error('Network error: ${e.toString()}');
    }
  }

  /// DELETE request method
  Future<ApiResponse<T>> delete<T>(
    String endpoint, {
    Map<String, String>? headers,
    T Function(Map<String, dynamic>)? fromJson,
  }) async {
    try {
      final url = Uri.parse('${ApiConstants.baseUrl}$endpoint');
      
      final response = await _client.delete(
        url,
        headers: {..._defaultHeaders, ...?headers},
      );

      return _handleResponse<T>(response, fromJson);
    } catch (e) {
      return ApiResponse<T>.error('Network error: ${e.toString()}');
    }
  }

  /// Private method to handle HTTP responses consistently
  /// Parses JSON and creates appropriate ApiResponse objects
  ApiResponse<T> _handleResponse<T>(
    http.Response response, 
    T Function(Map<String, dynamic>)? fromJson,
  ) {
    try {
      // Check if response has content to parse
      if (response.body.isEmpty) {
        if (response.statusCode >= 200 && response.statusCode < 300) {
          return ApiResponse<T>.success(null);
        } else {
          return ApiResponse<T>.error('Request failed with status: ${response.statusCode}');
        }
      }

      // Parse JSON response
      final Map<String, dynamic> jsonData = json.decode(response.body);

      // Handle successful responses (200-299)
      if (response.statusCode >= 200 && response.statusCode < 300) {
        // If fromJson function provided, use it to parse data
        if (fromJson != null && jsonData.containsKey('data')) {
          final T parsedData = fromJson(jsonData['data']);
          return ApiResponse<T>.success(parsedData);
        }
        // Otherwise return raw data
        return ApiResponse<T>.success(jsonData['data'] as T?);
      } 
      // Handle client errors (400-499)
      else if (response.statusCode >= 400 && response.statusCode < 500) {
        final message = jsonData['message'] ?? 'Client error occurred';
        return ApiResponse<T>.error(message);
      }
      // Handle server errors (500+)
      else {
        final message = jsonData['message'] ?? 'Server error occurred';
        return ApiResponse<T>.error(message);
      }
    } catch (e) {
      // Handle JSON parsing errors
      return ApiResponse<T>.error('Failed to parse response: ${e.toString()}');
    }
  }

  /// Method to set authorization header for authenticated requests
  void setAuthToken(String token) {
    _defaultHeaders['Authorization'] = 'Bearer $token';
  }

  /// Method to remove authorization header
  void clearAuthToken() {
    _defaultHeaders.remove('Authorization');
  }

  /// Cleanup method to close HTTP client
  void dispose() {
    _client.close();
  }
}
```

## core/services/storage_service.dart
**Purpose**: Handles local data persistence using SharedPreferences. Provides type-safe methods for storing and retrieving data locally.

```dart
// Local storage service for persisting data on device
// Benefits: Centralized storage logic, type-safe operations, consistent data handling

import 'dart:convert';
import 'package:shared_preferences/shared_preferences.dart';

/// Service class for local data storage operations
/// Wraps SharedPreferences with convenient, type-safe methods
class StorageService {
  // Singleton pattern implementation
  static final StorageService _instance = StorageService._internal();
  factory StorageService() => _instance;
  StorageService._internal();

  // SharedPreferences instance (lazy initialization)
  SharedPreferences? _prefs;

  /// Initialize the storage service
  /// Must be called before using any storage operations
  Future<void> init() async {
    _prefs ??= await SharedPreferences.getInstance();
  }

  /// Private getter to ensure preferences are initialized
  SharedPreferences get prefs {
    if (_prefs == null) {
      throw Exception('StorageService not initialized. Call init() first.');
    }
    return _prefs!;
  }

  // --- String Operations ---
  
  /// Save string value to storage
  Future<bool> setString(String key, String value) async {
    return await prefs.setString(key, value);
  }

  /// Get string value from storage
  /// Returns null if key doesn't exist
  String? getString(String key) {
    return prefs.getString(key);
  }

  // --- Integer Operations ---
  
  /// Save integer value to storage
  Future<bool> setInt(String key, int value) async {
    return await prefs.setInt(key, value);
  }

  /// Get integer value from storage
  /// Returns null if key doesn't exist
  int? getInt(String key) {
    return prefs.getInt(key);
  }

  // --- Boolean Operations ---
  
  /// Save boolean value to storage
  Future<bool> setBool(String key, bool value) async {
    return await prefs.setBool(key, value);
  }

  /// Get boolean value from storage with default fallback
  bool getBool(String key, {bool defaultValue = false}) {
    return prefs.getBool(key) ?? defaultValue;
  }

  // --- Double Operations ---
  
  /// Save double value to storage
  Future<bool> setDouble(String key, double value) async {
    return await prefs.setDouble(key, value);
  }

  /// Get double value from storage
  double? getDouble(String key) {
    return prefs.getDouble(key);
  }

  // --- List Operations ---
  
  /// Save string list to storage
  Future<bool> setStringList(String key, List<String> value) async {
    return await prefs.setStringList(key, value);
  }

  /// Get string list from storage
  List<String>? getStringList(String key) {
    return prefs.getStringList(key);
  }

  // --- Object Operations (JSON) ---
  
  /// Save complex object as JSON string
  /// Object must be serializable to Map<String, dynamic>
  Future<bool> setObject(String key, Map<String, dynamic> value) async {
    final jsonString = json.encode(value);
    return await setString(key, jsonString);
  }

  /// Get complex object from JSON string
  /// Returns null if key doesn't exist or JSON is invalid
  Map<String, dynamic>? getObject(String key) {
    final jsonString = getString(key);
    if (jsonString == null) return null;
    
    try {
      return json.decode(jsonString) as Map<String, dynamic>;
    } catch (e) {
      // Return null if JSON parsing fails
      return null;
    }
  }

  // --- Utility Operations ---
  
  /// Check if key exists in storage
  bool containsKey(String key) {
    return prefs.containsKey(key);
  }

  /// Remove specific key from storage
  Future<bool> remove(String key) async {
    return await prefs.remove(key);
  }

  /// Clear all data from storage
  /// Use with caution - this removes ALL stored data
  Future<bool> clear() async {
    return await prefs.clear();
  }

  /// Get all keys stored in preferences
  Set<String> getAllKeys() {
    return prefs.getKeys();
  }

  /// Reload preferences from storage
  /// Useful when preferences might have been modified externally
  Future<void> reload() async {
    await prefs.reload();
  }

  // --- Common App-Specific Storage Keys ---
  
  /// User authentication token storage
  static const String _authTokenKey = 'auth_token';
  static const String _userIdKey = 'user_id';
  static const String _themeKey = 'app_theme';
  static const String _onboardingKey = 'onboarding_completed';

  /// Save user authentication data
  Future<void> saveAuthData(String token, String userId) async {
    await Future.wait([
      setString(_authTokenKey, token),
      setString(_userIdKey, userId),
    ]);
  }

  /// Get saved authentication token
  String? getAuthToken() => getString(_authTokenKey);

  /// Get saved user ID
  String? getUserId() => getString(_userIdKey);

  /// Clear authentication data (logout)
  Future<void> clearAuthData() async {
    await Future.wait([
      remove(_authTokenKey),
      remove(_userIdKey),
    ]);
  }

  /// Check if user is logged in
  bool get isLoggedIn => getAuthToken() != null;

  /// Save app theme preference
  Future<void> saveTheme(String theme) async {
    await setString(_themeKey, theme);
  }

  /// Get saved theme preference
  String? getTheme() => getString(_themeKey);

  /// Mark onboarding as completed
  Future<void> setOnboardingCompleted() async {
    await setBool(_onboardingKey, true);
  }

  /// Check if onboarding is completed
  bool get isOnboardingCompleted => getBool(_onboardingKey);
}
```

## core/services/navigation_service.dart
**Purpose**: Centralized navigation service that allows navigation from anywhere in the app without requiring BuildContext. Essential for navigation from business logic layers.

```dart
// Global navigation service for context-free navigation
// Benefits: Navigate from anywhere (controllers, services), centralized navigation logic

import 'package:flutter/material.dart';

/// Global navigation service that provides navigation methods
/// without requiring BuildContext. Useful for navigation from
/// controllers, services, or other non-widget classes.
class NavigationService {
  // Singleton pattern implementation
  static final NavigationService _instance = NavigationService._internal();
  factory NavigationService() => _instance;
  NavigationService._internal();

  // Global navigator key - must be assigned to MaterialApp
  final GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();

  /// Get the current navigator state
  /// Throws exception if navigator is not available
  NavigatorState get _navigator {
    final navigator = navigatorKey.currentState;
    if (navigator == null) {
      throw Exception('Navigator not found. Make sure NavigationService.navigatorKey is assigned to MaterialApp.');
    }
    return navigator;
  }

  /// Get current context from navigator
  /// Returns null if context is not available
  BuildContext? get currentContext => navigatorKey.currentContext;

  // --- Basic Navigation Methods ---

  /// Navigate to a new screen and add it to the navigation stack
  Future<T?> navigateTo<T extends Object?>(Widget page) {
    return _navigator.push<T>(
      MaterialPageRoute(builder: (_) => page),
    );
  }

  /// Navigate to a named route
  Future<T?> navigateToNamed<T extends Object?>(
    String routeName, {
    Object? arguments,
  }) {
    return _navigator.pushNamed<T>(routeName, arguments: arguments);
  }

  /// Replace current screen with new screen
  Future<T?> navigateToReplacement<T extends Object?, TO extends Object?>(
    Widget page, {
    TO? result,
  }) {
    return _navigator.pushReplacement<T, TO>(
      MaterialPageRoute(builder: (_) => page),
      result: result,
    );
  }

  /// Replace current screen with named route
  Future<T?> navigateToReplacementNamed<T extends Object?, TO extends Object?>(
    String routeName, {
    TO? result,
    Object? arguments,
  }) {
    return _navigator.pushReplacementNamed<T, TO>(
      routeName,
      result: result,
      arguments: arguments,
    );
  }

  /// Navigate and clear entire navigation stack
  Future<T?> navigateAndClearStack<T extends Object?>(Widget page) {
    return _navigator.pushAndRemoveUntil<T>(
      MaterialPageRoute(builder: (_) => page),
      (route) => false, // Remove all previous routes
    );
  }

  /// Navigate to named route and clear stack
  Future<T?> navigateAndClearStackNamed<T extends Object?>(
    String routeName, {
    Object? arguments,
  }) {
    return _navigator.pushNamedAndRemoveUntil<T>(
      routeName,
      (route) => false,
      arguments: arguments,
    );
  }

  /// Navigate and remove routes until predicate returns true
  Future<T?> navigateAndRemoveUntil<T extends Object?>(
    Widget page,
    bool Function(Route<dynamic>) predicate,
  ) {
    return _navigator.pushAndRemoveUntil<T>(
      MaterialPageRoute(builder: (_) => page),
      predicate,
    );
  }

  // --- Back Navigation Methods ---

  /// Go back to previous screen
  /// Optionally pass result data
  void goBack<T extends Object?>([T? result]) {
    if (canGoBack()) {
      _navigator.pop<T>(result);
    }
  }

  /// Go back until predicate returns true
  void goBackUntil(bool Function(Route<dynamic>) predicate) {
    _navigator.popUntil(predicate);
  }

  /// Go back to specific named route
  void goBackToRoute(String routeName) {
    _navigator.popUntil(ModalRoute.withName(routeName));
  }

  /// Check if navigation stack can go back
  bool canGoBack() {
    return _navigator.canPop();
  }

  // --- Dialog and Modal Methods ---

  /// Show dialog with custom widget
  Future<T?> showDialogCustom<T>({
    required Widget dialog,
    bool barrierDismissible = true,
    Color? barrierColor,
    String? barrierLabel,
  }) {
    final context = currentContext;
    if (context == null) return Future.value(null);

    return showDialog<T>(
      context: context,
      barrierDismissible: barrierDismissible,
      barrierColor: barrierColor,
      barrierLabel: barrierLabel,
      builder: (_) => dialog,
    );
  }

  /// Show alert dialog with title and message
  Future<bool?> showAlert({
    required String title,
    required String message,
    String confirmText = 'OK',
    String? cancelText,
    bool barrierDismissible = true,
  }) {
    final context = currentContext;
    if (context == null) return Future.value(null);

    return showDialog<bool>(
      context: context,
      barrierDismissible: barrierDismissible,
      builder: (context) => AlertDialog(
        title: Text(title),
        content: Text(message),
        actions: [
          if (cancelText != null)
            TextButton(
              onPressed: () => Navigator.of(context).pop(false),
              child: Text(cancelText),
            ),
          TextButton(
            onPressed: () => Navigator.of(context).pop(true),
            child: Text(confirmText),
          ),
        ],
      ),
    );
  }

  /// Show bottom sheet modal
  Future<T?> showBottomSheetModal<T>({
    required Widget content,
    bool isScrollControlled = false,
    bool isDismissible = true,
    bool enableDrag = true,
    Color? backgroundColor,
    double? elevation,
    ShapeBorder? shape,
  }) {
    final context = currentContext;
    if (context == null) return Future.value(null);

    return showModalBottomSheet<T>(
      context: context,
      isScrollControlled: isScrollControlled,
      isDismissible: isDismissible,
      enableDrag: enableDrag,
      backgroundColor: backgroundColor,
      elevation: elevation,
      shape: shape,
      builder: (_) => content,
    );
  }

  // --- Snackbar Methods ---

  /// Show snackbar with message
  void showSnackBar(
    String message, {
    Duration duration = const Duration(seconds: 3),
    SnackBarAction? action,
    Color? backgroundColor,
    bool isError = false,
  }) {
    final context = currentContext;
    if (context == null) return;

    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message),
        duration: duration,
        action: action,
        backgroundColor: backgroundColor ?? (isError ? Colors.red : null),
      ),
    );
  }

  /// Show error snackbar
  void showErrorSnackBar(String message) {
    showSnackBar(message, isError: true);
  }

  /// Show success snackbar
  void showSuccessSnackBar(String message) {
    showSnackBar(message, backgroundColor: Colors.green);
  }

  // --- Utility Methods ---

  /// Get current route name
  String? getCurrentRouteName() {
    String? currentRouteName;
    _navigator.popUntil((route) {
      currentRouteName = route.settings.name;
      return true;
    });
    return currentRouteName;
  }

  /// Check if specific route is in navigation stack
  bool isRouteInStack(String routeName) {
    bool found = false;
    _navigator.popUntil((route) {
      if (route.settings.name == routeName) {
        found = true;
      }
      return true;
    });
    return found;
  }
}
```

## data/models/user_model.dart
**Purpose**: Data model representing user entity with JSON serialization/deserialization. Provides type-safe user data structure.

```dart
// User data model with JSON serialization
// Benefits: Type safety, consistent data structure, easy API integration

import '../../shared/enums/user_role.dart';

/// Model class representing a user entity
/// Handles JSON serialization/deserialization for API communication
class UserModel {
  // User properties with final fields for immutability
  final String id;
  final String firstName;
  final String lastName;
  final String email;
  final String? phoneNumber;
  final String? profileImageUrl;
  final UserRole role;
  final DateTime createdAt;
  final DateTime? lastLoginAt;
  final bool isActive;
  final Map<String, dynamic>? metadata;

  /// Constructor with required and optional parameters
  const UserModel({
    required this.id,
    required this.firstName,
    required this.lastName,
    required this.email,
    this.phoneNumber,
    this.profileImageUrl,
    required this.role,
    required this.createdAt,
    this.lastLoginAt,
    this.isActive = true,
    this.metadata,
  });

  /// Factory constructor to create UserModel from JSON
  /// Used when receiving data from API responses
  factory UserModel.fromJson(Map<String, dynamic> json) {
    return UserModel(
      id: json['id'] as String,
      firstName: json['first_name'] as String,
      lastName: json['last_name'] as String,
      email: json['email'] as String,
      phoneNumber: json['phone_number'] as String?,
      profileImageUrl: json['profile_image_url'] as String?,
      role: UserRole.fromString(json['role'] as String),
      createdAt: DateTime.parse(json['created_at'] as String),
      lastLoginAt: json['last_login_at'] != null 
          ? DateTime.parse(json['last_login_at'] as String)
          : null,
      isActive: json['is_active'] as bool? ?? true,
      metadata: json['metadata'] as Map<String, dynamic>?,
    );
  }

  /// Convert UserModel to JSON for API requests
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'first_name': firstName,
      'last_name': lastName,
      'email': email,
      'phone_number': phoneNumber,
      'profile_image_url': profileImageUrl,
      'role': role.value,
      'created_at': createdAt.toIso8601String(),
      'last_login_at': lastLoginAt?.toIso8601String(),
      'is_active': isActive,
      'metadata': metadata,
    };
  }

  /// Create a copy of UserModel with updated fields
  /// Useful for updating user data immutably
  UserModel copyWith({
    String? id,
    String? firstName,
    String? lastName,
    String? email,
    String? phoneNumber,
    String? profileImageUrl,
    UserRole? role,
    DateTime? createdAt,
    DateTime? lastLoginAt,
    bool? isActive,
    Map<String, dynamic>? metadata,
  }) {
    return UserModel(
      id: id ?? this.id,
      firstName: firstName ?? this.firstName,
      lastName: lastName ?? this.lastName,
      email: email ?? this.email,
      phoneNumber: phoneNumber ?? this.phoneNumber,
      profileImageUrl: profileImageUrl ?? this.profileImageUrl,
      role: role ?? this.role,
      createdAt: createdAt ?? this.createdAt,
      lastLoginAt: lastLoginAt ?? this.lastLoginAt,
      isActive: isActive ?? this.isActive,
      metadata: metadata ?? this.metadata,
    );
  }

  // --- Computed Properties ---

  /// Get user's full name
  String get fullName => '$firstName $lastName';

  /// Get user's display name (full name or email if name not available)
  String get displayName {
    if (firstName.isNotEmpty && lastName.isNotEmpty) {
      return fullName;
    }
    return email;
  }

  /// Get user's initials for avatar display
  String get initials {
    String firstInitial = firstName.isNotEmpty ? firstName[0].toUpperCase() : '';
    String lastInitial = lastName.isNotEmpty ? lastName[0].toUpperCase() : '';
    
    if (firstInitial.isEmpty && lastInitial.isEmpty) {
      return email.isNotEmpty ? email[0].toUpperCase() : '?';
    }
    
    return '$firstInitial$lastInitial';
  }

  /// Check if user has profile image
  bool get hasProfileImage => profileImageUrl != null && profileImageUrl!.isNotEmpty;

  /// Check if user is admin
  bool get isAdmin => role == UserRole.admin;

  /// Check if user is premium member
  bool get isPremium => role == UserRole.premium;

  /// Get time since last login in human readable format
  String get lastSeenText {
    if (lastLoginAt == null) return 'Never';
    
    final now = DateTime.now();
    final difference = now.difference(lastLoginAt!);
    
    if (difference.inDays > 0) {
      return '${difference.inDays} day${difference.inDays == 1 ? '' : 's'} ago';
    } else if (difference.inHours > 0) {
      return '${difference.inHours} hour${difference.inHours == 1 ? '' : 's'} ago';
    } else if (difference.inMinutes > 0) {
      return '${difference.inMinutes} minute${difference.inMinutes == 1 ? '' : 's'} ago';
    } else {
      return 'Just now';
    }
  }

  // --- Equality and Hashing ---

  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is UserModel && other.id == id;
  }

  @override
  int get hashCode => id.hashCode;

  @override
  String toString() {
    return 'UserModel(id: $id, fullName: $fullName, email: $email, role: ${role.value})';
  }

  // --- Static Factory Methods ---

  /// Create empty/default user model
  static UserModel empty() {
    return UserModel(
      id: '',
      firstName: '',
      lastName: '',
      email: '',
      role: UserRole.user,
      createdAt: DateTime.now(),
    );
  }

  /// Create mock user for testing/development
  static UserModel mock({
    String? id,
    String? firstName,
    String? lastName,
    String? email,
    UserRole? role,
  }) {
    return UserModel(
      id: id ?? '1',
      firstName: firstName ?? 'John',
      lastName: lastName ?? 'Doe',
      email: email ?? 'john.doe@example.com',
      phoneNumber: '+1234567890',
      profileImageUrl: 'https://via.placeholder.com/150',
      role: role ?? UserRole.user,
      createdAt: DateTime.now().subtract(const Duration(days: 30)),
      lastLoginAt: DateTime.now().subtract(const Duration(hours: 2)),
      isActive: true,
      metadata: {
        'preferences': {
          'theme': 'light',
          'notifications': true,
        },
        'stats': {
          'login_count': 25,
          'last_activity': DateTime.now().toIso8601String(),
        }
      },
    );
  }
}
```

## data/models/product_model.dart
**Purpose**: Data model for product entity with pricing, categories, and inventory information. Handles e-commerce related data structure.

```dart
// Product data model with comprehensive e-commerce fields
// Benefits: Structured product data, price calculations, inventory management

/// Model class representing a product entity
/// Contains all necessary fields for e-commerce functionality
class ProductModel {
  // Core product information
  final String id;
  final String name;
  final String description;
  final String shortDescription;
  final List<String> imageUrls;
  final String categoryId;
  final String categoryName;
  final String? brand;
  final String? sku; // Stock Keeping Unit
  
  // Pricing information
  final double price;
  final double? originalPrice; // For showing discounts
  final String currency;
  final double? discount; // Discount percentage
  
  // Inventory and availability
  final int stockQuantity;
  final bool isInStock;
  final bool isActive;
  final int? minOrderQuantity;
  final int? maxOrderQuantity;
  
  // Product specifications
  final Map<String, dynamic>? specifications;
  final List<String>? tags;
  final double? weight;
  final Map<String, double>? dimensions; // width, height, depth
  
  // Ratings and reviews
  final double averageRating;
  final int reviewCount;
  
  // Timestamps
  final DateTime createdAt;
  final DateTime updatedAt;
  final DateTime? featuredUntil;

  /// Constructor with required and optional parameters
  const ProductModel({
    required this.id,
    required this.name,
    required this.description,
    required this.shortDescription,
    required this.imageUrls,
    required this.categoryId,
    required this.categoryName,
    this.brand,
    this.sku,
    required this.price,
    this.originalPrice,
    this.currency = 'USD',
    this.discount,
    required this.stockQuantity,
    this.isInStock = true,
    this.isActive = true,
    this.minOrderQuantity = 1,
    this.maxOrderQuantity,
    this.specifications,
    this.tags,
    this.weight,
    this.dimensions,
    this.averageRating = 0.0,
    this.reviewCount = 0,
    required this.createdAt,
    required this.updatedAt,
    this.featuredUntil,
  });

  /// Factory constructor to create ProductModel from JSON
  factory ProductModel.fromJson(Map<String, dynamic> json) {
    return ProductModel(
      id: json['id'] as String,
      name: json['name'] as String,
      description: json['description'] as String,
      shortDescription: json['short_description'] as String,
      imageUrls: List<String>.from(json['image_urls'] as List),
      categoryId: json['category_id'] as String,
      categoryName: json['category_name'] as String,
      brand: json['brand'] as String?,
      sku: json['sku'] as String?,
      price: (json['price'] as num).toDouble(),
      originalPrice: json['original_price'] != null 
          ? (json['original_price'] as num).toDouble()
          : null,
      currency: json['currency'] as String? ?? 'USD',
      discount: json['discount'] != null 
          ? (json['discount'] as num).toDouble()
          : null,
      stockQuantity: json['stock_quantity'] as int,
      isInStock: json['is_in_stock'] as bool? ?? true,
      isActive: json['is_active'] as bool? ?? true,
      minOrderQuantity: json['min_order_quantity'] as int?,
      maxOrderQuantity: json['max_order_quantity'] as int?,
      specifications: json['specifications'] as Map<String, dynamic>?,
      tags: json['tags'] != null ? List<String>.from(json['tags'] as List) : null,
      weight: json['weight'] != null ? (json['weight'] as num).toDouble() : null,
      dimensions: json['dimensions'] != null 
          ? Map<String, double>.from(
              (json['dimensions'] as Map).map((k, v) => MapEntry(k.toString(), (v as num).toDouble()))
            )
          : null,
      averageRating: (json['average_rating'] as num?)?.toDouble() ?? 0.0,
      reviewCount: json['review_count'] as int? ?? 0,
      createdAt: DateTime.parse(json['created_at'] as String),
      updatedAt: DateTime.parse(json['updated_at'] as String),
      featuredUntil: json['featured_until'] != null 
          ? DateTime.parse(json['featured_until'] as String)
          : null,
    );
  }

  /// Convert ProductModel to JSON
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'description': description,
      'short_description': shortDescription,
      'image_urls': imageUrls,
      'category_id': categoryId,
      'category_name': categoryName,
      'brand': brand,
      'sku': sku,
      'price': price,
      'original_price': originalPrice,
      'currency': currency,
      'discount': discount,
      'stock_quantity': stockQuantity,
      'is_in_stock': isInStock,
      'is_active': isActive,
      'min_order_quantity': minOrderQuantity,
      'max_order_quantity': maxOrderQuantity,
      'specifications': specifications,
      'tags': tags,
      'weight': weight,
      'dimensions': dimensions,
      'average_rating': averageRating,
      'review_count': reviewCount,
      'created_at': createdAt.toIso8601String(),
      'updated_at': updatedAt.toIso8601String(),
      'featured_until': featuredUntil?.toIso8601String(),
    };
  }

  /// Create a copy with updated fields
  ProductModel copyWith({
    String? id,
    String? name,
    String? description,
    String? shortDescription,
    List<String>? imageUrls,
    String? categoryId,
    String? categoryName,
    String? brand,
    String? sku,
    double? price,
    double? originalPrice,
    String? currency,
    double? discount,
    int? stockQuantity,
    bool? isInStock,
    bool? isActive,
    int? minOrderQuantity,
    int? maxOrderQuantity,
    Map<String, dynamic>? specifications,
    List<String>? tags,
    double? weight,
    Map<String, double>? dimensions,
    double? averageRating,
    int? reviewCount,
    DateTime? createdAt,
    DateTime? updatedAt,
    DateTime? featuredUntil,
  }) {
    return ProductModel(
      id: id ?? this.id,
      name: name ?? this.name,
      description: description ?? this.description,
      shortDescription: shortDescription ?? this.shortDescription,
      imageUrls: imageUrls ?? this.imageUrls,
      categoryId: categoryId ?? this.categoryId,
      categoryName: categoryName ?? this.categoryName,
      brand: brand ?? this.brand,
      sku: sku ?? this.sku,
      price: price ?? this.price,
      originalPrice: originalPrice ?? this.originalPrice,
      currency: currency ?? this.currency,
      discount: discount ?? this.discount,
      stockQuantity: stockQuantity ?? this.stockQuantity,
      isInStock: isInStock ?? this.isInStock,
      isActive: isActive ?? this.isActive,
      minOrderQuantity: minOrderQuantity ?? this.minOrderQuantity,
      maxOrderQuantity: maxOrderQuantity ?? this.maxOrderQuantity,
      specifications: specifications ?? this.specifications,
      tags: tags ?? this.tags,
      weight: weight ?? this.weight,
      dimensions: dimensions ?? this.dimensions,
      averageRating: averageRating ?? this.averageRating,
      reviewCount: reviewCount ?? this.reviewCount,
      createdAt: createdAt ?? this.createdAt,
      updatedAt: updatedAt ?? this.updatedAt,
      featuredUntil: featuredUntil ?? this.featuredUntil,
    );
  }

  // --- Computed Properties ---

  /// Get primary image URL (first image or placeholder)
  String get primaryImageUrl {
    if (imageUrls.isEmpty) {
      return 'https://via.placeholder.com/300x300?text=No+Image';
    }
    return imageUrls.first;
  }

  /// Check if product has discount
  bool get hasDiscount {
    return originalPrice != null && originalPrice! > price;
  }

  /// Calculate discount percentage
  double get discountPercentage {
    if (!hasDiscount) return 0.0;
    return ((originalPrice! - price) / originalPrice!) * 100;
  }

  /// Get formatted discount percentage
  String get formattedDiscount {
    if (!hasDiscount) return '';
    return '${discountPercentage.toStringAsFixed(0)}% OFF';
  }

  /// Get formatted price with currency
  String get formattedPrice {
    return '$currency ${price.toStringAsFixed(2)}';
  }

  /// Get formatted original price with currency
  String get formattedOriginalPrice {
    if (originalPrice == null) return '';
    return '$currency ${originalPrice!.toStringAsFixed(2)}';
  }

  /// Check if product is available for purchase
  bool get isAvailable {
    return isActive && isInStock && stockQuantity > 0;
  }

  /// Check if product is low in stock (less than 10 items)
  bool get isLowStock {
    return stockQuantity <= 10 && stockQuantity > 0;
  }

  /// Check if product is out of stock
  bool get isOutOfStock {
    return stockQuantity <= 0 || !isInStock;
  }

  /// Get stock status text
  String get stockStatusText {
    if (isOutOfStock) return 'Out of Stock';
    if (isLowStock) return 'Low Stock';
    return 'In Stock';
  }

  /// Check if product is featured (featured until date is in future)
  bool get isFeatured {
    if (featuredUntil == null) return false;
    return featuredUntil!.isAfter(DateTime.now());
  }

  /// Get rating stars as string representation
  String get ratingStars {
    final fullStars = averageRating.floor();
    final hasHalfStar = (averageRating - fullStars) >= 0.5;
    
    String stars = '★' * fullStars;
    if (hasHalfStar) stars += '☆';
    
    final emptyStars = 5 - stars.length;
    stars += '☆' * emptyStars;
    
    return stars;
  }

  /// Get review summary text
  String get reviewSummary {
    if (reviewCount == 0) return 'No reviews';
    return '${averageRating.toStringAsFixed(1)} (${reviewCount} review${reviewCount == 1 ? '' : 's'})';
  }

  // --- Validation Methods ---

  /// Validate if quantity can be ordered
  bool canOrderQuantity(int quantity) {
    if (quantity < (minOrderQuantity ?? 1)) return false;
    if (maxOrderQuantity != null && quantity > maxOrderQuantity!) return false;
    if (quantity > stockQuantity) return false;
    return true;
  }

  /// Get maximum quantity that can be ordered
  int get maxOrderableQuantity {
    int max = stockQuantity;
    if (maxOrderQuantity != null && maxOrderQuantity! < max) {
      max = maxOrderQuantity!;
    }
    return max;
  }

  // --- Equality and Hashing ---

  @override
  bool operator ==(Object other) {
    if (identical(this, other)) return true;
    return other is ProductModel && other.id == id;
  }

  @override
  int get hashCode => id.hashCode;

  @override
  String toString() {
    return 'ProductModel(id: $id, name: $name, price: $formattedPrice, stock: $stockQuantity)';
  }

  // --- Static Factory Methods ---

  /// Create empty product model
  static ProductModel empty() {
    return ProductModel(
      id: '',
      name: '',
      description: '',
      shortDescription: '',
      imageUrls: [],
      categoryId: '',
      categoryName: '',
      price: 0.0,
      stockQuantity: 0,
      createdAt: DateTime.now(),
      updatedAt: DateTime.now(),
    );
  }

  /// Create mock product for testing/development
  static ProductModel mock({
    String? id,
    String? name,
    double? price,
    String? category,
  }) {
    return ProductModel(
      id: id ?? '1',
      name: name ?? 'Sample Product',
      description: 'This is a detailed description of the sample product with all the features and benefits explained.',
      shortDescription: 'Sample product for demonstration',
      imageUrls: [
        'https://via.placeholder.com/300x300?text=Product+1',
        'https://via.placeholder.com/300x300?text=Product+2',
      ],
      categoryId: '1',
      categoryName: category ?? 'Electronics',
      brand: 'Sample Brand',
      sku: 'SKU-001',
      price: price ?? 99.99,
      originalPrice: price != null ? price + 20 : 119.99,
      currency: 'USD',
      discount: 20.0,
      stockQuantity: 25,
      isInStock: true,
      isActive: true,
      minOrderQuantity: 1,
      maxOrderQuantity: 5,
      specifications: {
        'Color': 'Black',
        'Material': 'Plastic',
        'Warranty': '1 Year',
      },
      tags: ['popular', 'electronics', 'bestseller'],
      weight: 0.5,
      dimensions: {
        'width': 10.0,
        'height': 5.0,
        'depth': 2.0,
      },
      averageRating: 4.5,
      reviewCount: 128,
      createdAt: DateTime.now().subtract(const Duration(days: 30)),
      updatedAt: DateTime.now().subtract(const Duration(days: 1)),
      featuredUntil: DateTime.now().add(const Duration(days: 7)),
    );
  }
}
```

## data/models/api_response_model.dart
**Purpose**: Provides a standardized wrapper for all API responses, ensuring consistent error handling and success states across the application.

```dart
// Generic API response wrapper that standardizes all network responses
// Helps maintain consistent error handling and success states throughout the app
class ApiResponse<T> {
  final bool success;           // Indicates if the API call was successful
  final String? message;        // Optional message from the server
  final T? data;               // Generic data payload
  final int? statusCode;       // HTTP status code
  final String? error;         // Error message if request failed

  // Constructor for creating API response instances
  ApiResponse({
    required this.success,
    this.message,
    this.data,
    this.statusCode,
    this.error,
  });

  // Factory constructor for successful responses
  // Makes it easy to create success responses with data
  factory ApiResponse.success({
    required T data,
    String? message,
    int? statusCode,
  }) {
    return ApiResponse<T>(
      success: true,
      data: data,
      message: message ?? 'Request successful',
      statusCode: statusCode ?? 200,
    );
  }

  // Factory constructor for error responses
  // Standardizes error response creation
  factory ApiResponse.error({
    required String error,
    String? message,
    int? statusCode,
  }) {
    return ApiResponse<T>(
      success: false,
      error: error,
      message: message,
      statusCode: statusCode ?? 500,
    );
  }

  // Convert API response to JSON (for caching purposes)
  Map<String, dynamic> toJson() {
    return {
      'success': success,
      'message': message,
      'data': data,
      'statusCode': statusCode,
      'error': error,
    };
  }

  // Create API response from JSON (for cache retrieval)
  factory ApiResponse.fromJson(Map<String, dynamic> json) {
    return ApiResponse<T>(
      success: json['success'] ?? false,
      message: json['message'],
      data: json['data'],
      statusCode: json['statusCode'],
      error: json['error'],
    );
  }

  // Helper method to check if response has data
  bool get hasData => data != null;

  // Helper method to check if response has error
  bool get hasError => error != null;

  @override
  String toString() {
    return 'ApiResponse(success: $success, message: $message, statusCode: $statusCode)';
  }
}
```

## data/repositories/user_repository.dart
**Purpose**: Implements the Repository pattern to abstract data access logic for user-related operations, providing a clean interface between the data layer and business logic.

```dart
import '../models/user_model.dart';
import '../models/api_response_model.dart';
import '../../core/services/api_service.dart';
import '../../core/services/storage_service.dart';

// User repository that handles all user-related data operations
// Abstracts the data source (API, local storage) from the controllers
class UserRepository {
  final ApiService _apiService;        // Handles network requests
  final StorageService _storageService; // Handles local data persistence

  // Constructor with dependency injection
  UserRepository({
    required ApiService apiService,
    required StorageService storageService,
  })  : _apiService = apiService,
        _storageService = storageService;

  // Fetch user profile from API
  // Returns standardized API response with user data
  Future<ApiResponse<UserModel>> getUserProfile(String userId) async {
    try {
      // First check if user data exists in local storage (cache)
      final cachedUser = await _getCachedUser(userId);
      if (cachedUser != null) {
        return ApiResponse.success(data: cachedUser, message: 'User loaded from cache');
      }

      // If not cached, fetch from API
      final response = await _apiService.get('/users/$userId');
      
      if (response.success && response.data != null) {
        final user = UserModel.fromJson(response.data);
        // Cache the user data for offline access
        await _cacheUser(user);
        return ApiResponse.success(data: user, message: 'User profile loaded');
      } else {
        return ApiResponse.error(error: response.error ?? 'Failed to load user profile');
      }
    } catch (e) {
      // Handle any unexpected errors
      return ApiResponse.error(error: 'An error occurred: ${e.toString()}');
    }
  }

  // Update user profile
  // Handles both API update and local cache update
  Future<ApiResponse<UserModel>> updateUserProfile(UserModel user) async {
    try {
      final response = await _apiService.put('/users/${user.id}', user.toJson());
      
      if (response.success && response.data != null) {
        final updatedUser = UserModel.fromJson(response.data);
        // Update local cache with new data
        await _cacheUser(updatedUser);
        return ApiResponse.success(data: updatedUser, message: 'Profile updated successfully');
      } else {
        return ApiResponse.error(error: response.error ?? 'Failed to update profile');
      }
    } catch (e) {
      return ApiResponse.error(error: 'Update failed: ${e.toString()}');
    }
  }

  // Get all users (for user listing screens)
  Future<ApiResponse<List<UserModel>>> getAllUsers() async {
    try {
      final response = await _apiService.get('/users');
      
      if (response.success && response.data != null) {
        final List<dynamic> usersJson = response.data;
        final users = usersJson.map((json) => UserModel.fromJson(json)).toList();
        return ApiResponse.success(data: users, message: 'Users loaded successfully');
      } else {
        return ApiResponse.error(error: response.error ?? 'Failed to load users');
      }
    } catch (e) {
      return ApiResponse.error(error: 'Loading failed: ${e.toString()}');
    }
  }

  // Delete user account
  Future<ApiResponse<bool>> deleteUser(String userId) async {
    try {
      final response = await _apiService.delete('/users/$userId');
      
      if (response.success) {
        // Remove from local cache
        await _removeCachedUser(userId);
        return ApiResponse.success(data: true, message: 'User deleted successfully');
      } else {
        return ApiResponse.error(error: response.error ?? 'Failed to delete user');
      }
    } catch (e) {
      return ApiResponse.error(error: 'Delete failed: ${e.toString()}');
    }
  }

  // Private method to cache user data locally
  Future<void> _cacheUser(UserModel user) async {
    await _storageService.setString('user_${user.id}', user.toJson().toString());
  }

  // Private method to get cached user data
  Future<UserModel?> _getCachedUser(String userId) async {
    final cachedData = await _storageService.getString('user_$userId');
    if (cachedData != null) {
      try {
        // Parse the cached JSON string back to UserModel
        final Map<String, dynamic> json = {};  // In real app, parse the string
        return UserModel.fromJson(json);
      } catch (e) {
        // If parsing fails, return null to fetch from API
        return null;
      }
    }
    return null;
  }

  // Private method to remove cached user data
  Future<void> _removeCachedUser(String userId) async {
    await _storageService.remove('user_$userId');
  }

  // Clear all cached user data (useful for logout)
  Future<void> clearUserCache() async {
    await _storageService.clear();
  }
}
```

## data/repositories/product_repository.dart
**Purpose**: Manages product data operations, implementing caching strategies and providing a consistent interface for product-related data access.

```dart
import '../models/product_model.dart';
import '../models/api_response_model.dart';
import '../../core/services/api_service.dart';
import '../../core/services/storage_service.dart';

// Product repository handling all product-related data operations
// Provides abstraction layer between data sources and business logic
class ProductRepository {
  final ApiService _apiService;
  final StorageService _storageService;

  ProductRepository({
    required ApiService apiService,
    required StorageService storageService,
  })  : _apiService = apiService,
        _storageService = storageService;

  // Fetch all products with optional filtering and pagination
  Future<ApiResponse<List<ProductModel>>> getProducts({
    int page = 1,
    int limit = 10,
    String? category,
    String? searchQuery,
  }) async {
    try {
      // Create query parameters for API request
      final Map<String, dynamic> queryParams = {
        'page': page,
        'limit': limit,
        if (category != null) 'category': category,
        if (searchQuery != null) 'search': searchQuery,
      };

      // Check cache first for better performance
      final cacheKey = _generateCacheKey('products', queryParams);
      final cachedProducts = await _getCachedProducts(cacheKey);
      
      if (cachedProducts != null && cachedProducts.isNotEmpty) {
        return ApiResponse.success(
          data: cachedProducts, 
          message: 'Products loaded from cache'
        );
      }

      // Fetch from API if not in cache
      final response = await _apiService.get('/products', queryParams: queryParams);
      
      if (response.success && response.data != null) {
        final List<dynamic> productsJson = response.data;
        final products = productsJson.map((json) => ProductModel.fromJson(json)).toList();
        
        // Cache the results for future use
        await _cacheProducts(cacheKey, products);
        
        return ApiResponse.success(data: products, message: 'Products loaded successfully');
      } else {
        return ApiResponse.error(error: response.error ?? 'Failed to load products');
      }
    } catch (e) {
      return ApiResponse.error(error: 'Error loading products: ${e.toString()}');
    }
  }

  // Get single product by ID
  Future<ApiResponse<ProductModel>> getProductById(String productId) async {
    try {
      // Check cache first
      final cachedProduct = await _getCachedProduct(productId);
      if (cachedProduct != null) {
        return ApiResponse.success(data: cachedProduct, message: 'Product loaded from cache');
      }

      final response = await _apiService.get('/products/$productId');
      
      if (response.success && response.data != null) {
        final product = ProductModel.fromJson(response.data);
        // Cache individual product
        await _cacheProduct(product);
        return ApiResponse.success(data: product, message: 'Product loaded successfully');
      } else {
        return ApiResponse.error(error: response.error ?? 'Product not found');
      }
    } catch (e) {
      return ApiResponse.error(error: 'Error loading product: ${e.toString()}');
    }
  }

  // Get products by category
  Future<ApiResponse<List<ProductModel>>> getProductsByCategory(String category) async {
    return getProducts(category: category);
  }

  // Search products by name or description
  Future<ApiResponse<List<ProductModel>>> searchProducts(String query) async {
    return getProducts(searchQuery: query);
  }

  // Get featured products (mock implementation)
  Future<ApiResponse<List<ProductModel>>> getFeaturedProducts() async {
    try {
      final response = await _apiService.get('/products/featured');
      
      if (response.success && response.data != null) {
        final List<dynamic> productsJson = response.data;
        final products = productsJson.map((json) => ProductModel.fromJson(json)).toList();
        return ApiResponse.success(data: products, message: 'Featured products loaded');
      } else {
        return ApiResponse.error(error: response.error ?? 'Failed to load featured products');
      }
    } catch (e) {
      return ApiResponse.error(error: 'Error loading featured products: ${e.toString()}');
    }
  }

  // Private method to generate cache keys
  String _generateCacheKey(String prefix, Map<String, dynamic> params) {
    final sortedParams = params.entries.toList()..sort((a, b) => a.key.compareTo(b.key));
    final paramString = sortedParams.map((e) => '${e.key}=${e.value}').join('&');
    return '${prefix}_$paramString';
  }

  // Private method to cache product list
  Future<void> _cacheProducts(String cacheKey, List<ProductModel> products) async {
    final productsJson = products.map((p) => p.toJson()).toList();
    await _storageService.setString(cacheKey, productsJson.toString());
  }

  // Private method to get cached product list
  Future<List<ProductModel>?> _getCachedProducts(String cacheKey) async {
    final cachedData = await _storageService.getString(cacheKey);
    if (cachedData != null) {
      try {
        // In real implementation, properly parse JSON string
        // This is a simplified mock
        return []; // Return empty list for now
      } catch (e) {
        return null;
      }
    }
    return null;
  }

  // Private method to cache single product
  Future<void> _cacheProduct(ProductModel product) async {
    await _storageService.setString('product_${product.id}', product.toJson().toString());
  }

  // Private method to get cached single product
  Future<ProductModel?> _getCachedProduct(String productId) async {
    final cachedData = await _storageService.getString('product_$productId');
    if (cachedData != null) {
      try {
        // In real implementation, properly parse JSON string
        final Map<String, dynamic> json = {}; // Mock empty map
        return ProductModel.fromJson(json);
      } catch (e) {
        return null;
      }
    }
    return null;
  }

  // Clear product cache
  Future<void> clearProductCache() async {
    // In real implementation, remove only product-related cache entries
    await _storageService.clear();
  }
}
```

## data/providers/user_provider.dart
**Purpose**: Manages user state using Provider pattern with ChangeNotifier, providing reactive state management for user-related data across the application.

```dart
import 'package:flutter/foundation.dart';
import '../models/user_model.dart';
import '../repositories/user_repository.dart';
import '../../shared/enums/app_state.dart';

// User provider managing user state throughout the application
// Uses ChangeNotifier to provide reactive state updates to widgets
class UserProvider extends ChangeNotifier {
  final UserRepository _userRepository;

  UserProvider({required UserRepository userRepository})
      : _userRepository = userRepository;

  // Private state variables
  UserModel? _currentUser;              // Currently logged-in user
  List<UserModel> _users = [];          // List of all users
  AppState _state = AppState.initial;   // Current loading state
  String? _errorMessage;                // Error message if any operation fails

  // Public getters to access private state
  // These provide read-only access to the state from widgets
  UserModel? get currentUser => _currentUser;
  List<UserModel> get users => List.unmodifiable(_users); // Immutable copy
  AppState get state => _state;
  String? get errorMessage => _errorMessage;
  bool get isLoading => _state == AppState.loading;
  bool get hasError => _state == AppState.error;
  bool get isLoggedIn => _currentUser != null;

  // Load user profile by ID
  // Updates state and notifies listeners throughout the process
  Future<void> loadUserProfile(String userId) async {
    _setState(AppState.loading);
    
    try {
      final response = await _userRepository.getUserProfile(userId);
      
      if (response.success && response.data != null) {
        _currentUser = response.data;
        _setState(AppState.success);
      } else {
        _setError(response.error ?? 'Failed to load user profile');
      }
    } catch (e) {
      _setError('An unexpected error occurred: ${e.toString()}');
    }
  }

  // Update current user profile
  Future<void> updateUserProfile(UserModel updatedUser) async {
    _setState(AppState.loading);
    
    try {
      final response = await _userRepository.updateUserProfile(updatedUser);
      
      if (response.success && response.data != null) {
        _currentUser = response.data;
        _setState(AppState.success);
        
        // Show success message (in real app, you might use a snackbar service)
        debugPrint('Profile updated successfully');
      } else {
        _setError(response.error ?? 'Failed to update profile');
      }
    } catch (e) {
      _setError('Update failed: ${e.toString()}');
    }
  }

  // Load all users (for admin screens or user listings)
  Future<void> loadAllUsers() async {
    _setState(AppState.loading);
    
    try {
      final response = await _userRepository.getAllUsers();
      
      if (response.success && response.data != null) {
        _users = response.data!;
        _setState(AppState.success);
      } else {
        _setError(response.error ?? 'Failed to load users');
      }
    } catch (e) {
      _setError('Error loading users: ${e.toString()}');
    }
  }

  // Delete user account
  Future<bool> deleteUser(String userId) async {
    _setState(AppState.loading);
    
    try {
      final response = await _userRepository.deleteUser(userId);
      
      if (response.success) {
        // Remove user from local list if it exists
        _users.removeWhere((user) => user.id == userId);
        
        // If deleting current user, log them out
        if (_currentUser?.id == userId) {
          _currentUser = null;
        }
        
        _setState(AppState.success);
        return true;
      } else {
        _setError(response.error ?? 'Failed to delete user');
        return false;
      }
    } catch (e) {
      _setError('Delete failed: ${e.toString()}');
      return false;
    }
  }

  // Login user (sets current user)
  void loginUser(UserModel user) {
    _currentUser = user;
    _setState(AppState.success);
    notifyListeners();
  }

  // Logout current user
  void logoutUser() {
    _currentUser = null;
    _users.clear();
    _setState(AppState.initial);
    // Clear repository cache
    _userRepository.clearUserCache();
    notifyListeners();
  }

  // Search users by name
  List<UserModel> searchUsers(String query) {
    if (query.isEmpty) return _users;
    
    return _users.where((user) => 
      user.name.toLowerCase().contains(query.toLowerCase()) ||
      user.email.toLowerCase().contains(query.toLowerCase())
    ).toList();
  }

  // Get user by ID from current users list
  UserModel? getUserById(String userId) {
    try {
      return _users.firstWhere((user) => user.id == userId);
    } catch (e) {
      return null;
    }
  }

  // Private method to update state and notify listeners
  void _setState(AppState newState) {
    _state = newState;
    if (newState != AppState.error) {
      _errorMessage = null; // Clear error when state changes to non-error
    }
    notifyListeners(); // Notify all listening widgets to rebuild
  }

  // Private method to set error state
  void _setError(String error) {
    _errorMessage = error;
    _state = AppState.error;
    notifyListeners();
  }

  // Clear error state
  void clearError() {
    _errorMessage = null;
    if (_state == AppState.error) {
      _state = AppState.initial;
    }
    notifyListeners();
  }

  // Refresh current user data
  Future<void> refreshCurrentUser() async {
    if (_currentUser != null) {
      await loadUserProfile(_currentUser!.id);
    }
  }

  @override
  void dispose() {
    // Clean up resources when provider is disposed
    super.dispose();
  }
}
```

## data/providers/product_provider.dart
**Purpose**: Manages product state and business logic using Provider pattern, handling product listings, search, filtering, and favorites with reactive state updates.

```dart
import 'package:flutter/foundation.dart';
import '../models/product_model.dart';
import '../repositories/product_repository.dart';
import '../../shared/enums/app_state.dart';

// Product provider managing product state and operations
// Handles product listings, search, filtering, and favorites
class ProductProvider extends ChangeNotifier {
  final ProductRepository _productRepository;

  ProductProvider({required ProductRepository productRepository})
      : _productRepository = productRepository;

  // Private state variables
  List<ProductModel> _products = [];           // Main product list
  List<ProductModel> _featuredProducts = [];   // Featured products
  List<ProductModel> _searchResults = [];      // Search results
  List<String> _favoriteProductIds = [];       // User's favorite product IDs
  ProductModel? _selectedProduct;              // Currently selected product
  AppState _state = AppState.initial;          // Current loading state
  String? _errorMessage;                       // Error message
  String _currentCategory = 'all';             // Current category filter
  String _searchQuery = '';                    // Current search query
  int _currentPage = 1;                        // Current page for pagination
  bool _hasMoreProducts = true;                // Whether more products can be loaded

  // Public getters for accessing state
  List<ProductModel> get products => List.unmodifiable(_products);
  List<ProductModel> get featuredProducts => List.unmodifiable(_featuredProducts);
  List<ProductModel> get searchResults => List.unmodifiable(_searchResults);
  List<String> get favoriteProductIds => List.unmodifiable(_favoriteProductIds);
  ProductModel? get selectedProduct => _selectedProduct;
  AppState get state => _state;
  String? get errorMessage => _errorMessage;
  String get currentCategory => _currentCategory;
  String get searchQuery => _searchQuery;
  bool get isLoading => _state == AppState.loading;
  bool get hasError => _state == AppState.error;
  bool get hasMoreProducts => _hasMoreProducts;

  // Load products with optional category filtering
  Future<void> loadProducts({
    String category = 'all',
    bool refresh = false,
  }) async {
    // If refreshing, reset pagination
    if (refresh) {
      _currentPage = 1;
      _products.clear();
      _hasMoreProducts = true;
    }

    _setState(AppState.loading);
    _currentCategory = category;
    
    try {
      final response = await _productRepository.getProducts(
        page: _currentPage,
        limit: 10,
        category: category == 'all' ? null : category,
      );
      
      if (response.success && response.data != null) {
        final newProducts = response.data!;
        
        if (refresh) {
          _products = newProducts;
        } else {
          _products.addAll(newProducts);
        }
        
        // Check if there are more products to load
        _hasMoreProducts = newProducts.length >= 10;
        _currentPage++;
        
        _setState(AppState.success);
      } else {
        _setError(response.error ?? 'Failed to load products');
      }
    } catch (e) {
      _setError('Error loading products: ${e.toString()}');
    }
  }

  // Load more products (pagination)
  Future<void> loadMoreProducts() async {
    if (!_hasMoreProducts || _state == AppState.loading) return;
    
    await loadProducts(category: _currentCategory, refresh: false);
  }

  // Load featured products
  Future<void> loadFeaturedProducts() async {
    try {
      final response = await _productRepository.getFeaturedProducts();
      
      if (response.success && response.data != null) {
        _featuredProducts = response.data!;
        notifyListeners();
      } else {
        debugPrint('Failed to load featured products: ${response.error}');
      }
    } catch (e) {
      debugPrint('Error loading featured products: ${e.toString()}');
    }
  }

  // Search products
  Future<void> searchProducts(String query) async {
    _searchQuery = query;
    
    if (query.isEmpty) {
      _searchResults.clear();
      notifyListeners();
      return;
    }

    _setState(AppState.loading);
    
    try {
      final response = await _productRepository.searchProducts(query);
      
      if (response.success && response.data != null) {
        _searchResults = response.data!;
        _setState(AppState.success);
      } else {
        _setError(response.error ?? 'Search failed');
      }
    } catch (e) {
      _setError('Search error: ${e.toString()}');
    }
  }

  // Load single product by ID
  Future<void> loadProductById(String productId) async {
    _setState(AppState.loading);
    
    try {
      final response = await _productRepository.getProductById(productId);
      
      if (response.success && response.data != null) {
        _selectedProduct = response.data;
        _setState(AppState.success);
      } else {
        _setError(response.error ?? 'Product not found');
      }
    } catch (e) {
      _setError('Error loading product: ${e.toString()}');
    }
  }

  // Get products by category
  Future<void> loadProductsByCategory(String category) async {
    await loadProducts(category: category, refresh: true);
  }

  // Toggle product favorite status
  void toggleFavorite(String productId) {
    if (_favoriteProductIds.contains(productId)) {
      _favoriteProductIds.remove(productId);
    } else {
      _favoriteProductIds.add(productId);
    }
    notifyListeners();
    
    // In a real app, you would also save this to backend/local storage
    _saveFavoritesToStorage();
  }

  // Check if product is favorite
  bool isFavorite(String productId) {
    return _favoriteProductIds.contains(productId);
  }

  // Get favorite products
  List<ProductModel> get favoriteProducts {
    return _products.where((product) => 
      _favoriteProductIds.contains(product.id)
    ).toList();
  }

  // Clear search results
  void clearSearch() {
    _searchQuery = '';
    _searchResults.clear();
    notifyListeners();
  }

  // Filter products by price range
  List<ProductModel> filterProductsByPrice(double minPrice, double maxPrice) {
    return _products.where((product) => 
      product.price >= minPrice && product.price <= maxPrice
    ).toList();
  }

  // Get available categories from current products
  List<String> get availableCategories {
    final categories = _products.map((product) => product.category).toSet().toList();
    categories.sort();
    return ['all', ...categories];
  }

  // Get products sorted by price
  List<ProductModel> getProductsSortedByPrice({bool ascending = true}) {
    final sortedProducts = List<ProductModel>.from(_products);
    sortedProducts.sort((a, b) => ascending 
      ? a.price.compareTo(b.price)
      : b.price.compareTo(a.price)
    );
    return sortedProducts;
  }

  // Private method to save favorites to storage
  Future<void> _saveFavoritesToStorage() async {
    // In real implementation, save to SharedPreferences or backend
    debugPrint('Saving favorites: $_favoriteProductIds');
  }

  // Load favorites from storage
  Future<void> loadFavoritesFromStorage() async {
    // In real implementation, load from SharedPreferences or backend
    // For now, use mock data
    _favoriteProductIds = ['1', '3', '5']; // Mock favorite IDs
    notifyListeners();
  }

  // Private method to update state
  void _setState(AppState newState) {
    _state = newState;
    if (newState != AppState.error) {
      _errorMessage = null;
    }
    notifyListeners();
  }

  // Private method to set error
  void _setError(String error) {
    _errorMessage = error;
    _state = AppState.error;
    notifyListeners();
  }

  // Clear error
  void clearError() {
    _errorMessage = null;
    if (_state == AppState.error) {
      _state = AppState.initial;
    }
    notifyListeners();
  }

  // Refresh all data
  Future<void> refreshData() async {
    await Future.wait([
      loadProducts(refresh: true),
      loadFeaturedProducts(),
    ]);
  }

  @override
  void dispose() {
    // Clean up resources
    super.dispose();
  }
}
```

## data/providers/theme_provider.dart
**Purpose**: Manages application theme state, allowing users to switch between light and dark themes with persistence across app sessions.

```dart
import 'package:flutter/material.dart';
import '../../core/services/storage_service.dart';

// Theme provider managing application theme state
// Handles light/dark theme switching with persistence
class ThemeProvider extends ChangeNotifier {
  final StorageService _storageService;
  
  // Private state variables
  ThemeMode _themeMode = ThemeMode.system;  // Current theme mode
  bool _isDarkMode = false;                 // Whether dark mode is active

  ThemeProvider({required StorageService storageService})
      : _storageService = storageService {
    _loadThemeFromStorage();
  }

  // Public getters for accessing theme state
  ThemeMode get themeMode => _themeMode;
  bool get isDarkMode => _isDarkMode;
  bool get isLightMode => !_isDarkMode && _themeMode != ThemeMode.system;
  bool get isSystemMode => _themeMode == ThemeMode.system;

  // Set theme mode and persist to storage
  Future<void> setThemeMode(ThemeMode mode) async {
    _themeMode = mode;
    
    // Update dark mode flag based on theme mode
    switch (mode) {
      case ThemeMode.light:
        _isDarkMode = false;
        break;
      case ThemeMode.dark:
        _isDarkMode = true;
        break;
      case ThemeMode.system:
        // Get system brightness to determine if system is in dark mode
        _isDarkMode = _getSystemBrightness() == Brightness.dark;
        break;
    }
    
    // Save theme preference to storage
    await _saveThemeToStorage();
    
    // Notify listeners to rebuild UI with new theme
    notifyListeners();
  }

  // Toggle between light and dark themes
  Future<void> toggleTheme() async {
    if (_themeMode == ThemeMode.system) {
      // If system mode, switch to opposite of current system theme
      await setThemeMode(_isDarkMode ? ThemeMode.light : ThemeMode.dark);
    } else if (_themeMode == ThemeMode.light) {
      await setThemeMode(ThemeMode.dark);
    } else {
      await setThemeMode(ThemeMode.light);
    }
  }

  // Switch to light theme
  Future<void> setLightTheme() async {
    await setThemeMode(ThemeMode.light);
  }

  // Switch to dark theme
  Future<void> setDarkTheme() async {
    await setThemeMode(ThemeMode.dark);
  }

  // Use system theme
  Future<void> setSystemTheme() async {
    await setThemeMode(ThemeMode.system);
  }

  // Update theme when system brightness changes
  void updateSystemTheme(Brightness systemBrightness) {
    if (_themeMode == ThemeMode.system) {
      final newIsDarkMode = systemBrightness == Brightness.dark;
      if (_isDarkMode != newIsDarkMode) {
        _isDarkMode = newIsDarkMode;
        notifyListeners();
      }
    }
  }

  // Private method to load theme from storage
  Future<void> _loadThemeFromStorage() async {
    try {
      final themeString = await _storageService.getString('theme_mode');
      if (themeString != null) {
        // Parse stored theme mode
        switch (themeString) {
          case 'light':
            _themeMode = ThemeMode.light;
            _isDarkMode = false;
            break;
          case 'dark':
            _themeMode = ThemeMode.dark;
            _isDarkMode = true;
            break;
          case 'system':
          default:
            _themeMode = ThemeMode.system;
            _isDarkMode = _getSystemBrightness() == Brightness.dark;
            break;
        }
      } else {
        // Default to system theme if no preference saved
        _themeMode = ThemeMode.system;
        _isDarkMode = _getSystemBrightness() == Brightness.dark;
      }
      notifyListeners();
    } catch (e) {
      // If loading fails, use system default
      _themeMode = ThemeMode.system;
      _isDarkMode = _getSystemBrightness() == Brightness.dark;
    }
  }

  // Private method to save theme to storage
  Future<void> _saveThemeToStorage() async {
    try {
      String themeString;
      switch (_themeMode) {
        case ThemeMode.light:
          themeString = 'light';
          break;
        case ThemeMode.dark:
          themeString = 'dark';
          break;
        case ThemeMode.system:
          themeString = 'system';
          break;
      }
      await _storageService.setString('theme_mode', themeString);
    } catch (e) {
      // Handle storage error silently
      debugPrint('Failed to save theme preference: $e');
    }
  }

  // Private method to get system brightness
  Brightness _getSystemBrightness() {
    // In a real app, you would get this from MediaQuery or WidgetsBinding
    // For this example, we'll assume light mode as default
    return Brightness.light; // Mock implementation
  }

  // Get appropriate theme data for current mode
  ThemeData getThemeData(BuildContext context) {
    switch (_themeMode) {
      case ThemeMode.light:
        return ThemeData.light();
      case ThemeMode.dark:
        return ThemeData.dark();
      case ThemeMode.system:
        return MediaQuery.of(context).platformBrightness == Brightness.dark
            ? ThemeData.dark()
            : ThemeData.light();
    }
  }

  // Get theme mode display name
  String get themeModeDisplayName {
    switch (_themeMode) {
      case ThemeMode.light:
        return 'Light';
      case ThemeMode.dark:
        return 'Dark';
      case ThemeMode.system:
        return 'System';
    }
  }

  @override
  void dispose() {
    super.dispose();
  }
}
```

## presentation/controllers/home_controller.dart
**Purpose**: Implements the Controller layer of MVC pattern for the home screen, managing business logic, coordinating between models and views, and handling user interactions.

```dart
import 'package:flutter/foundation.dart';
import '../../data/providers/user_provider.dart';
import '../../data/providers/product_provider.dart';
import '../../core/services/navigation_service.dart';
import '../../shared/enums/app_state.dart';

// Home controller managing home screen business logic
// Acts as intermediary between home view and data providers
class HomeController extends ChangeNotifier {
  final UserProvider _userProvider;
  final ProductProvider _productProvider;
  final NavigationService _navigationService;

  HomeController({
    required UserProvider userProvider,
    required ProductProvider productProvider,
    required NavigationService navigationService,
  })  : _userProvider = userProvider,
        _productProvider = productProvider,
        _navigationService = navigationService;

  // Private state variables specific to home screen
  bool _isRefreshing = false;
  String? _welcomeMessage;
  List<String> _quickActions = [];

  // Public getters for accessing controller state
  bool get isRefreshing => _isRefreshing;
  String? get welcomeMessage => _welcomeMessage;
  List<String> get quickActions => List.unmodifiable(_quickActions);
  
  // Delegate getters to access provider states
  bool get isLoading => _productProvider.isLoading || _userProvider.isLoading;
  bool get hasError => _productProvider.hasError || _userProvider.hasError;
  String? get errorMessage => _productProvider.errorMessage ?? _userProvider.errorMessage;

  // Initialize home screen data
  // This method is called when home screen is first loaded
  Future<void> initializeHome() async {
    try {
      // Set up welcome message based on current user
      _setupWelcomeMessage();
      
      // Set up quick actions for the user
      _setupQuickActions();
      
      // Load initial data concurrently for better performance
      await Future.wait([
        _productProvider.loadFeaturedProducts(),
        _productProvider.loadProducts(refresh: true),
        _loadUserDashboardData(),
      ]);

      notifyListeners();
    } catch (e) {
      debugPrint('Error initializing home: $e');
    }
  }

  // Refresh home screen data
  // Called when user pulls to refresh
  Future<void> refreshHome() async {
    if (_isRefreshing) return; // Prevent multiple simultaneous refreshes
    
    _isRefreshing = true;
    notifyListeners();

    try {
      // Refresh all home screen data
      await Future.wait([
        _productProvider.refreshData(),
        _refreshUserData(),
      ]);

      // Update dynamic content
      _setupWelcomeMessage();
      _setupQuickActions();

    } catch (e) {
      debugPrint('Error refreshing home: $e');
    } finally {
      _isRefreshing = false;
      notifyListeners();
    }
  }

  // Handle search action from home screen
  Future<void> performSearch(String query) async {
    if (query.trim().isEmpty) return;

    try {
      await _productProvider.searchProducts(query.trim());
      // Navigate to search results screen
      _navigationService.navigateToNamed('/search-results');
    } catch (e) {
      debugPrint('Search error: $e');
    }
  }

  // Handle category selection
  Future<void> selectCategory(String category) async {
    try {
      await _productProvider.loadProductsByCategory(category);
      // Navigate to category products screen
      _navigationService.navigateToNamed('/category-products', arguments: category);
    } catch (e) {
      debugPrint('Category selection error: $e');
    }
  }

  // Handle product selection from featured or recent products
  void selectProduct(String productId) {
    _navigationService.navigateToNamed('/product-details', arguments: productId);
  }

  // Handle quick action selection
  Future<void> executeQuickAction(String action) async {
    switch (action) {
      case 'view_profile':
        _navigationService.navigateToNamed('/profile');
        break;
      case 'view_favorites':
        _navigationService.navigateToNamed('/favorites');
        break;
      case 'view_orders':
        _navigationService.navigateToNamed('/orders');
        break;
      case 'view_cart':
        _navigationService.navigateToNamed('/cart');
        break;
      default:
        debugPrint('Unknown quick action: $action');
    }
  }

  // Get personalized recommendations based on user data
  List<String> getPersonalizedRecommendations() {
    // In a real app, this would use ML algorithms or user behavior analysis
    // For now, return mock recommendations
    return [
      'Electronics',
      'Books',
      'Clothing',
      'Home & Garden',
    ];
  }

  // Check if user should see onboarding
  bool shouldShowOnboarding() {
    return _userProvider.currentUser == null;
  }

  // Handle onboarding completion
  Future<void> completeOnboarding() async {
    // In real app, mark onboarding as completed in user preferences
    _navigationService.navigateToNamed('/login');
  }

  // Get dashboard statistics for home screen
  Map<String, dynamic> getDashboardStats() {
    return {
      'total_products': _productProvider.products.length,
      'featured_products': _productProvider.featuredProducts.length,
      'user_favorites': _productProvider.favoriteProducts.length,
      'categories': _productProvider.availableCategories.length - 1, // Exclude 'all'
    };
  }

  // Private method to set up welcome message
  void _setupWelcomeMessage() {
    final currentUser = _userProvider.currentUser;
    final timeOfDay = DateTime.now().hour;
    
    String greeting;
    if (timeOfDay < 12) {
      greeting = 'Good Morning';
    } else if (timeOfDay < 17) {
      greeting = 'Good Afternoon';
    } else {
      greeting = 'Good Evening';
    }

    if (currentUser != null) {
      _welcomeMessage = '$greeting, ${currentUser.name}!';
    } else {
      _welcomeMessage = '$greeting! Welcome to our store.';
    }
  }

  // Private method to set up quick actions based on user state
  void _setupQuickActions() {
    _quickActions.clear();
    
    if (_userProvider.isLoggedIn) {
      _quickActions.addAll([
        'view_profile',
        'view_favorites',
        'view_orders',
        'view_cart',
      ]);
    } else {
      _quickActions.addAll([
        'login',
        'register',
        'guest_browse',
      ]);
    }
  }

  // Private method to load user-specific dashboard data
  Future<void> _loadUserDashboardData() async {
    if (_userProvider.isLoggedIn) {
      // Load user-specific data like favorites, recent orders, etc.
      await _productProvider.loadFavoritesFromStorage();
    }
  }

  // Private method to refresh user data
  Future<void> _refreshUserData() async {
    if (_userProvider.isLoggedIn) {
      await _userProvider.refreshCurrentUser();
    }
  }

  // Handle navigation to different sections
  void navigateToSection(String section) {
    switch (section) {
      case 'products':
        _navigationService.navigateToNamed('/products');
        break;
      case 'categories':
        _navigationService.navigateToNamed('/categories');
        break;
      case 'favorites':
        _navigationService.navigateToNamed('/favorites');
        break;
      case 'profile':
        _navigationService.navigateToNamed('/profile');
        break;
      default:
        debugPrint('Unknown section: $section');
    }
  }

  // Clear any errors
  void clearError() {
    _userProvider.clearError();
    _productProvider.clearError();
  }

  @override
  void dispose() {
    // Clean up controller resources
    super.dispose();
  }
}
```

## presentation/controllers/profile_controller.dart
**Purpose**: Manages profile screen business logic, handling user profile operations, form validation, and coordinating profile-related data updates.

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import '../../data/providers/user_provider.dart';
import '../../data/models/user_model.dart';
import '../../core/services/navigation_service.dart';
import '../../core/utils/validators.dart';
import '../../shared/enums/app_state.dart';

// Profile controller managing profile screen business logic
// Handles profile editing, validation, and user account operations
class ProfileController extends ChangeNotifier {
  final UserProvider _userProvider;
  final NavigationService _navigationService;

  ProfileController({
    required UserProvider userProvider,
    required NavigationService navigationService,
  })  : _userProvider = userProvider,
        _navigationService = navigationService {
    _initializeFormControllers();
  }

  // Form controllers for profile editing
  late TextEditingController nameController;
  late TextEditingController emailController;
  late TextEditingController phoneController;
  late TextEditingController bioController;

  // Form state
  final GlobalKey<FormState> formKey = GlobalKey<FormState>();
  bool _isEditing = false;
  bool _isSaving = false;
  Map<String, String?> _fieldErrors = {};

  // Public getters for accessing controller state
  bool get isEditing => _isEditing;
  bool get isSaving => _isSaving;
  bool get isLoading => _userProvider.isLoading || _isSaving;
  bool get hasError => _userProvider.hasError;
  String? get errorMessage => _userProvider.errorMessage;
  UserModel? get currentUser => _userProvider.currentUser;
  Map<String, String?> get fieldErrors => Map.unmodifiable(_fieldErrors);

  // Initialize form controllers with current user data
  void _initializeFormControllers() {
    nameController = TextEditingController();
    emailController = TextEditingController();
    phoneController = TextEditingController();
    bioController = TextEditingController();

    // Populate with current user data if available
    _populateFormFields();
  }

  // Populate form fields with current user data
  void _populateFormFields() {
    final user = _userProvider.currentUser;
    if (user != null) {
      nameController.text = user.name;
      emailController.text = user.email;
      phoneController.text = user.phoneNumber ?? '';
      bioController.text = user.bio ?? '';
    }
  }

  // Enable editing mode
  void startEditing() {
    _isEditing = true;
    _fieldErrors.clear();
    notifyListeners();
  }

  // Cancel editing and revert changes
  void cancelEditing() {
    _isEditing = false;
    _fieldErrors.clear();
    _populateFormFields(); // Restore original values
    notifyListeners();
  }

  // Validate individual field
  String? validateField(String field, String value) {
    String? error;
    
    switch (field) {
      case 'name':
        error = Validators.validateName(value);
        break;
      case 'email':
        error = Validators.validateEmail(value);
        break;
      case 'phone':
        error = value.isNotEmpty ? Validators.validatePhoneNumber(value) : null;
        break;
      case 'bio':
        error = value.length > 500 ? 'Bio must be less than 500 characters' : null;
        break;
    }

    // Update field errors
    if (error != null) {
      _fieldErrors[field] = error;
    } else {
      _fieldErrors.remove(field);
    }
    
    notifyListeners();
    return error;
  }

  // Validate entire form
  bool validateForm() {
    if (!formKey.currentState!.validate()) {
      return false;
    }

    // Additional custom validation
    _fieldErrors.clear();
    
    validateField('name', nameController.text);
    validateField('email', emailController.text);
    validateField('phone', phoneController.text);
    validateField('bio', bioController.text);

    return _fieldErrors.isEmpty;
  }

  // Save profile changes
  Future<bool> saveProfile() async {
    if (!validateForm()) {
      return false;
    }

    _isSaving = true;
    notifyListeners();

    try {
      final currentUser = _userProvider.currentUser;
      if (currentUser == null) {
        throw Exception('No user logged in');
      }

      // Create updated user model
      final updatedUser = UserModel(
        id: currentUser.id,
        name: nameController.text.trim(),
        email: emailController.text.trim(),
        phoneNumber: phoneController.text.trim().isEmpty 
            ? null 
            : phoneController.text.trim(),
        bio: bioController.text.trim().isEmpty 
            ? null 
            : bioController.text.trim(),
        profileImageUrl: currentUser.profileImageUrl,
        createdAt: currentUser.createdAt,
        isActive: currentUser.isActive,
      );

      // Update profile through provider
      await _userProvider.updateUserProfile(updatedUser);

      if (!_userProvider.hasError) {
        _isEditing = false;
        _showSuccessMessage('Profile updated successfully');
        return true;
      } else {
        return false;
      }
    } catch (e) {
      _showErrorMessage('Failed to update profile: ${e.toString()}');
      return false;
    } finally {
      _isSaving = false;
      notifyListeners();
    }
  }

  // Handle profile picture update
  Future<void> updateProfilePicture() async {
    try {
      // In a real app, you would use image_picker to select image
      // and upload to server. For now, we'll use a mock implementation
      _showInfoMessage('Profile picture update feature coming soon');
    } catch (e) {
      _showErrorMessage('Failed to update profile picture: ${e.toString()}');
    }
  }

  // Delete user account
  Future<void> deleteAccount() async {
    final confirmed = await _showDeleteConfirmation();
    if (!confirmed) return;

    try {
      final currentUser = _userProvider.currentUser;
      if (currentUser == null) return;

      final success = await _userProvider.deleteUser(currentUser.id);
      
      if (success) {
        _showSuccessMessage('Account deleted successfully');
        // Navigate to login screen after account deletion
        _navigationService.navigateToNamedAndClearStack('/login');
      }
    } catch (e) {
      _showErrorMessage('Failed to delete account: ${e.toString()}');
    }
  }

  // Logout user
  Future<void> logout() async {
    try {
      _userProvider.logoutUser();
      _showInfoMessage('Logged out successfully');
      _navigationService.navigateToNamedAndClearStack('/login');
    } catch (e) {
      _showErrorMessage('Logout failed: ${e.toString()}');
    }
  }

  // Change password (navigate to change password screen)
  void changePassword() {
    _navigationService.navigateToNamed('/change-password');
  }

  // View account settings
  void viewAccountSettings() {
    _navigationService.navigateToNamed('/account-settings');
  }

  // View privacy settings
  void viewPrivacySettings() {
    _navigationService.navigateToNamed('/privacy-settings');
  }

  // Get profile completion percentage
  double getProfileCompletionPercentage() {
    final user = _userProvider.currentUser;
    if (user == null) return 0.0;

    int completedFields = 0;
    const totalFields = 5; // name, email, phone, bio, profileImage

    if (user.name.isNotEmpty) completedFields++;
    if (user.email.isNotEmpty) completedFields++;
    if (user.phoneNumber != null && user.phoneNumber!.isNotEmpty) completedFields++;
    if (user.bio != null && user.bio!.isNotEmpty) completedFields++;
    if (user.profileImageUrl != null && user.profileImageUrl!.isNotEmpty) completedFields++;

    return completedFields / totalFields;
  }

  // Get profile completion suggestions
  List<String> getProfileCompletionSuggestions() {
    final user = _userProvider.currentUser;
    if (user == null) return [];

    List<String> suggestions = [];

    if (user.phoneNumber == null || user.phoneNumber!.isEmpty) {
      suggestions.add('Add your phone number');
    }
    if (user.bio == null || user.bio!.isEmpty) {
      suggestions.add('Write a short bio about yourself');
    }
    if (user.profileImageUrl == null || user.profileImageUrl!.isEmpty) {
      suggestions.add('Upload a profile picture');
    }

    return suggestions;
  }

  // Private method to show success message
  void _showSuccessMessage(String message) {
    // In a real app, you would use a SnackBar or Toast
    debugPrint('Success: $message');
  }

  // Private method to show error message
  void _showErrorMessage(String message) {
    // In a real app, you would use a SnackBar or Dialog
    debugPrint('Error: $message');
  }

  // Private method to show info message
  void _showInfoMessage(String message) {
    // In a real app, you would use a SnackBar
    debugPrint('Info: $message');
  }

  // Private method to show delete confirmation dialog
  Future<bool> _showDeleteConfirmation() async {
    // In a real app, you would show a proper confirmation dialog
    // For now, return true as a mock confirmation
    return true;
  }

  // Clear errors
  void clearError() {
    _userProvider.clearError();
    _fieldErrors.clear();
    notifyListeners();
  }

  // Refresh profile data
  Future<void> refreshProfile() async {
    await _userProvider.refreshCurrentUser();
    _populateFormFields();
  }

  @override
  void dispose() {
    // Dispose form controllers
    nameController.dispose();
    emailController.dispose();
    phoneController.dispose();
    bioController.dispose();
    super.dispose();
  }
}
```


## presentation/controllers/product_controller.dart
**Purpose**: Manages product-related business logic and coordinates between product views and data layer.
**Benefits**: Separates UI logic from business logic, making product management more maintainable and testable.

```dart
import 'package:flutter/foundation.dart';
import '../../data/models/product_model.dart';
import '../../data/providers/product_provider.dart';
import '../../shared/enums/app_state.dart';

/// Controller for managing product-related operations
/// Handles business logic for product listing, filtering, and selection
class ProductController extends ChangeNotifier {
  // Reference to the product provider for data operations
  final ProductProvider _productProvider;
  
  // Current state of the product controller
  AppState _state = AppState.initial;
  
  // Currently selected product
  Product? _selectedProduct;
  
  // Search query for filtering products
  String _searchQuery = '';
  
  // List of filtered products based on search
  List<Product> _filteredProducts = [];

  // Constructor - injects the product provider dependency
  ProductController(this._productProvider) {
    // Listen to changes in the product provider
    _productProvider.addListener(_onProductProviderChanged);
    // Initialize with current products
    _filteredProducts = _productProvider.products;
  }

  // Getters for accessing private fields
  AppState get state => _state;
  Product? get selectedProduct => _selectedProduct;
  String get searchQuery => _searchQuery;
  List<Product> get filteredProducts => _filteredProducts;
  bool get isLoading => _state == AppState.loading;
  bool get hasError => _state == AppState.error;
  
  /// Loads all products from the data source
  Future<void> loadProducts() async {
    try {
      // Set loading state
      _setState(AppState.loading);
      
      // Load products through provider
      await _productProvider.loadProducts();
      
      // Update filtered products
      _applyFilter();
      
      // Set success state
      _setState(AppState.success);
    } catch (e) {
      // Handle errors and set error state
      _setState(AppState.error);
      if (kDebugMode) {
        print('Error loading products: $e');
      }
    }
  }
  
  /// Selects a specific product
  void selectProduct(Product product) {
    _selectedProduct = product;
    notifyListeners(); // Notify UI about the change
  }
  
  /// Clears the currently selected product
  void clearSelection() {
    _selectedProduct = null;
    notifyListeners();
  }
  
  /// Updates the search query and filters products
  void updateSearchQuery(String query) {
    _searchQuery = query;
    _applyFilter(); // Apply filter with new query
    notifyListeners();
  }
  
  /// Clears the search query and shows all products
  void clearSearch() {
    _searchQuery = '';
    _applyFilter();
    notifyListeners();
  }
  
  /// Refreshes the product list
  Future<void> refreshProducts() async {
    await loadProducts();
  }
  
  /// Filters products based on search query
  void _applyFilter() {
    if (_searchQuery.isEmpty) {
      // Show all products if no search query
      _filteredProducts = List.from(_productProvider.products);
    } else {
      // Filter products based on name or description
      _filteredProducts = _productProvider.products
          .where((product) =>
              product.name.toLowerCase().contains(_searchQuery.toLowerCase()) ||
              product.description.toLowerCase().contains(_searchQuery.toLowerCase()))
          .toList();
    }
  }
  
  /// Handles changes in the product provider
  void _onProductProviderChanged() {
    _applyFilter(); // Re-apply filter when provider data changes
    notifyListeners();
  }
  
  /// Updates the internal state
  void _setState(AppState newState) {
    _state = newState;
    notifyListeners();
  }
  
  /// Cleanup method to prevent memory leaks
  @override
  void dispose() {
    _productProvider.removeListener(_onProductProviderChanged);
    super.dispose();
  }
}
```

## presentation/views/widgets/common/loading_widget.dart
**Purpose**: Consistent loading indicator widget used throughout the application.
**Benefits**: Provides uniform loading experience and can be easily customized for different contexts.

```dart
import 'package:flutter/material.dart';

/// Custom loading widget with consistent styling
/// Provides different loading states and customization options
class LoadingWidget extends StatelessWidget {
  // Loading message to display
  final String? message;
  
  // Size of the loading indicator
  final double size;
  
  // Color of the loading indicator
  final Color? color;
  
  // Whether to show the message below the indicator
  final bool showMessage;
  
  // Custom widget to show instead of default indicator
  final Widget? customIndicator;

  const LoadingWidget({
    Key? key,
    this.message,
    this.size = 32.0,
    this.color,
    this.showMessage = true,
    this.customIndicator,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final colorScheme = Theme.of(context).colorScheme;
    
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        mainAxisSize: MainAxisSize.min,
        children: [
          // Loading indicator
          customIndicator ?? SizedBox(
            width: size,
            height: size,
            child: CircularProgressIndicator(
              strokeWidth: 3,
              valueColor: AlwaysStoppedAnimation<Color>(
                color ?? colorScheme.primary,
              ),
            ),
          ),
          
          // Loading message
          if (showMessage && message != null) ...[
            const SizedBox(height: 16),
            Text(
              message!,
              style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                color: colorScheme.onSurface.withOpacity(0.7),
              ),
              textAlign: TextAlign.center,
            ),
          ],
        ],
      ),
    );
  }
}

/// Specialized loading widget for overlay situations
class OverlayLoadingWidget extends StatelessWidget {
  final String? message;
  final Color? backgroundColor;

  const OverlayLoadingWidget({
    Key? key,
    this.message,
    this.backgroundColor,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: backgroundColor ?? Colors.black.withOpacity(0.3),
      child: LoadingWidget(
        message: message,
        color: Colors.white,
      ),
    );
  }
}
```

## presentation/views/widgets/common/error_widget.dart
**Purpose**: Consistent error display widget with retry functionality.
**Benefits**: Provides uniform error handling UI and allows users to recover from errors gracefully.

```dart
import 'package:flutter/material.dart';

/// Custom error widget for displaying error states
/// Provides consistent error UI with retry functionality
class CustomErrorWidget extends StatelessWidget {
  // Error message to display
  final String message;
  
  // Callback function for retry action
  final VoidCallback? onRetry;
  
  // Icon to display with error
  final IconData? icon;
  
  // Custom error title
  final String? title;
  
  // Whether to show retry button
  final bool showRetryButton;
  
  // Custom retry button text
  final String retryButtonText;

  const CustomErrorWidget({
    Key? key,
    required this.message,
    this.onRetry,
    this.icon,
    this.title,
    this.showRetryButton = true,
    this.retryButtonText = 'Retry',
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final colorScheme = Theme.of(context).colorScheme;
    
    return Center(
      child: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          mainAxisSize: MainAxisSize.min,
          children: [
            // Error icon
            Icon(
              icon ?? Icons.error_outline,
              size: 64,
              color: colorScheme.error,
            ),
            
            const SizedBox(height: 16),
            
            // Error title
            if (title != null) ...[
              Text(
                title!,
                style: Theme.of(context).textTheme.headlineSmall?.copyWith(
                  color: colorScheme.error,
                ),
                textAlign: TextAlign.center,
              ),
              const SizedBox(height: 8),
            ],
            
            // Error message
            Text(
              message,
              style: Theme.of(context).textTheme.bodyLarge?.copyWith(
                color: colorScheme.onSurface.withOpacity(0.7),
              ),
              textAlign: TextAlign.center,
            ),
            
            // Retry button
            if (showRetryButton && onRetry != null) ...[
              const SizedBox(height: 24),
              ElevatedButton.icon(
                onPressed: onRetry,
                icon: const Icon(Icons.refresh),
                label: Text(retryButtonText),
                style: ElevatedButton.styleFrom(
                  backgroundColor: colorScheme.primary,
                  foregroundColor: colorScheme.onPrimary,
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }
}

/// Specialized error widget for network errors
class NetworkErrorWidget extends StatelessWidget {
  final VoidCallback? onRetry;

  const NetworkErrorWidget({
    Key? key,
    this.onRetry,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomErrorWidget(
      title: 'Connection Error',
      message: 'Please check your internet connection and try again.',
      icon: Icons.wifi_off,
      onRetry: onRetry,
    );
  }
}

/// Specialized error widget for data not found
class NotFoundErrorWidget extends StatelessWidget {
  final String? customMessage;
  final VoidCallback? onRetry;

  const NotFoundErrorWidget({
    Key? key,
    this.customMessage,
    this.onRetry,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomErrorWidget(
      title: 'Not Found',
      message: customMessage ?? 'The requested data could not be found.',
      icon: Icons.search_off,
      onRetry: onRetry,
      showRetryButton: onRetry != null,
    );
  }
}
```

## presentation/views/widgets/specific/user_card.dart
**Purpose**: Displays user information in a card format.
**Benefits**: Provides consistent user data presentation and demonstrates model-view integration.

```dart
import 'package:flutter/material.dart';
import '../../../../data/models/user_model.dart';
import '../../../../shared/enums/user_role.dart';

/// User card widget for displaying user information
/// Demonstrates model-view integration and consistent UI patterns
class UserCard extends StatelessWidget {
  // User data to display
  final User user;
  
  // Whether the card is selectable
  final bool isSelectable;
  
  // Whether the card is currently selected
  final bool isSelected;
  
  //

## presentation/views/screens/home_screen.dart
**Purpose**: Main landing screen that displays user information and navigation options.
**Benefits**: Provides a centralized entry point and demonstrates StatefulWidget usage with controller integration.

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../../../data/providers/user_provider.dart';
import '../../../data/providers/theme_provider.dart';
import '../../controllers/home_controller.dart';
import '../widgets/common/loading_widget.dart';
import '../widgets/common/error_widget.dart';
import '../widgets/specific/user_card.dart';
import '../../../app/routes/app_routes.dart';

/// Home screen - main landing page of the application
/// Demonstrates StatefulWidget usage and controller integration
class HomeScreen extends StatefulWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  // Controller for managing home screen logic
  late HomeController _homeController;

  @override
  void initState() {
    super.initState();
    // Initialize controller with required dependencies
    _homeController = HomeController(
      context.read<UserProvider>(),
    );
    // Load initial data
    _homeController.loadUserData();
  }

  @override
  void dispose() {
    // Clean up controller to prevent memory leaks
    _homeController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // App bar with theme toggle and title
      appBar: AppBar(
        title: const Text('Home'),
        elevation: 0,
        actions: [
          // Theme toggle button
          Consumer<ThemeProvider>(
            builder: (context, themeProvider, child) {
              return IconButton(
                icon: Icon(
                  themeProvider.isDarkMode ? Icons.light_mode : Icons.dark_mode,
                ),
                onPressed: () => themeProvider.toggleTheme(),
                tooltip: 'Toggle Theme',
              );
            },
          ),
        ],
      ),
      
      // Main body content
      body: ChangeNotifierProvider.value(
        value: _homeController,
        child: Consumer<HomeController>(
          builder: (context, controller, child) {
            // Show loading indicator while data is being fetched
            if (controller.isLoading) {
              return const LoadingWidget(message: 'Loading user data...');
            }
            
            // Show error message if data loading failed
            if (controller.hasError) {
              return CustomErrorWidget(
                message: 'Failed to load user data',
                onRetry: () => controller.loadUserData(),
              );
            }
            
            // Main content when data is loaded successfully
            return RefreshIndicator(
              onRefresh: () => controller.refreshData(),
              child: SingleChildScrollView(
                physics: const AlwaysScrollableScrollPhysics(),
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    // Welcome message
                    _buildWelcomeSection(controller),
                    
                    const SizedBox(height: 24),
                    
                    // User information card
                    _buildUserSection(controller),
                    
                    const SizedBox(height: 24),
                    
                    // Navigation options
                    _buildNavigationSection(),
                    
                    const SizedBox(height: 24),
                    
                    // Quick stats section
                    _buildQuickStatsSection(controller),
                  ],
                ),
              ),
            );
          },
        ),
      ),
    );
  }
  
  /// Builds the welcome section with greeting
  Widget _buildWelcomeSection(HomeController controller) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Welcome Back!',
              style: Theme.of(context).textTheme.headlineSmall,
            ),
            const SizedBox(height: 8),
            Text(
              controller.getGreetingMessage(),
              style: Theme.of(context).textTheme.bodyLarge,
            ),
          ],
        ),
      ),
    );
  }
  
  /// Builds the user information section
  Widget _buildUserSection(HomeController controller) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'User Information',
          style: Theme.of(context).textTheme.titleLarge,
        ),
        const SizedBox(height: 12),
        Consumer<UserProvider>(
          builder: (context, userProvider, child) {
            final user = userProvider.currentUser;
            if (user != null) {
              return UserCard(user: user);
            }
            return const Text('No user data available');
          },
        ),
      ],
    );
  }
  
  /// Builds the navigation section with quick access buttons
  Widget _buildNavigationSection() {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'Quick Access',
          style: Theme.of(context).textTheme.titleLarge,
        ),
        const SizedBox(height: 12),
        Row(
          children: [
            // Navigate to profile button
            Expanded(
              child: ElevatedButton.icon(
                onPressed: () => Navigator.pushNamed(
                  context,
                  AppRoutes.profile,
                ),
                icon: const Icon(Icons.person),
                label: const Text('Profile'),
              ),
            ),
            const SizedBox(width: 12),
            // Navigate to products button
            Expanded(
              child: ElevatedButton.icon(
                onPressed: () => Navigator.pushNamed(
                  context,
                  AppRoutes.products,
                ),
                icon: const Icon(Icons.shopping_bag),
                label: const Text('Products'),
              ),
            ),
          ],
        ),
      ],
    );
  }
  
  /// Builds the quick stats section
  Widget _buildQuickStatsSection(HomeController controller) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              'Quick Stats',
              style: Theme.of(context).textTheme.titleMedium,
            ),
            const SizedBox(height: 12),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: [
                _buildStatItem('Login Count', controller.loginCount.toString()),
                _buildStatItem('Last Login', controller.getLastLoginTime()),
                _buildStatItem('App Version', '1.0.0'),
              ],
            ),
          ],
        ),
      ),
    );
  }
  
  /// Builds individual stat item
  Widget _buildStatItem(String label, String value) {
    return Column(
      children: [
        Text(
          value,
          style: Theme.of(context).textTheme.headlineSmall?.copyWith(
            fontWeight: FontWeight.bold,
          ),
        ),
        const SizedBox(height: 4),
        Text(
          label,
          style: Theme.of(context).textTheme.bodySmall,
        ),
      ],
    );
  }
}
```

## presentation/views/screens/profile_screen.dart
**Purpose**: Displays and manages user profile information with edit capabilities.
**Benefits**: Shows form handling, validation, and controller pattern for user data management.

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../../../data/providers/user_provider.dart';
import '../../controllers/profile_controller.dart';
import '../widgets/common/custom_button.dart';
import '../widgets/common/custom_text_field.dart';
import '../widgets/common/loading_widget.dart';
import '../widgets/common/error_widget.dart';
import '../../../core/utils/validators.dart';

/// Profile screen for displaying and editing user information
/// Demonstrates form handling and validation in MVC pattern
class ProfileScreen extends StatefulWidget {
  const ProfileScreen({Key? key}) : super(key: key);

  @override
  State<ProfileScreen> createState() => _ProfileScreenState();
}

class _ProfileScreenState extends State<ProfileScreen> {
  // Controller for managing profile operations
  late ProfileController _profileController;
  
  // Form key for validation
  final _formKey = GlobalKey<FormState>();
  
  // Text controllers for form fields
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _phoneController = TextEditingController();

  @override
  void initState() {
    super.initState();
    // Initialize controller with user provider
    _profileController = ProfileController(
      context.read<UserProvider>(),
    );
    // Load user profile data
    _profileController.loadProfile();
    // Set up listeners for profile changes
    _profileController.addListener(_onProfileChanged);
  }

  @override
  void dispose() {
    // Clean up controllers and listeners
    _profileController.removeListener(_onProfileChanged);
    _profileController.dispose();
    _nameController.dispose();
    _emailController.dispose();
    _phoneController.dispose();
    super.dispose();
  }

  /// Handles profile data changes
  void _onProfileChanged() {
    final user = _profileController.currentUser;
    if (user != null) {
      // Update text controllers with user data
      _nameController.text = user.name;
      _emailController.text = user.email;
      _phoneController.text = user.phone ?? '';
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Profile'),
        elevation: 0,
        actions: [
          // Save button in app bar
          ChangeNotifierProvider.value(
            value: _profileController,
            child: Consumer<ProfileController>(
              builder: (context, controller, child) {
                return TextButton(
                  onPressed: controller.hasChanges && !controller.isLoading
                      ? _saveProfile
                      : null,
                  child: const Text('Save'),
                );
              },
            ),
          ),
        ],
      ),
      
      body: ChangeNotifierProvider.value(
        value: _profileController,
        child: Consumer<ProfileController>(
          builder: (context, controller, child) {
            // Show loading indicator while data is being fetched
            if (controller.isLoading && controller.currentUser == null) {
              return const LoadingWidget(message: 'Loading profile...');
            }
            
            // Show error message if profile loading failed
            if (controller.hasError) {
              return CustomErrorWidget(
                message: 'Failed to load profile',
                onRetry: () => controller.loadProfile(),
              );
            }
            
            // Main profile form
            return SingleChildScrollView(
              padding: const EdgeInsets.all(16.0),
              child: Form(
                key: _formKey,
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    // Profile header with avatar
                    _buildProfileHeader(controller),
                    
                    const SizedBox(height: 32),
                    
                    // Profile form fields
                    _buildProfileForm(controller),
                    
                    const SizedBox(height: 32),
                    
                    // Action buttons
                    _buildActionButtons(controller),
                  ],
                ),
              ),
            );
          },
        ),
      ),
    );
  }
  
  /// Builds the profile header with avatar and basic info
  Widget _buildProfileHeader(ProfileController controller) {
    final user = controller.currentUser;
    
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Row(
          children: [
            // User avatar
            CircleAvatar(
              radius: 40,
              backgroundColor: Theme.of(context).primaryColor,
              child: Text(
                user?.name.substring(0, 1).toUpperCase() ?? 'U',
                style: const TextStyle(
                  fontSize: 24,
                  fontWeight: FontWeight.bold,
                  color: Colors.white,
                ),
              ),
            ),
            const SizedBox(width: 16),
            
            // User basic info
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    user?.name ?? 'Unknown User',
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 4),
                  Text(
                    user?.email ?? 'No email',
                    style: Theme.of(context).textTheme.bodyMedium,
                  ),
                  const SizedBox(height: 4),
                  Text(
                    'ID: ${user?.id ?? 'Unknown'}',
                    style: Theme.of(context).textTheme.bodySmall,
                  ),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
  
  /// Builds the profile form with input fields
  Widget _buildProfileForm(ProfileController controller) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'Profile Information',
          style: Theme.of(context).textTheme.titleLarge,
        ),
        const SizedBox(height: 16),
        
        // Name field
        CustomTextField(
          controller: _nameController,
          label: 'Full Name',
          hint: 'Enter your full name',
          prefixIcon: Icons.person,
          validator: Validators.validateName,
          onChanged: (_) => controller.markAsChanged(),
        ),
        
        const SizedBox(height: 16),
        
        // Email field
        CustomTextField(
          controller: _emailController,
          label: 'Email Address',
          hint: 'Enter your email address',
          prefixIcon: Icons.email,
          keyboardType: TextInputType.emailAddress,
          validator: Validators.validateEmail,
          onChanged: (_) => controller.markAsChanged(),
        ),
        
        const SizedBox(height: 16),
        
        // Phone field
        CustomTextField(
          controller: _phoneController,
          label: 'Phone Number',
          hint: 'Enter your phone number',
          prefixIcon: Icons.phone,
          keyboardType: TextInputType.phone,
          validator: Validators.validatePhone,
          onChanged: (_) => controller.markAsChanged(),
        ),
      ],
    );
  }
  
  /// Builds action buttons for save and reset
  Widget _buildActionButtons(ProfileController controller) {
    return Column(
      children: [
        // Save button
        CustomButton(
          text: 'Save Changes',
          onPressed: controller.hasChanges && !controller.isLoading
              ? _saveProfile
              : null,
          isLoading: controller.isLoading,
          width: double.infinity,
        ),
        
        const SizedBox(height: 12),
        
        // Reset button
        CustomButton(
          text: 'Reset Changes',
          onPressed: controller.hasChanges && !controller.isLoading
              ? _resetForm
              : null,
          variant: ButtonVariant.outlined,
          width: double.infinity,
        ),
      ],
    );
  }
  
  /// Saves the profile data
  Future<void> _saveProfile() async {
    if (_formKey.currentState!.validate()) {
      final success = await _profileController.updateProfile(
        name: _nameController.text.trim(),
        email: _emailController.text.trim(),
        phone: _phoneController.text.trim(),
      );
      
      if (success && mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('Profile updated successfully'),
            backgroundColor: Colors.green,
          ),
        );
      } else if (mounted) {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(
            content: Text('Failed to update profile'),
            backgroundColor: Colors.red,
          ),
        );
      }
    }
  }
  
  /// Resets the form to original values
  void _resetForm() {
    _profileController.resetChanges();
    _onProfileChanged(); // Refresh form fields
  }
}
```

## presentation/views/screens/product_screen.dart
**Purpose**: Displays product listings with search, filter, and selection capabilities.
**Benefits**: Demonstrates list management, search functionality, and complex UI interactions in MVC pattern.

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../../../data/providers/product_provider.dart';
import '../../controllers/product_controller.dart';
import '../widgets/common/loading_widget.dart';
import '../widgets/common/error_widget.dart';
import '../widgets/specific/product_card.dart';

/// Product screen for displaying and managing product listings
/// Demonstrates list management and search functionality
class ProductScreen extends StatefulWidget {
  const ProductScreen({Key? key}) : super(key: key);

  @override
  State<ProductScreen> createState() => _ProductScreenState();
}

class _ProductScreenState extends State<ProductScreen> {
  // Controller for managing product operations
  late ProductController _productController;
  
  // Search controller for the search field
  final _searchController = TextEditingController();
  
  // Focus node for search field
  final _searchFocusNode = FocusNode();

  @override
  void initState() {
    super.initState();
    // Initialize controller with product provider
    _productController = ProductController(
      context.read<ProductProvider>(),
    );
    // Load products on screen initialization
    _productController.loadProducts();
  }

  @override
  void dispose() {
    // Clean up resources
    _productController.dispose();
    _searchController.dispose();
    _searchFocusNode.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Products'),
        elevation: 0,
        bottom: PreferredSize(
          preferredSize: const Size.fromHeight(60),
          child: _buildSearchBar(),
        ),
      ),
      
      body: ChangeNotifierProvider.value(
        value: _productController,
        child: Consumer<ProductController>(
          builder: (context, controller, child) {
            // Show loading indicator while products are being fetched
            if (controller.isLoading) {
              return const LoadingWidget(message: 'Loading products...');
            }
            
            // Show error message if product loading failed
            if (controller.hasError) {
              return CustomErrorWidget(
                message: 'Failed to load products',
                onRetry: () => controller.loadProducts(),
              );
            }
            
            // Show empty state if no products available
            if (controller.filteredProducts.isEmpty) {
              return _buildEmptyState(controller);
            }
            
            // Main product list
            return RefreshIndicator(
              onRefresh: () => controller.refreshProducts(),
              child: Column(
                children: [
                  // Search results info
                  _buildSearchInfo(controller),
                  
                  // Product list
                  Expanded(
                    child: _buildProductList(controller),
                  ),
                ],
              ),
            );
          },
        ),
      ),
      
      // Floating action button for selected product info
      floatingActionButton: ChangeNotifierProvider.value(
        value: _productController,
        child: Consumer<ProductController>(
          builder: (context, controller, child) {
            if (controller.selectedProduct != null) {
              return FloatingActionButton.extended(
                onPressed: () => _showProductDetails(controller.selectedProduct!),
                icon: const Icon(Icons.info),
                label: const Text('View Details'),
              );
            }
            return const SizedBox.shrink();
          },
        ),
      ),
    );
  }
  
  /// Builds the search bar widget
  Widget _buildSearchBar() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: TextField(
        controller: _searchController,
        focusNode: _searchFocusNode,
        decoration: InputDecoration(
          hintText: 'Search products...',
          prefixIcon: const Icon(Icons.search),
          suffixIcon: _searchController.text.isNotEmpty
              ? IconButton(
                  icon: const Icon(Icons.clear),
                  onPressed: () {
                    _searchController.clear();
                    _productController.clearSearch();
                    _searchFocusNode.unfocus();
                  },
                )
              : null,
          border: OutlineInputBorder(
            borderRadius: BorderRadius.circular(12),
          ),
          filled: true,
          fillColor: Theme.of(context).cardColor,
        ),
        onChanged: (value) {
          _productController.updateSearchQuery(value);
        },
        onSubmitted: (value) {
          _searchFocusNode.unfocus();
        },
      ),
    );
  }
  
  /// Builds search results information
  Widget _buildSearchInfo(ProductController controller) {
    if (controller.searchQuery.isEmpty) {
      return const SizedBox.shrink();
    }
    
    return Container(
      width: double.infinity,
      padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
      color: Theme.of(context).primaryColor.withOpacity(0.1),
      child: Text(
        'Found ${controller.filteredProducts.length} products for "${controller.searchQuery}"',
        style: Theme.of(context).textTheme.bodyMedium,
      ),
    );
  }
  
  /// Builds the product list
  Widget _buildProductList(ProductController controller) {
    return ListView.builder(
      padding: const EdgeInsets.all(16),
      itemCount: controller.filteredProducts.length,
      itemBuilder: (context, index) {
        final product = controller.filteredProducts[index];
        final isSelected = controller.selectedProduct?.id == product.id;
        
        return Padding(
          padding: const EdgeInsets.only(bottom: 12),
          child: ProductCard(
            product: product,
            isSelected: isSelected,
            onTap: () {
              if (isSelected) {
                controller.clearSelection();
              } else {
                controller.selectProduct(product);
              }
            },
            onLongPress: () => _showProductDetails(product),
          ),
        );
      },
    );
  }
  
  /// Builds empty state when no products are available
  Widget _buildEmptyState(ProductController controller) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(
            controller.searchQuery.isEmpty ? Icons.inventory_2 : Icons.search_off,
            size: 80,
            color: Colors.grey[400],
          ),
          const SizedBox(height: 16),
          Text(
            controller.searchQuery.isEmpty
                ? 'No products available'
                : 'No products found for "${controller.searchQuery}"',
            style: Theme.of(context).textTheme.headlineSmall,
            textAlign: TextAlign.center,
          ),
          const SizedBox(height: 8),
          Text(
            controller.searchQuery.isEmpty
                ? 'Products will appear here once loaded'
                : 'Try adjusting your search terms',
            style: Theme.of(context).textTheme.bodyMedium,
            textAlign: TextAlign.center,
          ),
          if (controller.searchQuery.isNotEmpty) ...[
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                _searchController.clear();
                controller.clearSearch();
              },
              child: const Text('Clear Search'),
            ),
          ],
        ],
      ),
    );
  }
  
  /// Shows detailed product information in a bottom sheet
  void _showProductDetails(product) {
    showModalBottomSheet(
      context: context,
      isScrollControlled: true,
      shape: const RoundedRectangleBorder(
        borderRadius: BorderRadius.vertical(top: Radius.circular(20)),
      ),
      builder: (context) => DraggableScrollableSheet(
        initialChildSize: 0.6,
        maxChildSize: 0.9,
        minChildSize: 0.3,
        expand: false,
        builder: (context, scrollController) {
          return SingleChildScrollView(
            controller: scrollController,
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  // Handle bar
                  Center(
                    child: Container(
                      width: 40,
                      height: 4,
                      decoration: BoxDecoration(
                        color: Colors.grey[300],
                        borderRadius: BorderRadius.circular(2),
                      ),
                    ),
                  ),
                  const SizedBox(height: 16),
                  
                  // Product name
                  Text(
                    product.name,
                    style: Theme.of(context).textTheme.headlineSmall,
                  ),
                  const SizedBox(height: 8),
                  
                  // Product price
                  Text(
                    '\$${product.price}',
                    style: Theme.of(context).textTheme.titleLarge?.copyWith(
                      color: Theme.of(context).primaryColor,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 16),
                  
                  // Product description
                  Text(
                    'Description',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  const SizedBox(height: 8),
                  Text(
                    product.description,
                    style: Theme.of(context).textTheme.bodyMedium,
                  ),
                  const SizedBox(height: 16),
                  
                  // Product details
                  Text(
                    'Details',
                    style: Theme.of(context).textTheme.titleMedium,
                  ),
                  const SizedBox(height: 8),
                  _buildDetailRow('Product ID', product.id.toString()),
                  _buildDetailRow('Category', product.category),
                  _buildDetailRow('In Stock', product.inStock ? 'Yes' : 'No'),
                  
                  const SizedBox(height: 24),
                  
                  // Action buttons
                  Row(
                    children: [
                      Expanded(
                        child: ElevatedButton(
                          onPressed: () {
                            Navigator.pop(context);
                            // Add to cart logic here
                            ScaffoldMessenger.of(context).showSnackBar(
                              SnackBar(
                                content: Text('${product.name} added to cart'),
                                backgroundColor: Colors.green,
                              ),
                            );
                          },
                          child: const Text('Add to Cart'),
                        ),
                      ),
                      const SizedBox(width: 12),
                      Expanded(
                        child: OutlinedButton(
                          onPressed: () => Navigator.pop(context),
                          child: const Text('Close'),
                        ),
                      ),
                    ],
                  ),
                ],
              ),
            ),
          );
        },
      ),
    );
  }
  
  /// Builds a detail row for product information
  Widget _buildDetailRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(
            label,
            style: Theme.of(context).textTheme.bodyMedium?.copyWith(
              fontWeight: FontWeight.w500,
            ),
          ),
          Text(
            value,
            style: Theme.of(context).textTheme.bodyMedium,
          ),
        ],
      ),
    );
  }
}
```

## presentation/views/screens/splash_screen.dart
**Purpose**: Initial loading screen that handles app initialization and navigation.
**Benefits**: Provides smooth app startup experience and demonstrates StatelessWidget with async operations.

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../../../data/providers/user_provider.dart';
import '../../../app/routes/app_routes.dart';
import '../../../core/constants/app_constants.dart';

/// Splash screen displayed during app initialization
/// Handles initial data loading and navigation setup
class SplashScreen extends StatefulWidget {
  const SplashScreen({Key? key}) : super(key: key);

  @override
  State<SplashScreen> createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen>
    with SingleTickerProviderStateMixin {
  // Animation controller for splash animations
  late AnimationController _animationController;
  late Animation<double> _fadeAnimation;
  late Animation<double> _scaleAnimation;

  @override
  void initState() {
    super.initState();
    // Initialize animations
    _setupAnimations();
    // Start app initialization process
    _initializeApp();
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }

  /// Sets up splash screen animations
  void _setupAnimations() {
    _animationController = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    // Fade in animation for logo
    _fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: const Interval(0.0, 0.6, curve: Curves.easeIn),
    ));

    // Scale animation for logo
    _scaleAnimation = Tween<double>(
      begin: 0.8,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: const Interval(0.2, 0.8, curve: Curves.elasticOut),
    ));

    // Start animations
    _animationController.forward();
  }

  /// Initializes the app with necessary data and services
  Future<void> _initializeApp() async {
    try {
      // Simulate app initialization delay
      await Future.delayed(const Duration(seconds: 1));

      // Initialize user data
      if (mounted) {
        await context.read<UserProvider>().initializeUser();
      }

      // Additional initialization tasks can be added here
      // - Check for app updates
      // - Load cached data
      // - Initialize analytics
      // - Setup push notifications

      // Ensure minimum splash duration for better UX
      await Future.delayed(const Duration(milliseconds: 500));

      // Navigate to home screen after initialization
      if (mounted) {
        _navigateToHome();
      }
    } catch (e) {
      // Handle initialization errors
      if (mounted) {
        _showErrorAndRetry(e.toString());
      }
    }
  }

  /// Navigates to the home screen
  void _navigateToHome() {
    Navigator.pushReplacementNamed(context, AppRoutes.home);
  }

  /// Shows error dialog and provides retry option
  void _showErrorAndRetry(String error) {
    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (context) => AlertDialog(
        title: const Text('Initialization Failed'),
        content: Text('Failed to initialize app: $error'),
        actions: [
          TextButton(
            onPressed: () {
              Navigator.pop(context);
              _initializeApp(); // Retry initialization
            },
            child: const Text('Retry'),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Theme.of(context).primaryColor,
      body: AnimatedBuilder(
        animation: _animationController,
        builder: (context, child) {
          return Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                // App logo with animations
                Transform.scale(
                  scale: _scaleAnimation.value,
                  child: FadeTransition(
                    opacity: _fadeAnimation,
                    child: Container(
                      width: 120,
                      height: 120,
                      decoration: BoxDecoration(
                        color: Colors.white,
                        borderRadius: BorderRadius.circular(20),
                        boxShadow: [
                          BoxShadow(
                            color: Colors.black.withOpacity(0.2),
                            blurRadius: 10,
                            offset: const Offset(0, 4),
                          ),
                        ],
                      ),
                      child: const Icon(
                        Icons.flutter_dash,
                        size: 60,
                        color: Colors.blue,
                      ),
                    ),
                  ),
                ),

                const SizedBox(height: 24),

                // App name
                FadeTransition(
                  opacity: _fadeAnimation,
                  child: Text(
                    AppConstants.appName,
                    style: const TextStyle(
                      color: Colors.white,
                      fontSize: 28,
                      fontWeight: FontWeight.bold,
                      letterSpacing: 1.2,
                    ),
                  ),
                ),

                const SizedBox(height: 8),

                // App tagline
                FadeTransition(
                  opacity: _fadeAnimation,
                  child: Text(
                    'Flutter MVC Demo',
                    style: TextStyle(
                      color: Colors.white.withOpacity(0.8),
                      fontSize: 16,
                      fontWeight: FontWeight.w300,
                    ),
                  ),
                ),

                const SizedBox(height: 60),

                // Loading indicator
                FadeTransition(
                  opacity: _fadeAnimation,
                  child: const CircularProgressIndicator(
                    valueColor: AlwaysStoppedAnimation<Color>(Colors.white),
                    strokeWidth: 2,
                  ),
                ),

                const SizedBox(height: 16),

                // Loading text
                FadeTransition(
                  opacity: _fadeAnimation,
                  child: Text(
                    'Initializing...',
                    style: TextStyle(
                      color: Colors.white.withOpacity(0.7),
                      fontSize: 14,
                    ),
                  ),
                ),
              ],
            ),
          );
        },
      ),
    );
  }
}
```

## presentation/views/widgets/common/custom_button.dart
**Purpose**: Reusable button widget with consistent styling and multiple variants.
**Benefits**: Ensures UI consistency, reduces code duplication, and provides flexible button configurations.

```dart
import 'package:flutter/material.dart';

/// Enumeration for different button variants
enum ButtonVariant {
  filled,    // Default filled button
  outlined,  // Outlined button
  text,      // Text button
}

/// Custom reusable button widget with consistent styling
/// Provides different variants and loading states
class CustomButton extends StatelessWidget {
  // Button text
  final String text;
  
  // Callback function when button is pressed
  final VoidCallback? onPressed;
  
  // Button variant (filled, outlined, text)
  final ButtonVariant variant;
  
  // Loading state indicator
  final bool isLoading;
  
  // Button width (null for default, double.infinity for full width)
  final double? width;
  
  // Button height
  final double? height;
  
  // Icon to display alongside text
  final IconData? icon;
  
  // Custom text style
  final TextStyle? textStyle;
  
  // Custom background color
  final Color? backgroundColor;
  
  // Custom foreground color
  final Color? foregroundColor;

  const CustomButton({
    Key? key,
    required this.text,
    this.onPressed,
    this.variant = ButtonVariant.filled,
    this.isLoading = false,
    this.width,
    this.height,
    this.icon,
    this.textStyle,
    this.backgroundColor,
    this.foregroundColor,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // Determine if button should be disabled
    final bool isDisabled = onPressed == null || isLoading;
    
    // Get theme colors
    final colorScheme = Theme.of(context).colorScheme;
    
    // Build button content (text with optional icon and loading indicator)
    Widget buttonContent = _buildButtonContent(context);
    
    // Wrap with SizedBox if width or height is specified
    if (width != null || height != null) {
      buttonContent = SizedBox(
        width: width,
        height: height ?? 48, // Default height
        child: buttonContent,
      );
    }
    
    // Return appropriate button widget based on variant
    switch (variant) {
      case ButtonVariant.filled:
        return ElevatedButton(
          onPressed: isDisabled ? null : onPressed,
          style: ElevatedButton.styleFrom(
            backgroundColor: backgroundColor ?? colorScheme.primary,
            foregroundColor: foregroundColor ?? colorScheme.onPrimary,
            disabledBackgroundColor: colorScheme.onSurface.withOpacity(0.12),
            disabledForegroundColor: colorScheme.onSurface.withOpacity(0.38),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
          ),
          child: buttonContent,
        );
        
      case ButtonVariant.outlined:
        return OutlinedButton(
          onPressed: isDisabled ? null : onPressed,
          style: OutlinedButton.styleFrom(
            foregroundColor: foregroundColor ?? colorScheme.primary,
            disabledForegroundColor: colorScheme.onSurface.withOpacity(0.38),
            side: BorderSide(
              color: isDisabled
                  ? colorScheme.onSurface.withOpacity(0.12)
                  : foregroundColor ?? colorScheme.primary,
            ),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
          ),
          child: buttonContent,
        );
        
      case ButtonVariant.text:
        return TextButton(
          onPressed: isDisabled ? null : onPressed,
          style: TextButton.styleFrom(
            foregroundColor: foregroundColor ?? colorScheme.primary,
            disabledForegroundColor: colorScheme.onSurface.withOpacity(0.38),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
            padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
          ),
          child: buttonContent,
        );
    }
  }
  
  /// Builds the button content with text, icon, and loading indicator
  Widget _buildButtonContent(BuildContext context) {
    // If loading, show loading indicator
    if (isLoading) {
      return Row(
        mainAxisSize: MainAxisSize.min,
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          SizedBox(
            width: 16,
            height: 16,
            child: CircularProgressIndicator(
              strokeWidth: 2,
              valueColor: AlwaysStoppedAnimation<Color>(
                variant == ButtonVariant.filled
                    ? Theme.of(context).colorScheme.onPrimary
                    : Theme.of(context).colorScheme.primary,
              ),
            ),
          ),
          const SizedBox(width: 8),
          Text(
            'Loading...',
            style: textStyle,
          ),
        ],
      );
    }
    
    // If icon is provided, show icon with text
    if (icon != null) {
      return Row(
        mainAxisSize: MainAxisSize.min,
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(icon, size: 18),
          const SizedBox(width: 8),
          Text(
            text,
            style: textStyle,
          ),
        ],
      );
    }
    
    // Default: just text
    return Text(
      text,
      style: textStyle,
    );
  }
}
```

