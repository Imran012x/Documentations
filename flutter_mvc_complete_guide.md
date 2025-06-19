# Complete Flutter MVC Application - Production Ready Structure

## Project Structure Overview

```
pubspec.yaml                         # Flutter project config file
README.md                            # Project documentation
.gitignore                           # Git ignore file (optional but recommended)

assets/                              # Assets folder for images, fonts, etc.
├── images/                          # Store image assets
│   └── logo.png
├── fonts/                           # Custom fonts if any
│   └── Roboto-Regular.ttf
└── translations/                    # Localization files if needed
    └── en.json

lib/
├── main.dart                        # App entry point
├── app/
│   ├── app.dart                     # Main app configuration
│   └── routes/
│       ├── app_routes.dart          # Route definitions
│       └── route_generator.dart     # Route generation logic
├── core/
│   ├── constants/
│   │   ├── app_constants.dart       # App-wide constants
│   │   ├── api_constants.dart       # API endpoints
│   │   └── theme_constants.dart     # Theme configuration
│   ├── utils/
│   │   ├── validators.dart          # Input validation utilities
│   │   ├── formatters.dart          # Data formatting utilities
│   │   └── extensions.dart          # Dart extensions
│   └── services/
│       ├── api_service.dart         # HTTP service layer
│       ├── storage_service.dart     # Local storage service
│       └── navigation_service.dart  # Navigation service
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
│   │       │   ├── custom_button.dart     # Reusable button widget
│   │       │   ├── custom_text_field.dart # Reusable text field
│   │       │   ├── loading_widget.dart    # Loading indicator
│   │       │   └── error_widget.dart      # Error display widget
│   │       └── specific/
│   │           ├── user_card.dart         # User-specific card widget
│   │           └── product_card.dart      # Product-specific card widget
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

test/                                # Unit and widget tests
├── home_screen_test.dart
└── widget_test.dart

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
## lib/presentation/views/widgets/common/custom_text_field.dart

**✅ What issue it solves/benefits:**
- Provides consistent text input styling across the entire application
- Encapsulates validation logic and error handling in a reusable component
- Reduces code duplication and maintains design consistency
- Centralizes text field behavior modifications and updates
- Improves maintainability by having a single source of truth for text inputs

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

/// Custom reusable text field widget that provides consistent styling
/// and behavior across the application
class CustomTextField extends StatefulWidget {
  // Text field label displayed above the input
  final String label;
  
  // Placeholder text shown when field is empty
  final String? hintText;
  
  // Controller to manage the text field's value
  final TextEditingController? controller;
  
  // Function called when text changes
  final ValueChanged<String>? onChanged;
  
  // Function called when editing is complete
  final VoidCallback? onEditingComplete;
  
  // Function called when field is submitted
  final ValueChanged<String>? onSubmitted;
  
  // Validation function that returns error message or null
  final String? Function(String?)? validator;
  
  // Whether this field is for password input (hides text)
  final bool isPassword;
  
  // Whether this field is required (shows asterisk)
  final bool isRequired;
  
  // Whether this field is enabled for input
  final bool isEnabled;
  
  // Whether this field should be read-only
  final bool isReadOnly;
  
  // Maximum number of lines for the text field
  final int maxLines;
  
  // Maximum length of text allowed
  final int? maxLength;
  
  // Keyboard type for the input
  final TextInputType keyboardType;
  
  // Input formatters to restrict input
  final List<TextInputFormatter>? inputFormatters;
  
  // Prefix icon for the text field
  final Widget? prefixIcon;
  
  // Suffix icon for the text field
  final Widget? suffixIcon;
  
  // Initial value for the text field
  final String? initialValue;
  
  // Focus node for controlling focus
  final FocusNode? focusNode;
  
  // Text capitalization behavior
  final TextCapitalization textCapitalization;

  const CustomTextField({
    Key? key,
    required this.label,
    this.hintText,
    this.controller,
    this.onChanged,
    this.onEditingComplete,
    this.onSubmitted,
    this.validator,
    this.isPassword = false,
    this.isRequired = false,
    this.isEnabled = true,
    this.isReadOnly = false,
    this.maxLines = 1,
    this.maxLength,
    this.keyboardType = TextInputType.text,
    this.inputFormatters,
    this.prefixIcon,
    this.suffixIcon,
    this.initialValue,
    this.focusNode,
    this.textCapitalization = TextCapitalization.none,
  }) : super(key: key);

  @override
  State<CustomTextField> createState() => _CustomTextFieldState();
}

class _CustomTextFieldState extends State<CustomTextField> {
  // Controls whether password text is visible
  bool _isPasswordVisible = false;
  
  // Internal controller if none provided
  late TextEditingController _controller;
  
  // Whether this field currently has validation errors
  bool _hasError = false;

  @override
  void initState() {
    super.initState();
    
    // Initialize controller if not provided
    _controller = widget.controller ?? TextEditingController();
    
    // Set initial value if provided and no controller given
    if (widget.initialValue != null && widget.controller == null) {
      _controller.text = widget.initialValue!;
    }
    
    // Listen for text changes to clear error state
    _controller.addListener(_onTextChanged);
  }

  @override
  void dispose() {
    // Clean up controller listener
    _controller.removeListener(_onTextChanged);
    
    // Dispose controller only if we created it internally
    if (widget.controller == null) {
      _controller.dispose();
    }
    
    super.dispose();
  }

  /// Called when text changes to clear error state
  void _onTextChanged() {
    if (_hasError) {
      setState(() {
        _hasError = false;
      });
    }
  }

  /// Toggles password visibility
  void _togglePasswordVisibility() {
    setState(() {
      _isPasswordVisible = !_isPasswordVisible;
    });
  }

  /// Validates the current text and updates error state
  String? _validateText(String? value) {
    final error = widget.validator?.call(value);
    
    // Update error state for UI changes
    WidgetsBinding.instance.addPostFrameCallback((_) {
      if (mounted) {
        setState(() {
          _hasError = error != null;
        });
      }
    });
    
    return error;
  }

  @override
  Widget build(BuildContext context) {
    // Get theme for consistent styling
    final theme = Theme.of(context);
    
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        // Label with optional required indicator
        _buildLabel(theme),
        
        const SizedBox(height: 8),
        
        // Main text field
        _buildTextField(theme),
      ],
    );
  }

  /// Builds the label widget with optional required asterisk
  Widget _buildLabel(ThemeData theme) {
    return RichText(
      text: TextSpan(
        text: widget.label,
        style: theme.textTheme.bodyMedium?.copyWith(
          fontWeight: FontWeight.w500,
          color: theme.colorScheme.onSurface,
        ),
        children: widget.isRequired
            ? [
                TextSpan(
                  text: ' *',
                  style: TextStyle(
                    color: theme.colorScheme.error,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ]
            : null,
      ),
    );
  }

  /// Builds the main text field with all configurations
  Widget _buildTextField(ThemeData theme) {
    return TextFormField(
      // Basic configuration
      controller: _controller,
      focusNode: widget.focusNode,
      enabled: widget.isEnabled,
      readOnly: widget.isReadOnly,
      maxLines: widget.maxLines,
      maxLength: widget.maxLength,
      keyboardType: widget.keyboardType,
      textCapitalization: widget.textCapitalization,
      inputFormatters: widget.inputFormatters,
      
      // Password configuration
      obscureText: widget.isPassword && !_isPasswordVisible,
      
      // Callbacks
      onChanged: widget.onChanged,
      onEditingComplete: widget.onEditingComplete,
      onFieldSubmitted: widget.onSubmitted,
      validator: widget.validator != null ? _validateText : null,
      
      // Styling
      style: theme.textTheme.bodyLarge?.copyWith(
        color: widget.isEnabled 
            ? theme.colorScheme.onSurface 
            : theme.colorScheme.onSurface.withOpacity(0.6),
      ),
      
      decoration: InputDecoration(
        // Hint text
        hintText: widget.hintText,
        hintStyle: theme.textTheme.bodyLarge?.copyWith(
          color: theme.colorScheme.onSurface.withOpacity(0.6),
        ),
        
        // Icons
        prefixIcon: widget.prefixIcon,
        suffixIcon: _buildSuffixIcon(),
        
        // Border styling
        border: _buildBorder(theme, false),
        enabledBorder: _buildBorder(theme, false),
        focusedBorder: _buildBorder(theme, true),
        errorBorder: _buildErrorBorder(theme),
        focusedErrorBorder: _buildErrorBorder(theme),
        disabledBorder: _buildDisabledBorder(theme),
        
        // Fill color
        filled: true,
        fillColor: widget.isEnabled
            ? theme.colorScheme.surface
            : theme.colorScheme.surfaceVariant.withOpacity(0.3),
        
        // Content padding
        contentPadding: const EdgeInsets.symmetric(
          horizontal: 16,
          vertical: 12,
        ),
        
        // Counter style for max length
        counterStyle: theme.textTheme.bodySmall?.copyWith(
          color: theme.colorScheme.onSurface.withOpacity(0.6),
        ),
        
        // Error styling
        errorStyle: theme.textTheme.bodySmall?.copyWith(
          color: theme.colorScheme.error,
        ),
        errorMaxLines: 2,
      ),
    );
  }

  /// Builds the suffix icon (password toggle or custom)
  Widget? _buildSuffixIcon() {
    if (widget.isPassword) {
      // Password visibility toggle
      return IconButton(
        icon: Icon(
          _isPasswordVisible ? Icons.visibility_off : Icons.visibility,
          color: Theme.of(context).colorScheme.onSurface.withOpacity(0.6),
        ),
        onPressed: _togglePasswordVisibility,
        tooltip: _isPasswordVisible ? 'Hide password' : 'Show password',
      );
    }
    
    return widget.suffixIcon;
  }

  /// Builds normal border
  OutlineInputBorder _buildBorder(ThemeData theme, bool isFocused) {
    return OutlineInputBorder(
      borderRadius: BorderRadius.circular(8),
      borderSide: BorderSide(
        color: isFocused
            ? theme.colorScheme.primary
            : theme.colorScheme.outline.withOpacity(0.5),
        width: isFocused ? 2 : 1,
      ),
    );
  }

  /// Builds error border
  OutlineInputBorder _buildErrorBorder(ThemeData theme) {
    return OutlineInputBorder(
      borderRadius: BorderRadius.circular(8),
      borderSide: BorderSide(
        color: theme.colorScheme.error,
        width: 2,
      ),
    );
  }

  /// Builds disabled border
  OutlineInputBorder _buildDisabledBorder(ThemeData theme) {
    return OutlineInputBorder(
      borderRadius: BorderRadius.circular(8),
      borderSide: BorderSide(
        color: theme.colorScheme.outline.withOpacity(0.3),
        width: 1,
      ),
    );
  }
}

/// Extension to provide common text field configurations
extension CustomTextFieldExtensions on CustomTextField {
  /// Creates an email text field
  static CustomTextField email({
    Key? key,
    String label = 'Email',
    String? hintText = 'Enter your email',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    String? Function(String?)? validator,
    bool isRequired = true,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      validator: validator,
      isRequired: isRequired,
      isEnabled: isEnabled,
      keyboardType: TextInputType.emailAddress,
      textCapitalization: TextCapitalization.none,
      prefixIcon: const Icon(Icons.email_outlined),
    );
  }

  /// Creates a password text field
  static CustomTextField password({
    Key? key,
    String label = 'Password',
    String? hintText = 'Enter your password',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    String? Function(String?)? validator,
    bool isRequired = true,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      validator: validator,
      isRequired: isRequired,
      isEnabled: isEnabled,
      isPassword: true,
      prefixIcon: const Icon(Icons.lock_outlined),
    );
  }

  /// Creates a phone number text field
  static CustomTextField phone({
    Key? key,
    String label = 'Phone Number',
    String? hintText = 'Enter your phone number',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    String? Function(String?)? validator,
    bool isRequired = true,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      validator: validator,
      isRequired: isRequired,
      isEnabled: isEnabled,
      keyboardType: TextInputType.phone,
      prefixIcon: const Icon(Icons.phone_outlined),
      inputFormatters: [
        FilteringTextInputFormatter.digitsOnly,
        LengthLimitingTextInputFormatter(15),
      ],
    );
  }

  /// Creates a search text field
  static CustomTextField search({
    Key? key,
    String label = 'Search',
    String? hintText = 'Search...',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    ValueChanged<String>? onSubmitted,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      onSubmitted: onSubmitted,
      isEnabled: isEnabled,
      keyboardType: TextInputType.text,
      prefixIcon: const Icon(Icons.search_outlined),
      suffixIcon: controller?.text.isNotEmpty == true
          ? IconButton(
              icon: const Icon(Icons.clear),
              onPressed: () => controller?.clear(),
            )
          : null,
    );
  }
}
```

## lib/presentation/views/widgets/common/loading_widget.dart

**Purpose**: Provides a reusable loading indicator widget that can be used across different screens to show loading states consistently throughout the app.

**Benefits of Separation**:
- ✅ **Consistency**: Ensures all loading states look the same across the app
- ✅ **Reusability**: Can be used in any screen or widget without code duplication
- ✅ **Maintainability**: Changes to loading design only need to be made in one place
- ✅ **Customization**: Allows for different loading styles based on context
- ✅ **Performance**: Optimized widget that doesn't rebuild unnecessarily

```dart
import 'package:flutter/material.dart';

/// A reusable loading widget that provides consistent loading indicators
/// across the entire application with customizable options
class LoadingWidget extends StatelessWidget {
  // Message to display below the loading indicator
  final String? message;
  
  // Size of the loading indicator (default: 50.0)
  final double size;
  
  // Color of the loading indicator (uses theme primary color if null)
  final Color? color;
  
  // Whether to show a semi-transparent background overlay
  final bool showOverlay;
  
  // Type of loading indicator to display
  final LoadingType type;

  /// Creates a loading widget with customizable properties
  /// 
  /// [message] - Optional text to display below the loading indicator
  /// [size] - Size of the loading indicator (default: 50.0)
  /// [color] - Color of the loading indicator
  /// [showOverlay] - Whether to show background overlay (default: false)
  /// [type] - Type of loading indicator (default: circular)
  const LoadingWidget({
    Key? key,
    this.message,
    this.size = 50.0,
    this.color,
    this.showOverlay = false,
    this.type = LoadingType.circular,
  }) : super(key: key);

  /// Factory constructor for creating a full-screen loading overlay
  /// Used when you want to block the entire screen during loading
  factory LoadingWidget.overlay({
    String? message,
    double size = 50.0,
    Color? color,
    LoadingType type = LoadingType.circular,
  }) {
    return LoadingWidget(
      message: message,
      size: size,
      color: color,
      showOverlay: true,
      type: type,
    );
  }

  /// Factory constructor for creating a small inline loading indicator
  /// Used for buttons or small sections that are loading
  factory LoadingWidget.small({
    String? message,
    Color? color,
    LoadingType type = LoadingType.circular,
  }) {
    return LoadingWidget(
      message: message,
      size: 20.0,
      color: color,
      showOverlay: false,
      type: type,
    );
  }

  /// Factory constructor for creating a large prominent loading indicator
  /// Used for main content areas that are loading
  factory LoadingWidget.large({
    String? message,
    Color? color,
    LoadingType type = LoadingType.circular,
  }) {
    return LoadingWidget(
      message: message,
      size: 80.0,
      color: color,
      showOverlay: false,
      type: type,
    );
  }

  @override
  Widget build(BuildContext context) {
    // Get the theme for consistent styling
    final theme = Theme.of(context);
    
    // Determine the color to use (provided color or theme primary color)
    final indicatorColor = color ?? theme.primaryColor;
    
    // Build the main loading content
    Widget loadingContent = _buildLoadingContent(theme, indicatorColor);
    
    // If overlay is requested, wrap with a modal barrier
    if (showOverlay) {
      return _buildOverlayContent(loadingContent);
    }
    
    return loadingContent;
  }

  /// Builds the main loading content with indicator and optional message
  Widget _buildLoadingContent(ThemeData theme, Color indicatorColor) {
    return Center(
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          // Display the appropriate loading indicator based on type
          _buildLoadingIndicator(indicatorColor),
          
          // Show message if provided
          if (message != null) ...[
            const SizedBox(height: 16.0),
            Text(
              message!,
              style: theme.textTheme.bodyMedium?.copyWith(
                color: theme.textTheme.bodyMedium?.color?.withOpacity(0.7),
              ),
              textAlign: TextAlign.center,
            ),
          ],
        ],
      ),
    );
  }

  /// Builds the loading indicator based on the specified type
  Widget _buildLoadingIndicator(Color indicatorColor) {
    switch (type) {
      case LoadingType.circular:
        // Standard circular progress indicator
        return SizedBox(
          width: size,
          height: size,
          child: CircularProgressIndicator(
            valueColor: AlwaysStoppedAnimation<Color>(indicatorColor),
            strokeWidth: 3.0,
          ),
        );
      
      case LoadingType.linear:
        // Linear progress indicator for horizontal loading
        return SizedBox(
          width: size * 2, // Make linear indicators wider
          child: LinearProgressIndicator(
            valueColor: AlwaysStoppedAnimation<Color>(indicatorColor),
            backgroundColor: indicatorColor.withOpacity(0.2),
          ),
        );
      
      case LoadingType.dots:
        // Custom animated dots loading indicator
        return SizedBox(
          width: size,
          height: size,
          child: _DotsLoadingIndicator(
            color: indicatorColor,
            size: size,
          ),
        );
      
      case LoadingType.pulse:
        // Pulsing circle loading indicator
        return SizedBox(
          width: size,
          height: size,
          child: _PulseLoadingIndicator(
            color: indicatorColor,
            size: size,
          ),
        );
    }
  }

  /// Builds the overlay content with semi-transparent background
  Widget _buildOverlayContent(Widget child) {
    return Container(
      // Full screen overlay
      width: double.infinity,
      height: double.infinity,
      // Semi-transparent dark background
      color: Colors.black.withOpacity(0.5),
      child: child,
    );
  }
}

/// Enum defining different types of loading indicators available
enum LoadingType {
  circular,  // Standard circular progress indicator
  linear,    // Horizontal linear progress indicator
  dots,      // Custom animated dots
  pulse,     // Pulsing circle animation
}

/// Custom animated dots loading indicator widget
class _DotsLoadingIndicator extends StatefulWidget {
  final Color color;
  final double size;

  const _DotsLoadingIndicator({
    required this.color,
    required this.size,
  });

  @override
  State<_DotsLoadingIndicator> createState() => _DotsLoadingIndicatorState();
}

class _DotsLoadingIndicatorState extends State<_DotsLoadingIndicator>
    with TickerProviderStateMixin {
  late AnimationController _controller;
  late List<Animation<double>> _animations;

  @override
  void initState() {
    super.initState();
    
    // Create animation controller for dots
    _controller = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    );
    
    // Create staggered animations for each dot
    _animations = List.generate(3, (index) {
      return Tween<double>(
        begin: 0.0,
        end: 1.0,
      ).animate(
        CurvedAnimation(
          parent: _controller,
          curve: Interval(
            index * 0.2,
            (index * 0.2) + 0.6,
            curve: Curves.easeInOut,
          ),
        ),
      );
    });
    
    // Start the animation and repeat
    _controller.repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final dotSize = widget.size / 8; // Calculate dot size relative to total size
    
    return AnimatedBuilder(
      animation: _controller,
      builder: (context, child) {
        return Row(
          mainAxisSize: MainAxisSize.min,
          children: List.generate(3, (index) {
            return Padding(
              padding: EdgeInsets.symmetric(horizontal: dotSize / 4),
              child: Transform.scale(
                scale: 0.5 + (_animations[index].value * 0.5),
                child: Container(
                  width: dotSize,
                  height: dotSize,
                  decoration: BoxDecoration(
                    color: widget.color.withOpacity(0.3 + (_animations[index].value * 0.7)),
                    shape: BoxShape.circle,
                  ),
                ),
              ),
            );
          }),
        );
      },
    );
  }
}

/// Custom pulsing circle loading indicator widget
class _PulseLoadingIndicator extends StatefulWidget {
  final Color color;
  final double size;

  const _PulseLoadingIndicator({
    required this.color,
    required this.size,
  });

  @override
  State<_PulseLoadingIndicator> createState() => _PulseLoadingIndicatorState();
}

class _PulseLoadingIndicatorState extends State<_PulseLoadingIndicator>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    
    // Create animation controller for pulse effect
    _controller = AnimationController(
      duration: const Duration(milliseconds: 1000),
      vsync: this,
    );
    
    // Create scaling animation for pulse effect
    _animation = Tween<double>(
      begin: 0.5,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeInOut,
      ),
    );
    
    // Start the animation and repeat
    _controller.repeat(reverse: true);
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Transform.scale(
          scale: _animation.value,
          child: Container(
            width: widget.size,
            height: widget.size,
            decoration: BoxDecoration(
              color: widget.color.withOpacity(0.3 + (_animation.value * 0.4)),
              shape: BoxShape.circle,
            ),
          ),
        );
      },
    );
  }
}
```

### Usage Examples:

```dart
// Basic usage in a screen
class ExampleScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: LoadingWidget(
        message: "Loading data...",
      ),
    );
  }
}

// Usage with different types
LoadingWidget(type: LoadingType.dots)
LoadingWidget.small(message: "Saving...")
LoadingWidget.large(message: "Processing...")
LoadingWidget.overlay(message: "Please wait...")

// Usage in a conditional widget
Consumer<UserProvider>(
  builder: (context, userProvider, child) {
    if (userProvider.isLoading) {
      return LoadingWidget(message: "Loading users...");
    }
    return UserListView();
  },
)
```



## lib/presentation/views/widgets/common/error_widget.dart

**✅ What issue it solves/benefits:**
- Provides consistent error handling and display across the application
- Offers different error types (network, validation, generic) with appropriate actions
- Centralizes error UI patterns and reduces code duplication
- Improves user experience with clear error messages and recovery options
- Supports both inline and full-screen error displays

```dart
import 'package:flutter/material.dart';

/// Enum to define different types of errors
enum ErrorType {
  network,        // Network/connectivity errors
  validation,     // Form validation errors
  notFound,       // Resource not found errors
  permission,     // Permission denied errors
  generic,        // Generic application errors
  timeout,        // Request timeout errors
}

/// Custom error widget that provides consistent error display
/// throughout the application with various error types and actions
class CustomErrorWidget extends StatelessWidget {
  // Type of error to display
  final ErrorType errorType;
  
  // Main error message
  final String message;
  
  // Detailed error description (optional)
  final String? description;
  
  // Function to call when retry button is pressed
  final VoidCallback? onRetry;
  
  // Function to call when secondary action is pressed
  final VoidCallback? onSecondaryAction;
  
  // Custom retry button text
  final String? retryText;
  
  // Custom secondary action text
  final String? secondaryActionText;
  
  // Whether to show the retry button
  final bool showRetry;
  
  // Whether to show secondary action button
  final bool showSecondaryAction;
  
  // Custom icon for the error
  final Widget? customIcon;
  
  // Whether this is a compact error display
  final bool isCompact;
  
  // Background color for the error container
  final Color? backgroundColor;

  const CustomErrorWidget({
    Key? key,
    required this.errorType,
    required this.message,
    this.description,
    this.onRetry,
    this.onSecondaryAction,
    this.retryText,
    this.secondaryActionText,
    this.showRetry = true,
    this.showSecondaryAction = false,
    this.customIcon,
    this.isCompact = false,
    this.backgroundColor,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    
    if (isCompact) {
      return _buildCompactError(theme);
    }
    
    return _buildFullError(theme);
  }

  /// Builds compact error display for inline usage
  Widget _buildCompactError(ThemeData theme) {
    return Container(
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: backgroundColor ?? _getErrorColor(theme).withOpacity(0.1),
        borderRadius: BorderRadius.circular(8),
        border: Border.all(
          color: _getErrorColor(theme).withOpacity(0.3),
          width: 1,
        ),
      ),
      child: Row(
        children: [
          // Error icon
          Icon(
            _getErrorIcon(),
            color: _getErrorColor(theme),
            size: 20,
          ),
          
          const SizedBox(width: 12),
          
          // Error message
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(
                  message,
                  style: theme.textTheme.bodyMedium?.copyWith(
                    color: theme.colorScheme.onSurface,
                    fontWeight: FontWeight.w500,
                  ),
                ),
                
                if (description != null) ...[
                  const SizedBox(height: 4),
                  Text(
                    description!,
                    style: theme.textTheme.bodySmall?.copyWith(
                      color: theme.colorScheme.onSurface.withOpacity(0.7),
                    ),
                  ),
                ],
              ],
            ),
          ),
          
          // Retry button
          if (showRetry && onRetry != null) ...[
            const SizedBox(width: 8),
            TextButton(
              onPressed: onRetry,
              style: TextButton.styleFrom(
                foregroundColor: _getErrorColor(theme),
                padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 4),
                minimumSize: Size.zero,
                tapTargetSize: MaterialTapTargetSize.shrinkWrap,
              ),
              child: Text(retryText ?? 'Retry'),
            ),
          ],
        ],
      ),
    );
  }

  /// Builds full error display for screen-level usage
  Widget _buildFullError(ThemeData theme) {
    return Container(
      width: double.infinity,
      padding: const EdgeInsets.all(24),
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          // Error illustration/icon
          Container(
            width: 80,
            height: 80,
            decoration: BoxDecoration(
              color: _getErrorColor(theme).withOpacity(0.1),
              shape: BoxShape.circle,
            ),
            child: customIcon ?? Icon(
              _getErrorIcon(),
              size: 40,
              color: _getErrorColor(theme),
            ),
          ),
          
          const SizedBox(height: 24),
          
          // Error title
          Text(
            _getErrorTitle(),
            style: theme.textTheme.headlineSmall?.copyWith(
              color: theme.colorScheme.onSurface,
              fontWeight: FontWeight.w600,
            ),
            textAlign: TextAlign.center,
          ),
          
          const SizedBox(height: 12),
          
          // Error message
          Text(
            message,
            style: theme.textTheme.bodyLarge?.copyWith(
              color: theme.colorScheme.onSurface.withOpacity(0.8),
            ),
            textAlign: TextAlign.center,
          ),
          
          // Error description
          if (description != null) ...[
            const SizedBox(height: 8),
            Text(
              description!,
              style: theme.textTheme.bodyMedium?.copyWith(
                color: theme.colorScheme.onSurface.withOpacity(0.7),
              ),
              textAlign: TextAlign.center,
            ),
          ],
          
          const SizedBox(height: 32),
          
          // Action buttons
          _buildActionButtons(theme),
        ],
      ),
    );
  }

  /// Builds action buttons (retry, secondary action)
  Widget _buildActionButtons(ThemeData theme) {
    if (!showRetry && !showSecondaryAction) {
      return const SizedBox.shrink();
    }
    
    return Column(
      children: [
        // Primary retry button
        if (showRetry && onRetry != null)
          SizedBox(
            width: double.infinity,
            child: ElevatedButton(
              onPressed: onRetry,
              style: ElevatedButton.styleFrom(
                backgroundColor: _getErrorColor(theme),
                foregroundColor: Colors.white,
                padding: const EdgeInsets.symmetric(vertical: 12),
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(8),
                ),
              ),
              child: Text(retryText ?? _getDefaultRetryText()),
            ),
          ),
        
        // Secondary action button
        if (showSecondaryAction && onSecondaryAction != null) ...[
          const SizedBox(height: 12),
          SizedBox(
            width: double.infinity,
            child: OutlinedButton(
              onPressed: onSecondaryAction,
              style: OutlinedButton.styleFrom(
                foregroundColor: theme.colorScheme.onSurface,
                side: BorderSide(color: theme.colorScheme.outline),
                padding: const EdgeInsets.symmetric(vertical: 12),
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(8),
                ),
              ),
              child: Text(secondaryActionText ?? 'Go Back'),
            ),
          ),
        ],
      ],
    );
  }

  /// Gets the appropriate error color based on error type
  Color _getErrorColor(ThemeData theme) {
    switch (errorType) {
      case ErrorType.network:
        return theme.colorScheme.error;
      case ErrorType.validation:
        return Colors.orange;
      case ErrorType.notFound:
        return Colors.blue;
      case ErrorType.permission:
        return Colors.red;
      case ErrorType.timeout:
        return Colors.amber;
      case ErrorType.generic:
      default:
        return theme.colorScheme.error;
    }
  }

  /// Gets the appropriate error icon based on error type
  IconData _getErrorIcon() {
    switch (errorType) {
      case ErrorType.network:
        return Icons.wifi_off_rounded;
      case ErrorType.validation:
        return Icons.warning_rounded;
      case ErrorType.notFound:
        return Icons.search_off_rounded;
      case ErrorType.permission:
        return Icons.lock_rounded;
      case ErrorType.timeout:
        return Icons.access_time_rounded;
      case ErrorType.generic:
      default:
        return Icons.error_rounded;
    }
  }

  /// Gets the appropriate error title based on error type
  String _getErrorTitle() {
    switch (errorType) {
      case ErrorType.network:
        return 'Connection Error';
      case ErrorType.validation:
        return 'Validation Error';
      case ErrorType.notFound:
        return 'Not Found';
      case ErrorType.permission:
        return 'Access Denied';
      case ErrorType.timeout:
        return 'Request Timeout';
      case ErrorType.generic:
      default:
        return 'Something Went Wrong';
    }
  }

  /// Gets the default retry text based on error type
  String _getDefaultRetryText() {
    switch (errorType) {
      case ErrorType.network:
        return 'Retry Connection';
      case ErrorType.timeout:
        return 'Try Again';
      case ErrorType.generic:
      default:
        return 'Retry';
    }
  }
}

/// Factory methods for common error scenarios
extension CustomErrorWidgetFactory on CustomErrorWidget {
  /// Creates a network error widget
  static CustomErrorWidget network({
    Key? key,
    String? message = 'Unable to connect to the internet',
    String? description = 'Please check your connection and try again',
    VoidCallback? onRetry,
    bool isCompact = false,
  }) {
    return CustomErrorWidget(
      key: key,
      errorType: ErrorType.network,
      message: message!,
      description: description,
      onRetry: onRetry,
      isCompact: isCompact,
    );
  }

  /// Creates a not found error widget
  static CustomErrorWidget notFound({
    Key? key,
    String? message = 'Content not found',
    String? description = 'The requested content could not be found',
    VoidCallback? onSecondaryAction,
    bool isCompact = false,
  }) {
    return CustomErrorWidget(
      key: key,
      errorType: ErrorType.notFound,
      message: message!,
      description: description,
      showRetry: false,
      showSecondaryAction: true,
      onSecondaryAction: onSecondaryAction,
      secondaryActionText: 'Go Back',
      isCompact: isCompact,
    );
  }

  /// Creates a permission error widget
  static CustomErrorWidget permission({
    Key? key,
    String? message = 'Permission denied',
    String? description = 'You don\'t have permission to access this content',
    VoidCallback? onSecondaryAction,
    bool isCompact = false,
  }) {
    return CustomErrorWidget(
      key: key,
      errorType: ErrorType.permission,
      message: message!,
      description: description,
      showRetry: false,
      showSecondaryAction: true,
      onSecondaryAction: onSecondaryAction,
      secondaryActionText: 'Go Back',
      isCompact: isCompact,
    );
  }

  /// Creates a generic error widget
  static CustomErrorWidget generic({
    Key? key,
    String? message = 'An unexpected error occurred',
    String? description,
    VoidCallback? onRetry,
    bool isCompact = false,
  }) {
    return CustomErrorWidget(
      key: key,
      errorType: ErrorType.generic,
      message: message!,
      description: description,
      onRetry: onRetry,
      isCompact: isCompact,
    );
  }
}
```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

/// Custom reusable text field widget that provides consistent styling
/// and behavior across the application
class CustomTextField extends StatefulWidget {
  // Text field label displayed above the input
  final String label;
  
  // Placeholder text shown when field is empty
  final String? hintText;
  
  // Controller to manage the text field's value
  final TextEditingController? controller;
  
  // Function called when text changes
  final ValueChanged<String>? onChanged;
  
  // Function called when editing is complete
  final VoidCallback? onEditingComplete;
  
  // Function called when field is submitted
  final ValueChanged<String>? onSubmitted;
  
  // Validation function that returns error message or null
  final String? Function(String?)? validator;
  
  // Whether this field is for password input (hides text)
  final bool isPassword;
  
  // Whether this field is required (shows asterisk)
  final bool isRequired;
  
  // Whether this field is enabled for input
  final bool isEnabled;
  
  // Whether this field should be read-only
  final bool isReadOnly;
  
  // Maximum number of lines for the text field
  final int maxLines;
  
  // Maximum length of text allowed
  final int? maxLength;
  
  // Keyboard type for the input
  final TextInputType keyboardType;
  
  // Input formatters to restrict input
  final List<TextInputFormatter>? inputFormatters;
  
  // Prefix icon for the text field
  final Widget? prefixIcon;
  
  // Suffix icon for the text field
  final Widget? suffixIcon;
  
  // Initial value for the text field
  final String? initialValue;
  
  // Focus node for controlling focus
  final FocusNode? focusNode;
  
  // Text capitalization behavior
  final TextCapitalization textCapitalization;

  const CustomTextField({
    Key? key,
    required this.label,
    this.hintText,
    this.controller,
    this.onChanged,
    this.onEditingComplete,
    this.onSubmitted,
    this.validator,
    this.isPassword = false,
    this.isRequired = false,
    this.isEnabled = true,
    this.isReadOnly = false,
    this.maxLines = 1,
    this.maxLength,
    this.keyboardType = TextInputType.text,
    this.inputFormatters,
    this.prefixIcon,
    this.suffixIcon,
    this.initialValue,
    this.focusNode,
    this.textCapitalization = TextCapitalization.none,
  }) : super(key: key);

  @override
  State<CustomTextField> createState() => _CustomTextFieldState();
}

class _CustomTextFieldState extends State<CustomTextField> {
  // Controls whether password text is visible
  bool _isPasswordVisible = false;
  
  // Internal controller if none provided
  late TextEditingController _controller;
  
  // Whether this field currently has validation errors
  bool _hasError = false;

  @override
  void initState() {
    super.initState();
    
    // Initialize controller if not provided
    _controller = widget.controller ?? TextEditingController();
    
    // Set initial value if provided and no controller given
    if (widget.initialValue != null && widget.controller == null) {
      _controller.text = widget.initialValue!;
    }
    
    // Listen for text changes to clear error state
    _controller.addListener(_onTextChanged);
  }

  @override
  void dispose() {
    // Clean up controller listener
    _controller.removeListener(_onTextChanged);
    
    // Dispose controller only if we created it internally
    if (widget.controller == null) {
      _controller.dispose();
    }
    
    super.dispose();
  }

  /// Called when text changes to clear error state
  void _onTextChanged() {
    if (_hasError) {
      setState(() {
        _hasError = false;
      });
    }
  }

  /// Toggles password visibility
  void _togglePasswordVisibility() {
    setState(() {
      _isPasswordVisible = !_isPasswordVisible;
    });
  }

  /// Validates the current text and updates error state
  String? _validateText(String? value) {
    final error = widget.validator?.call(value);
    
    // Update error state for UI changes
    WidgetsBinding.instance.addPostFrameCallback((_) {
      if (mounted) {
        setState(() {
          _hasError = error != null;
        });
      }
    });
    
    return error;
  }

  @override
  Widget build(BuildContext context) {
    // Get theme for consistent styling
    final theme = Theme.of(context);
    
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        // Label with optional required indicator
        _buildLabel(theme),
        
        const SizedBox(height: 8),
        
        // Main text field
        _buildTextField(theme),
      ],
    );
  }

  /// Builds the label widget with optional required asterisk
  Widget _buildLabel(ThemeData theme) {
    return RichText(
      text: TextSpan(
        text: widget.label,
        style: theme.textTheme.bodyMedium?.copyWith(
          fontWeight: FontWeight.w500,
          color: theme.colorScheme.onSurface,
        ),
        children: widget.isRequired
            ? [
                TextSpan(
                  text: ' *',
                  style: TextStyle(
                    color: theme.colorScheme.error,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ]
            : null,
      ),
    );
  }

  /// Builds the main text field with all configurations
  Widget _buildTextField(ThemeData theme) {
    return TextFormField(
      // Basic configuration
      controller: _controller,
      focusNode: widget.focusNode,
      enabled: widget.isEnabled,
      readOnly: widget.isReadOnly,
      maxLines: widget.maxLines,
      maxLength: widget.maxLength,
      keyboardType: widget.keyboardType,
      textCapitalization: widget.textCapitalization,
      inputFormatters: widget.inputFormatters,
      
      // Password configuration
      obscureText: widget.isPassword && !_isPasswordVisible,
      
      // Callbacks
      onChanged: widget.onChanged,
      onEditingComplete: widget.onEditingComplete,
      onFieldSubmitted: widget.onSubmitted,
      validator: widget.validator != null ? _validateText : null,
      
      // Styling
      style: theme.textTheme.bodyLarge?.copyWith(
        color: widget.isEnabled 
            ? theme.colorScheme.onSurface 
            : theme.colorScheme.onSurface.withOpacity(0.6),
      ),
      
      decoration: InputDecoration(
        // Hint text
        hintText: widget.hintText,
        hintStyle: theme.textTheme.bodyLarge?.copyWith(
          color: theme.colorScheme.onSurface.withOpacity(0.6),
        ),
        
        // Icons
        prefixIcon: widget.prefixIcon,
        suffixIcon: _buildSuffixIcon(),
        
        // Border styling
        border: _buildBorder(theme, false),
        enabledBorder: _buildBorder(theme, false),
        focusedBorder: _buildBorder(theme, true),
        errorBorder: _buildErrorBorder(theme),
        focusedErrorBorder: _buildErrorBorder(theme),
        disabledBorder: _buildDisabledBorder(theme),
        
        // Fill color
        filled: true,
        fillColor: widget.isEnabled
            ? theme.colorScheme.surface
            : theme.colorScheme.surfaceVariant.withOpacity(0.3),
        
        // Content padding
        contentPadding: const EdgeInsets.symmetric(
          horizontal: 16,
          vertical: 12,
        ),
        
        // Counter style for max length
        counterStyle: theme.textTheme.bodySmall?.copyWith(
          color: theme.colorScheme.onSurface.withOpacity(0.6),
        ),
        
        // Error styling
        errorStyle: theme.textTheme.bodySmall?.copyWith(
          color: theme.colorScheme.error,
        ),
        errorMaxLines: 2,
      ),
    );
  }

  /// Builds the suffix icon (password toggle or custom)
  Widget? _buildSuffixIcon() {
    if (widget.isPassword) {
      // Password visibility toggle
      return IconButton(
        icon: Icon(
          _isPasswordVisible ? Icons.visibility_off : Icons.visibility,
          color: Theme.of(context).colorScheme.onSurface.withOpacity(0.6),
        ),
        onPressed: _togglePasswordVisibility,
        tooltip: _isPasswordVisible ? 'Hide password' : 'Show password',
      );
    }
    
    return widget.suffixIcon;
  }

  /// Builds normal border
  OutlineInputBorder _buildBorder(ThemeData theme, bool isFocused) {
    return OutlineInputBorder(
      borderRadius: BorderRadius.circular(8),
      borderSide: BorderSide(
        color: isFocused
            ? theme.colorScheme.primary
            : theme.colorScheme.outline.withOpacity(0.5),
        width: isFocused ? 2 : 1,
      ),
    );
  }

  /// Builds error border
  OutlineInputBorder _buildErrorBorder(ThemeData theme) {
    return OutlineInputBorder(
      borderRadius: BorderRadius.circular(8),
      borderSide: BorderSide(
        color: theme.colorScheme.error,
        width: 2,
      ),
    );
  }

  /// Builds disabled border
  OutlineInputBorder _buildDisabledBorder(ThemeData theme) {
    return OutlineInputBorder(
      borderRadius: BorderRadius.circular(8),
      borderSide: BorderSide(
        color: theme.colorScheme.outline.withOpacity(0.3),
        width: 1,
      ),
    );
  }
}

/// Extension to provide common text field configurations
extension CustomTextFieldExtensions on CustomTextField {
  /// Creates an email text field
  static CustomTextField email({
    Key? key,
    String label = 'Email',
    String? hintText = 'Enter your email',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    String? Function(String?)? validator,
    bool isRequired = true,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      validator: validator,
      isRequired: isRequired,
      isEnabled: isEnabled,
      keyboardType: TextInputType.emailAddress,
      textCapitalization: TextCapitalization.none,
      prefixIcon: const Icon(Icons.email_outlined),
    );
  }

  /// Creates a password text field
  static CustomTextField password({
    Key? key,
    String label = 'Password',
    String? hintText = 'Enter your password',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    String? Function(String?)? validator,
    bool isRequired = true,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      validator: validator,
      isRequired: isRequired,
      isEnabled: isEnabled,
      isPassword: true,
      prefixIcon: const Icon(Icons.lock_outlined),
    );
  }

  /// Creates a phone number text field
  static CustomTextField phone({
    Key? key,
    String label = 'Phone Number',
    String? hintText = 'Enter your phone number',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    String? Function(String?)? validator,
    bool isRequired = true,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      validator: validator,
      isRequired: isRequired,
      isEnabled: isEnabled,
      keyboardType: TextInputType.phone,
      prefixIcon: const Icon(Icons.phone_outlined),
      inputFormatters: [
        FilteringTextInputFormatter.digitsOnly,
        LengthLimitingTextInputFormatter(15),
      ],
    );
  }

  /// Creates a search text field
  static CustomTextField search({
    Key? key,
    String label = 'Search',
    String? hintText = 'Search...',
    TextEditingController? controller,
    ValueChanged<String>? onChanged,
    ValueChanged<String>? onSubmitted,
    bool isEnabled = true,
  }) {
    return CustomTextField(
      key: key,
      label: label,
      hintText: hintText,
      controller: controller,
      onChanged: onChanged,
      onSubmitted: onSubmitted,
      isEnabled: isEnabled,
      keyboardType: TextInputType.text,
      prefixIcon: const Icon(Icons.search_outlined),
      suffixIcon: controller?.text.isNotEmpty == true
          ? IconButton(
              icon: const Icon(Icons.clear),
              onPressed: () => controller?.clear(),
            )
          : null,
    );
  }
}
```


## lib/presentation/views/widgets/specific/user_card.dart

**✅ What issue it solves/benefits:**
- Creates a reusable component specifically for displaying user information
- Maintains consistent user data presentation across different screens
- Encapsulates user-related UI logic and styling in one place
- Provides interactive elements (tap, favorite, etc.) for user cards
- Supports different display modes (list item, grid item, detailed card)

```dart
import 'package:flutter/material.dart';
import '../../../../data/models/user_model.dart';
import '../../../../shared/enums/user_role.dart';

/// Enum to define different user card display modes
enum UserCardType {
  listItem,    // Compact horizontal layout for lists
  gridItem,    // Square card for grid views
  detailed,    // Full detailed card with all information
  minimal,     // Minimal display with just essential info
}

/// Reusable user card widget that displays user information
/// in various formats based on the specified type
class UserCard extends StatefulWidget {
  // User data to display
  final UserModel user;
  
  // Type of card layout
  final UserCardType cardType;
  
  // Callback when card is tapped
  final VoidCallback? onTap;
  
  // Callback when favorite button is tapped
  final ValueChanged<bool>? onFavoriteToggle;
  
  // Callback when more options is tapped
  final VoidCallback? onMoreOptions;
  
  // Whether the user is currently favorited
  final bool isFavorite;
  
  // Whether to show favorite button
  final bool showFavorite;
  
  // Whether to show more options button
  final bool showMoreOptions;
  
  // Whether to show online status indicator
  final bool showOnlineStatus;
  
  // Custom background color
  final Color? backgroundColor;
  
  // Whether the card is in selection mode
  final bool isSelectionMode;
  
  // Whether this card is selected
  final bool isSelected;
  
  // Callback when selection changes
  final ValueChanged<bool>? onSelectionChanged;

  const UserCard({
    Key? key,
    required this.user,
    this.cardType = UserCardType.listItem,
    this.onTap,
    this.onFavoriteToggle,
    this.onMoreOptions,
    this.isFavorite = false,
    this.showFavorite = false,
    this.showMoreOptions = false,
    this.showOnlineStatus = true,
    this.backgroundColor,
    this.isSelectionMode = false,
    this.isSelected = false,
    this.onSelectionChanged,
  }) : super(key: key);

  @override
  State<UserCard> createState() => _UserCardState();
}

class _UserCardState extends State<UserCard> {
  // Track hover state for desktop interactions
  bool _isHovered = false;

  @override
  Widget build(BuildContext context) {
    switch (widget.cardType) {
      case UserCardType.listItem:
        return _buildListItem();
      case UserCardType.gridItem:
        return _buildGridItem();
      case UserCardType.detailed:
        return _buildDetailedCard();
      case UserCardType.minimal:
        return _buildMinimalCard();
    }
  }

  /// Builds list item layout (horizontal, compact)
  Widget _buildListItem() {
    final theme = Theme.of(context);
    
    return MouseRegion(
      onEnter: (_) => setState(() => _isHovered = true),
      onExit: (_) => setState(() => _isHovered = false),
      child: AnimatedContainer(
        duration: const Duration(milliseconds: 200),
        margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
        decoration: BoxDecoration(
          color: widget.backgroundColor ?? theme.colorScheme.surface,
          borderRadius: BorderRadius.circular(12),
          border: Border.all(
            color: widget.isSelected 
                ? theme.colorScheme.primary
                : theme.colorScheme.outline.withOpacity(0.2),
            width: widget.isSelected ? 2 : 1,
          ),
          boxShadow: _isHovered
              ? [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.1),
                    blurRadius: 8,
                    offset: const Offset(0, 2),
                  ),
                ]
              : null,
        ),
        child: Material(
          color: Colors.transparent,
          child: InkWell(
            onTap: widget.isSelectionMode 
                ? () => widget.onSelectionChanged?.call(!widget.isSelected)
                : widget.onTap,
            borderRadius: BorderRadius.circular(12),
            child: Padding(
              padding: const EdgeInsets.all(12),
              child: Row(
                children: [
                  // Selection checkbox (if in selection mode)
                  if (widget.isSelectionMode) ...[
                    Checkbox(
                      value: widget.isSelected,
                      onChanged: (value) => widget.onSelectionChanged?.call(value ?? false),
                    ),
                    const SizedBox(width: 8),
                  ],
                  
                  // User avatar with online status
                  Stack(
                    children: [
                      _buildAvatar(48),
                      if (widget.showOnlineStatus && widget.user.isOnline)
                        Positioned(
                          right: 2,
                          bottom: 2,
                          child: Container(
                            width: 14,
                            height: 14,
                            decoration: BoxDecoration(
                              color: Colors.green,
                              shape: BoxShape.circle,
                              border: Border.all(
                                color: theme.colorScheme.surface,
                                width: 2,
                              ),
                            ),
                          ),
                        ),
                    ],
                  ),
                  
                  const SizedBox(width: 12),
                  
                  // User information
                  Expanded(
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        // Name and role
                        Row(
                          children: [
                            Expanded(
                              child: Text(
                                widget.user.name,
                                style: theme.textTheme.titleMedium?.copyWith(
                                  fontWeight: FontWeight.w600,
                                ),
                                overflow: TextOverflow.ellipsis,
                              ),
                            ),
                            if (widget.user.role != UserRole.user)
                              _buildRoleBadge(),
                          ],
                        ),
                        
                        const SizedBox(height: 4),
                        
                        // Email
                        Text(
                          widget.user.email,
                          style: theme.textTheme.bodyMedium?.copyWith(
                            color: theme.colorScheme.onSurface.withOpacity(0.7),
                          ),
                          overflow: TextOverflow.ellipsis,
                        ),
                        
                        // Last seen (if offline)
                        if (!widget.user.isOnline && widget.user.lastSeen != null) ...[
                          const SizedBox(height: 2),
                          Text(
                            'Last seen ${_formatLastSeen(widget.user.lastSeen!)}',
                            style: theme.textTheme.bodySmall?.copyWith(
                              color: theme.colorScheme.onSurface.withOpacity(0.5),
                            ),
                          ),
                        ],
                      ],
                    ),
                  ),
                  
                  // Action buttons
                  if (!widget.isSelectionMode) ...[
                    // Favorite button
                    if (widget.showFavorite)
                      IconButton(
                        icon: Icon(
                          widget.isFavorite ? Icons.favorite : Icons.favorite_border,
                          color: widget.isFavorite ? Colors.red : null,
                        ),
                        onPressed: () => widget.onFavoriteToggle?.call(!widget.isFavorite),
                        tooltip: widget.isFavorite ? 'Remove from favorites' : 'Add to favorites',
                      ),
                    
                    // More options button
                    if (widget.showMoreOptions)
                      IconButton(
                        icon: const Icon(Icons.more_vert),
                        onPressed: widget.onMoreOptions,
                        tooltip: 'More options',
                      ),
                  ],
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  /// Builds grid item layout (square card)
  Widget _buildGridItem() {
    final theme = Theme.of(context);
    
    return MouseRegion(
      onEnter: (_) => setState(() => _isHovered = true),
      onExit: (_) => setState(() => _isHovered = false),
      child: AnimatedContainer(
        duration: const Duration(milliseconds: 200),
        decoration: BoxDecoration(
          color: widget.backgroundColor ?? theme.colorScheme.surface,
          borderRadius: BorderRadius.circular(16),
          border: Border.all(
            color: widget.isSelected 
                ? theme.colorScheme.primary
                : theme.colorScheme.outline.withOpacity(0.2),
            width: widget.isSelected ? 2 : 1,
          ),
          boxShadow: _isHovered
              ? [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.1),
                    blurRadius: 12,
                    offset: const Offset(0, 4),
                  ),
                ]
              : [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.05),
                    blurRadius: 4,
                    offset: const Offset(0, 2),
                  ),
                ],
        ),
        child: Material(
          color: Colors.transparent,
          child: InkWell(
            onTap: widget.isSelectionMode 
                ? () => widget.onSelectionChanged?.call(!widget.isSelected)
                : widget.onTap,
            borderRadius: BorderRadius.circular(16),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  // Selection checkbox (if in selection mode)
                  if (widget.isSelectionMode) ...[
                    Align(
                      alignment: Alignment.topRight,
                      child: Checkbox(
                        value: widget.isSelected,
                        onChanged: (value) => widget.onSelectionChanged?.call(value ?? false),
                      ),
                    ),
                  ] else ...[
                    // Action buttons row
                    if (widget.showFavorite || widget.showMoreOptions)
                      Row(
                        mainAxisAlignment: MainAxisAlignment.end,
                        children: [
                          if (widget.showFavorite)
                            IconButton(
                              icon: Icon(
                                widget.isFavorite ? Icons.favorite : Icons.favorite_border,
                                color: widget.isFavorite ? Colors.red : null,
                                size: 20,
                              ),
                              onPressed: () => widget.onFavoriteToggle?.call(!widget.isFavorite),
                              tooltip: widget.isFavorite ? 'Remove from favorites' : 'Add to favorites',
                            ),
                          if (widget.showMoreOptions)
                            IconButton(
                              icon: const Icon(Icons.more_vert, size: 20),
                              onPressed: widget.onMoreOptions,
                              tooltip: 'More options',
                            ),
                        ],
                      ),
                  ],
                  
                  // User avatar with online status
                  Stack(
                    children: [
                      _buildAvatar(64),
                      if (widget.showOnlineStatus && widget.user.isOnline)
                        Positioned(
                          right: 4,
                          bottom: 4,
                          child: Container(
                            width: 18,
                            height: 18,
                            decoration: BoxDecoration(
                              color: Colors.green,
                              shape: BoxShape.circle,
                              border: Border.all(
                                color: theme.colorScheme.surface,
                                width: 3,
                              ),
                            ),
                          ),
                        ),
                    ],
                  ),
                  
                  const SizedBox(height: 12),
                  
                  // User name
                  Text(
                    widget.user.name,
                    style: theme.textTheme.titleMedium?.copyWith(
                      fontWeight: FontWeight.w600,
                    ),
                    textAlign: TextAlign.center,
                    overflow: TextOverflow.ellipsis,
                    maxLines: 1,
                  ),
                  
                  const SizedBox(height: 4),
                  
                  // User email
                  Text(
                    widget.user.email,
                    style: theme.textTheme.bodySmall?.copyWith(
                      color: theme.colorScheme.onSurface.withOpacity(0.7),
                    ),
                    textAlign: TextAlign.center,
                    overflow: TextOverflow.ellipsis,
                    maxLines: 1,
                  ),
                  
                  // Role badge
                  if (widget.user.role != UserRole.user) ...[
                    const SizedBox(height: 8),
                    _buildRoleBadge(),
                  ],
                  
                  // Online status text
                  if (widget.showOnlineStatus) ...[
                    const SizedBox(height: 8),
                    Text(
                      widget.user.isOnline 
                          ? 'Online' 
                          : 'Last seen ${_formatLastSeen(widget.user.lastSeen ?? DateTime.now())}',
                      style: theme.textTheme.bodySmall?.copyWith(
                        color: widget.user.isOnline 
                            ? Colors.green 
                            : theme.colorScheme.onSurface.withOpacity(0.5),
                      ),
                      textAlign: TextAlign.center,
                      overflow: TextOverflow.ellipsis,
                      maxLines: 1,
                    ),
                  ],
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  /// Builds detailed card layout (full information)
  Widget _buildDetailedCard() {
    final theme = Theme.of(context);
    
    return MouseRegion(
      onEnter: (_) => setState(() => _isHovered = true),
      onExit: (_) => setState(() => _isHovered = false),
      child: AnimatedContainer(
        duration: const Duration(milliseconds: 200),
        margin: const EdgeInsets.all(8),
        decoration: BoxDecoration(
          color: widget.backgroundColor ?? theme.colorScheme.surface,
          borderRadius: BorderRadius.circular(16),
          border: Border.all(
            color: widget.isSelected 
                ? theme.colorScheme.primary
                : theme.colorScheme.outline.withOpacity(0.2),
            width: widget.isSelected ? 2 : 1,
          ),
          boxShadow: _isHovered
              ? [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.15),
                    blurRadius: 16,
                    offset: const Offset(0, 6),
                  ),
                ]
              : [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.08),
                    blurRadius: 8,
                    offset: const Offset(0, 2),
                  ),
                ],
        ),
        child: Material(
          color: Colors.transparent,
          child: InkWell(
            onTap: widget.isSelectionMode 
                ? () => widget.onSelectionChanged?.call(!widget.isSelected)
                : widget.onTap,
            borderRadius: BorderRadius.circular(16),
            child: Padding(
              padding: const EdgeInsets.all(20),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  // Header row with avatar and actions
                  Row(
                    children: [
                      // Selection checkbox (if in selection mode)
                      if (widget.isSelectionMode) ...[
                        Checkbox(
                          value: widget.isSelected,
                          onChanged: (value) => widget.onSelectionChanged?.call(value ?? false),
                        ),
                        const SizedBox(width: 12),
                      ],
                      
                      // User avatar with online status
                      Stack(
                        children: [
                          _buildAvatar(72),
                          if (widget.showOnlineStatus && widget.user.isOnline)
                            Positioned(
                              right: 4,
                              bottom: 4,
                              child: Container(
                                width: 20,
                                height: 20,
                                decoration: BoxDecoration(
                                  color: Colors.green,
                                  shape: BoxShape.circle,
                                  border: Border.all(
                                    color: theme.colorScheme.surface,
                                    width: 3,
                                  ),
                                ),
                              ),
                            ),
                        ],
                      ),
                      
                      const SizedBox(width: 16),
                      
                      // User basic info
                      Expanded(
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            // Name and role
                            Row(
                              children: [
                                Expanded(
                                  child: Text(
                                    widget.user.name,
                                    style: theme.textTheme.headlineSmall?.copyWith(
                                      fontWeight: FontWeight.w700,
                                    ),
                                    overflow: TextOverflow.ellipsis,
                                  ),
                                ),
                                if (widget.user.role != UserRole.user)
                                  _buildRoleBadge(),
                              ],
                            ),
                            
                            const SizedBox(height: 4),
                            
                            // Email
                            Text(
                              widget.user.email,
                              style: theme.textTheme.bodyLarge?.copyWith(
                                color: theme.colorScheme.onSurface.withOpacity(0.8),
                              ),
                              overflow: TextOverflow.ellipsis,
                            ),
                            
                            // Phone (if available)
                            if (widget.user.phone?.isNotEmpty == true) ...[
                              const SizedBox(height: 2),
                              Text(
                                widget.user.phone!,
                                style: theme.textTheme.bodyMedium?.copyWith(
                                  color: theme.colorScheme.onSurface.withOpacity(0.7),
                                ),
                              ),
                            ],
                          ],
                        ),
                      ),
                      
                      // Action buttons
                      if (!widget.isSelectionMode) ...[
                        Column(
                          children: [
                            if (widget.showFavorite)
                              IconButton(
                                icon: Icon(
                                  widget.isFavorite ? Icons.favorite : Icons.favorite_border,
                                  color: widget.isFavorite ? Colors.red : null,
                                ),
                                onPressed: () => widget.onFavoriteToggle?.call(!widget.isFavorite),
                                tooltip: widget.isFavorite ? 'Remove from favorites' : 'Add to favorites',
                              ),
                            if (widget.showMoreOptions)
                              IconButton(
                                icon: const Icon(Icons.more_vert),
                                onPressed: widget.onMoreOptions,
                                tooltip: 'More options',
                              ),
                          ],
                        ),
                      ],
                    ],
                  ),
                  
                  const SizedBox(height: 16),
                  
                  // Additional information
                  Row(
                    children: [
                      // Online status
                      if (widget.showOnlineStatus) ...[
                        Icon(
                          widget.user.isOnline ? Icons.circle : Icons.circle_outlined,
                          color: widget.user.isOnline ? Colors.green : Colors.grey,
                          size: 12,
                        ),
                        const SizedBox(width: 8),
                        Text(
                          widget.user.isOnline 
                              ? 'Online now' 
                              : 'Last seen ${_formatLastSeen(widget.user.lastSeen ?? DateTime.now())}',
                          style: theme.textTheme.bodySmall?.copyWith(
                            color: theme.colorScheme.onSurface.withOpacity(0.7),
                          ),
                        ),
                        const Spacer(),
                      ],
                      
                      // Join date
                      Text(
                        'Joined ${_formatJoinDate(widget.user.createdAt)}',
                        style: theme.textTheme.bodySmall?.copyWith(
                          color: theme.colorScheme.onSurface.withOpacity(0.6),
                        ),
                      ),
                    ],
                  ),
                  
                  // Bio or description (if available)
                  if (widget.user.bio?.isNotEmpty == true) ...[
                    const SizedBox(height: 12),
                    Text(
                      widget.user.bio!,
                      style: theme.textTheme.bodyMedium?.copyWith(
                        color: theme.colorScheme.onSurface.withOpacity(0.8),
                      ),
                      maxLines: 3,
                      overflow: TextOverflow.ellipsis,
                    ),
                  ],
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  /// Builds minimal card layout (just avatar and name)
  Widget _buildMinimalCard() {
    final theme = Theme.of(context);
    
    return MouseRegion(
      onEnter: (_) => setState(() => _isHovered = true),
      onExit: (_) => setState(() => _isHovered = false),
      child: AnimatedContainer(
        duration: const Duration(milliseconds: 200),
        padding: const EdgeInsets.all(8),
        decoration: BoxDecoration(
          color: widget.backgroundColor ?? Colors.transparent,
          borderRadius: BorderRadius.circular(8),
          border: widget.isSelected 
              ? Border.all(color: theme.colorScheme.primary, width: 2)
              : null,
        ),
        child: Material(
          color: Colors.transparent,
          child: InkWell(
            onTap: widget.isSelectionMode 
                ? () => widget.onSelectionChanged?.call(!widget.isSelected)
                : widget.onTap,
            borderRadius: BorderRadius.circular(8),
            child: Padding(
              padding: const EdgeInsets.all(8),
              child: Row(
                mainAxisSize: MainAxisSize.min,
                children: [
                  // Selection checkbox (if in selection mode)
                  if (widget.isSelectionMode) ...[
                    Checkbox(
                      value: widget.isSelected,
                      onChanged: (value) => widget.onSelectionChanged?.call(value ?? false),
                    ),
                    const SizedBox(width: 4),
                  ],
                  
                  // User avatar
                  Stack(
                    children: [
                      _buildAvatar(32),
                      if (widget.showOnlineStatus && widget.user.isOnline)
                        Positioned(
                          right: 0,
                          bottom: 0,
                          child: Container(
                            width: 10,
                            height: 10,
                            decoration: BoxDecoration(
                              color: Colors.green,
                              shape: BoxShape.circle,
                              border: Border.all(
                                color: theme.colorScheme.surface,
                                width: 2,
                              ),
                            ),
                          ),
                        ),
                    ],
                  ),
                  
                  const SizedBox(width: 8),
                  
                  // User name
                  Flexible(
                    child: Text(
                      widget.user.name,
                      style: theme.textTheme.bodyMedium?.copyWith(
                        fontWeight: FontWeight.w500,
                      ),
                      overflow: TextOverflow.ellipsis,
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  /// Builds user avatar with fallback initials
  Widget _buildAvatar(double size) {
    if (widget.user.avatarUrl?.isNotEmpty == true) {
      return CircleAvatar(
        radius: size / 2,
        backgroundImage: NetworkImage(widget.user.avatarUrl!),
        onBackgroundImageError: (_, __) {
          // Fallback to initials if image fails to load
        },
        child: widget.user.avatarUrl!.isEmpty ? _buildInitials(size) : null,
      );
    }
    
    return CircleAvatar(
      radius: size / 2,
      backgroundColor: _getAvatarColor(),
      child: _buildInitials(size),
    );
  }

  /// Builds initials text for avatar
  Widget _buildInitials(double size) {
    final initials = widget.user.name
        .split(' ')
        .take(2)
        .map((word) => word.isNotEmpty ? word[0].toUpperCase() : '')
        .join();
    
    return Text(
      initials,
      style: TextStyle(
        color: Colors.white,
        fontSize: size * 0.4,
        fontWeight: FontWeight.w600,
      ),
    );
  }

  /// Gets avatar background color based on user name
  Color _getAvatarColor() {
    final colors = [
      Colors.blue,
      Colors.green,
      Colors.orange,
      Colors.purple,
      Colors.red,
      Colors.teal,
      Colors.indigo,
      Colors.pink,
    ];
    
    final index = widget.user.name.hashCode % colors.length;
    return colors[index.abs()];
  }

  /// Builds role badge
  Widget _buildRoleBadge() {
    final theme = Theme.of(context);
    
    Color badgeColor;
    String roleText;
    
    switch (widget.user.role) {
      case UserRole.admin:
        badgeColor = Colors.red;
        roleText = 'Admin';
        break;
      case UserRole.moderator:
        badgeColor = Colors.orange;
        roleText = 'Mod';
        break;
      case UserRole.premium:
        badgeColor = Colors.purple;
        roleText = 'Premium';
        break;
      case UserRole.user:
      default:
        return const SizedBox.shrink();
    }
    
    return Container(
      padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 2),
      decoration: BoxDecoration(
        color: badgeColor.withOpacity(0.1),
        borderRadius: BorderRadius.circular(12),
        border: Border.all(color: badgeColor.withOpacity(0.3)),
      ),
      child: Text(
        roleText,
        style: theme.textTheme.bodySmall?.copyWith(
          color: badgeColor,
          fontWeight: FontWeight.w600,
        ),
      ),
    );
  }

  /// Formats last seen date
  String _formatLastSeen(DateTime lastSeen) {
    final now = DateTime.now();
    final difference = now.difference(lastSeen);
    
    if (difference.inMinutes < 1) {
      return 'just now';
    } else if (difference.inMinutes < 60) {
      return '${difference.inMinutes}m ago';
    } else if (difference.inHours < 24) {
      return '${difference.inHours}h ago';
    } else if (difference.inDays < 7) {
      return '${difference.inDays}d ago';
    } else {
      return '${lastSeen.day}/${lastSeen.month}/${lastSeen.year}';
    }
  }

  /// Formats join date
  String _formatJoinDate(DateTime joinDate) {
    final months = [
      'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
      'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
    ];
    
    return '${months[joinDate.month - 1]} ${joinDate.year}';
  }
}
```

## lib/presentation/views/widgets/specific/product_card.dart

**✅ What issue it solves/benefits:**
- Creates a specialized component for displaying product information consistently
- Handles various product display modes (grid, list, featured) with appropriate layouts
- Encapsulates product-specific UI logic like pricing, ratings, and stock status
- Provides interactive elements (add to cart, favorite, share) for e-commerce functionality
- Maintains consistent product presentation across different screens

```dart
import 'package:flutter/material.dart';
import '../../../../data/models/product_model.dart';

/// Enum to define different product card display modes
enum ProductCardType {
  grid,        // Square card for grid views
  list,        // Horizontal layout for list views
  featured,    // Large featured card with detailed info
  compact,     // Minimal card for suggestions
}

/// Reusable product card widget that displays product information
/// in various formats based on the specified type
class ProductCard extends StatefulWidget {
  // Product data to display
  final ProductModel product;
  
  // Type of card layout
  final ProductCardType cardType;
  
  // Callback when card is tapped
  final VoidCallback? onTap;
  
  // Callback when add to cart is tapped
  final VoidCallback? onAddToCart;
  
  // Callback when favorite button is tapped
  final ValueChanged<bool>? onFavoriteToggle;
  
  // Callback when share button is tapped
  final VoidCallback? onShare;
  
  // Whether the product is currently favorited
  final bool isFavorite;
  
  // Whether to show add to cart button
  final bool showAddToCart;
  
  // Whether to show favorite button
  final bool showFavorite;
  
  // Whether to show share button
  final bool showShare;
  
  // Whether to show discount badge
  final bool showDiscount;
  
  // Custom background color
  final Color? backgroundColor;
  
  // Whether the card is in selection mode
  final bool isSelectionMode;
  
  // Whether this card is selected
  final bool isSelected;
  
  // Callback when selection changes
  final ValueChanged<bool>? onSelectionChanged;

  const ProductCard({
    Key? key,
    required this.product,
    this.cardType = ProductCardType.grid,
    this.onTap,
    this.onAddToCart,
    this.onFavoriteToggle,
    this.onShare,
    this.isFavorite = false,
    this.showAddToCart = true,
    this.showFavorite = true,
    this.showShare = false,
    this.showDiscount = true,
    this.backgroundColor,
    this.isSelectionMode = false,
    this.isSelected = false,
    this.onSelectionChanged,
  }) : super(key: key);

  @override
  State<ProductCard> createState() => _ProductCardState();
}

class _ProductCardState extends State<ProductCard> {
  // Track hover state for desktop interactions
  bool _isHovered = false;

  @override
  Widget build(BuildContext context) {
    switch (widget.cardType) {
      case ProductCardType.grid:
        return _buildGridCard();
      case ProductCardType.list:
        return _buildListCard();
      case ProductCardType.featured:
        return _buildFeaturedCard();
      case ProductCardType.compact:
        return _buildCompactCard();
    }
  }

  /// Builds grid card layout (square card for grid views)
  Widget _buildGridCard() {
    final theme = Theme.of(context);
    
    return MouseRegion(
      onEnter: (_) => setState(() => _isHovered = true),
      onExit: (_) => setState(() => _isHovered = false),
      child: AnimatedContainer(
        duration: const Duration(milliseconds: 200),
        decoration: BoxDecoration(
          color: widget.backgroundColor ?? theme.colorScheme.surface,
          borderRadius: BorderRadius.circular(16),
          border: Border.all(
            color: widget.isSelected 
                ? theme.colorScheme.primary
                : theme.colorScheme.outline.withOpacity(0.2),
            width: widget.isSelected ? 2 : 1,
          ),
          boxShadow: _isHovered
              ? [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.1),
                    blurRadius: 12,
                    offset: const Offset(0, 4),
                  ),
                ]
              : [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.05),
                    blurRadius: 4,
                    offset: const Offset(0, 2),
                  ),
                ],
        ),
        child: Material(
          color: Colors.transparent,
          child: InkWell(
            onTap: widget.isSelectionMode 
                ? () => widget.onSelectionChanged?.call(!widget.isSelected)
                : widget.onTap,
            borderRadius: BorderRadius.circular(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                // Product image with badges
                Expanded(
                  flex: 3,
                  child: _buildProductImage(),
                ),
                
                // Product information
                Expanded(
                  flex: 2,
                  child: Padding(
                    padding: const EdgeInsets.all(12),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        // Product name
                        Text(
                          widget.product.name,
                          style: theme.textTheme.titleSmall?.copyWith(
                            fontWeight: FontWeight.w600,
                          ),
                          maxLines: 2,
                          overflow: TextOverflow.ellipsis,
                        ),
                        
                        const SizedBox(height: 4),
                        
                        // Price and rating
                        Row(
                          children: [
                            Expanded(child: _buildPriceSection()),
                            _buildRatingSection(),
                          ],
                        ),
                        
                        const Spacer(),
                        
                        // Action buttons
                        if (!widget.isSelectionMode)
                          _buildActionButtons(),
                      ],
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  /// Builds list card layout (horizontal layout)
  Widget _buildListCard() {
    final theme = Theme.of(context);
    
    return MouseRegion(
      onEnter: (_) => setState(() => _isHovered = true),
      onExit: (_) => setState(() => _isHovered = false),
      child: AnimatedContainer(
        duration: const Duration(milliseconds: 200),
        margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 4),
        height: 120,
        decoration: BoxDecoration(
          color: widget.backgroundColor ?? theme.colorScheme.surface,
          borderRadius: BorderRadius.circular(12),
          border: Border.all(
            color: widget.isSelected 
                ? theme.colorScheme.primary
                : theme.colorScheme.outline.withOpacity(0.2),
            width: widget.isSelected ? 2 : 1,
          ),
          boxShadow

## lib/presentation/views/widgets/common/loading_widget.dart

**✅ What issue it solves/benefits:**
- Provides consistent loading indicators across the entire application
- Centralizes loading state management and prevents UI blocking
- Offers different loading styles for various use cases (overlay, inline, skeleton)
- Improves user experience with smooth loading transitions
- Reduces code duplication for loading states

```dart
import 'package:flutter/material.dart';

/// Enum to define different types of loading indicators
enum LoadingType {
  circular,    // Standard circular progress indicator
  linear,      // Linear progress bar
  dots,        // Animated dots
  skeleton,    // Skeleton loading placeholder
  overlay,     // Full screen overlay loading
}

/// Custom loading widget that provides consistent loading indicators
/// throughout the application with various display options
class LoadingWidget extends StatefulWidget {
  // Type of loading indicator to display
  final LoadingType type;
  
  // Custom message to show with loading indicator
  final String? message;
  
  // Size of the loading indicator
  final double size;
  
  // Color of the loading indicator
  final Color? color;
  
  // Whether to show the loading message
  final bool showMessage;
  
  // Background color for overlay type
  final Color? backgroundColor;
  
  // Whether the overlay is dismissible
  final bool isDismissible;
  
  // Custom widget to show instead of default message
  final Widget? customMessage;
  
  // Height for skeleton loading
  final double? skeletonHeight;
  
  // Width for skeleton loading
  final double? skeletonWidth;

  const LoadingWidget({
    Key? key,
    this.type = LoadingType.circular,
    this.message,
    this.size = 40.0,
    this.color,
    this.showMessage = true,
    this.backgroundColor,
    this.isDismissible = false,
    this.customMessage,
    this.skeletonHeight,
    this.skeletonWidth,
  }) : super(key: key);

  @override
  State<LoadingWidget> createState() => _LoadingWidgetState();
}

class _LoadingWidgetState extends State<LoadingWidget>
    with TickerProviderStateMixin {
  // Animation controller for custom animations
  late AnimationController _animationController;
  
  // Animation for dot loading
  late Animation<double> _dotAnimation;

  @override
  void initState() {
    super.initState();
    
    // Initialize animation controller
    _animationController = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    );
    
    // Setup dot animation
    _dotAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeInOut,
    ));
    
    // Start animation for dot type
    if (widget.type == LoadingType.dots) {
      _animationController.repeat();
    }
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    switch (widget.type) {
      case LoadingType.circular:
        return _buildCircularLoading();
      case LoadingType.linear:
        return _buildLinearLoading();
      case LoadingType.dots:
        return _buildDotsLoading();
      case LoadingType.skeleton:
        return _buildSkeletonLoading();
      case LoadingType.overlay:
        return _buildOverlayLoading();
    }
  }

  /// Builds circular progress indicator with optional message
  Widget _buildCircularLoading() {
    final theme = Theme.of(context);
    
    return Center(
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          // Circular progress indicator
          SizedBox(
            width: widget.size,
            height: widget.size,
            child: CircularProgressIndicator(
              strokeWidth: 3,
              valueColor: AlwaysStoppedAnimation<Color>(
                widget.color ?? theme.colorScheme.primary,
              ),
            ),
          ),
          
          // Optional message
          if (widget.showMessage && widget.message != null) ...[
            const SizedBox(height: 16),
            widget.customMessage ?? _buildMessage(theme),
          ],
        ],
      ),
    );
  }

  /// Builds linear progress indicator
  Widget _buildLinearLoading() {
    final theme = Theme.of(context);
    
    return Column(
      mainAxisSize: MainAxisSize.min,
      children: [
        // Linear progress indicator
        LinearProgressIndicator(
          valueColor: AlwaysStoppedAnimation<Color>(
            widget.color ?? theme.colorScheme.primary,
          ),
          backgroundColor: theme.colorScheme.surfaceVariant,
        ),
        
        // Optional message
        if (widget.showMessage && widget.message != null) ...[
          const SizedBox(height: 12),
          widget.customMessage ?? _buildMessage(theme),
        ],
      ],
    );
  }

  /// Builds animated dots loading indicator
  Widget _buildDotsLoading() {
    final theme = Theme.of(context);
    
    return Center(
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          // Animated dots
          AnimatedBuilder(
            animation: _dotAnimation,
            builder: (context, child) {
              return Row(
                mainAxisSize: MainAxisSize.min,
                children: List.generate(3, (index) {
                  // Calculate delay for each dot
                  final delay = index * 0.2;
                  final animationValue = (_dotAnimation.value - delay).clamp(0.0, 1.0);
                  
                  return Container(
                    margin: const EdgeInsets.symmetric(horizontal: 4),
                    child: Transform.scale(
                      scale: 0.8 + (0.4 * Curves.elasticOut.transform(animationValue)),
                      child: Container(
                        width: 8,
                        height: 8,
                        decoration: BoxDecoration(
                          color: (widget.color ?? theme.colorScheme.primary)
                              .withOpacity(0.3 + (0.7 * animationValue)),
                          shape: BoxShape.circle,
                        ),
                      ),
                    ),
                  );
                }),
              );
            },
          ),
          
          // Optional message
          if (widget.showMessage && widget.message != null) ...[
            const SizedBox(height: 16),
            widget.customMessage ?? _buildMessage(theme),
          ],
        ],
      ),
    );
  }

  /// Builds skeleton loading placeholder
  Widget _buildSkeletonLoading() {
    return _SkeletonAnimation(
      child: Container(
        width: widget.skeletonWidth ?? double.infinity,
        height: widget.skeletonHeight ?? 16,
        decoration: BoxDecoration(
          color: Colors.grey[300],
          borderRadius: BorderRadius.circular(4),
        ),
      ),
    );
  }

  /// Builds full screen overlay loading
  Widget _buildOverlayLoading() {
    final theme = Theme.of(context);
    
    return Material(
      color: widget.backgroundColor ?? 
             theme.colorScheme.surface.withOpacity(0.8),
      child: InkWell(
        // Prevent dismissal if not dismissible
        onTap: widget.isDismissible ? () => Navigator.of(context).pop() : null,
        child: Container(
          width: double.infinity,
          height: double.infinity,
          child: Center(
            child: Container(
              padding: const EdgeInsets.all(24),
              decoration: BoxDecoration(
                color: theme.colorScheme.surface,
                borderRadius: BorderRadius.circular(12),
                boxShadow: [
                  BoxShadow(
                    color: Colors.black.withOpacity(0.1),
                    blurRadius: 10,
                    offset: const Offset(0, 4),
                  ),
                ],
              ),
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: [
                  // Loading indicator
                  SizedBox(
                    width: widget.size,
                    height: widget.size,
                    child: CircularProgressIndicator(
                      strokeWidth: 3,
                      valueColor: AlwaysStoppedAnimation<Color>(
                        widget.color ?? theme.colorScheme.primary,
                      ),
                    ),
                  ),
                  
                  // Message
                  if (widget.showMessage && widget.message != null) ...[
                    const SizedBox(height: 16),
                    widget.customMessage ?? _buildMessage(theme),
                  ],
                  
                  // Dismissible hint
                  if (widget.isDismissible) ...[
                    const SizedBox(height: 8),
                    Text(
                      'Tap anywhere to dismiss',
                      style: theme.textTheme.bodySmall?.copyWith(
                        color: theme.colorScheme.onSurface.withOpacity(0.6),
                      ),
                    ),
                  ],
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  /// Builds the loading message text
  Widget _buildMessage(ThemeData theme) {
    return Text(
      widget.message!,
      style: theme.textTheme.bodyMedium?.copyWith(
        color: theme.colorScheme.onSurface.withOpacity(0.7),
      ),
      textAlign: TextAlign.center,
    );
  }
}

/// Skeleton animation widget for shimmer effect
class _SkeletonAnimation extends StatefulWidget {
  final Widget child;

  const _SkeletonAnimation({required this.child});

  @override
  State<_SkeletonAnimation> createState() => _SkeletonAnimationState();
}

class _SkeletonAnimationState extends State<_SkeletonAnimation>
    with SingleTickerProviderStateMixin {
  late AnimationController _shimmerController;

  @override
  void initState() {
    super.initState();
    _shimmerController = AnimationController(
      duration: const Duration(milliseconds: 1500),
      vsync: this,
    )..repeat();
  }

  @override
  void dispose() {
    _shimmerController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _shimmerController,
      builder: (context, child) {
        return ShaderMask(
          blendMode: BlendMode.srcATop,
          shaderCallback: (bounds) {
            return LinearGradient(
              colors: const [
                Color(0xFFEBEBF4),
                Color(0xFFF4F4F4),
                Color(0xFFEBEBF4),
              ],
              stops: const [0.1, 0.3, 0.4],
              begin: const Alignment(-1.0, -0.3),
              end: const Alignment(1.0, 0.3),
              tileMode: TileMode.clamp,
              transform: _SlidingGradientTransform(
                slidePercent: _shimmerController.value,
              ),
            ).createShader(bounds);
          },
          child: widget.child,
        );
      },
    );
  }
}

/// Transform for sliding gradient effect in skeleton loading
class _SlidingGradientTransform extends GradientTransform {
  final double slidePercent;

  _SlidingGradientTransform({required this.slidePercent});

  @override
  Matrix4? transform(Rect bounds, {TextDirection? textDirection}) {
    return Matrix4.translationValues(bounds.width * slidePercent, 0.0, 0.0);
  }
}

/// Utility class for common loading scenarios
class LoadingUtils {
  /// Shows overlay loading dialog
  static void showOverlayLoading(
    BuildContext context, {
    String? message = 'Loading...',
    bool isDismissible = false,
  }) {
    showDialog(
      context: context,
      barrierDismissible: isDismissible,
      builder: (context) => LoadingWidget(
        type: LoadingType.overlay,
        message: message,
        isDismissible: isDismissible,
      ),
    );
  }

  /// Hides overlay loading dialog
  static void hideOverlayLoading(BuildContext context) {
    if (Navigator.of(context).canPop()) {
      Navigator.of(context).pop();
    }
  }

  /// Creates a list of skeleton items for list loading
  static List<Widget> buildSkeletonList({
    int itemCount = 5,
    double itemHeight = 80,
    EdgeInsets? padding,
  }) {
    return List.generate(itemCount, (index) {
      return Container(
        padding: padding ?? const EdgeInsets.all(16),
        child: Column(
          children: [
            LoadingWidget(
              type: LoadingType.skeleton,
              skeletonHeight: itemHeight * 0.3,
            ),
            const SizedBox(height: 8),
            LoadingWidget(
              type: LoadingType.skeleton,
              skeletonHeight: itemHeight * 0.2,
              skeletonWidth: 200,
            ),
            const SizedBox(height: 4),
            LoadingWidget(
              type: LoadingType.skeleton,
              skeletonHeight: itemHeight * 0.2,
              skeletonWidth: 150,
            ),
          ],
        ),
      );
    });
  }
}
```
## presentation/themes/app_theme.dart

**What issue it solves/Benefits of separation:**
- ✅ Centralizes theme management and provides a single point of control for app-wide theming
- ✅ Enables easy switching between light/dark themes with consistent styling
- ✅ Maintains design system consistency across the entire application
- ✅ Simplifies theme customization and reduces code duplication
- ✅ Provides type-safe access to theme properties throughout the app

```dart
// Main theme configuration that orchestrates light and dark themes
// This file acts as the central hub for all theme-related configurations
import 'package:flutter/material.dart';
import 'light_theme.dart';
import 'dark_theme.dart';
import '../../core/constants/theme_constants.dart';

/// AppTheme class manages the overall theme configuration for the application
/// It provides static methods to get light and dark themes, and utility methods
/// for accessing theme-specific properties
class AppTheme {
  // Private constructor to prevent instantiation
  // This class is designed to be used as a static utility class
  AppTheme._();

  /// Returns the light theme configuration
  /// This method creates and returns a complete ThemeData object for light mode
  static ThemeData get lightTheme {
    return LightTheme.theme;
  }

  /// Returns the dark theme configuration
  /// This method creates and returns a complete ThemeData object for dark mode
  static ThemeData get darkTheme {
    return DarkTheme.theme;
  }

  /// Gets the current theme mode based on system settings or user preference
  /// This can be extended to include user preference from shared preferences
  static ThemeMode get themeMode {
    // For now, we follow system theme, but this can be customized
    // to read from user preferences stored in local storage
    return ThemeMode.system;
  }

  /// Utility method to get text theme from current context
  /// This provides easy access to text styles throughout the app
  static TextTheme getTextTheme(BuildContext context) {
    return Theme.of(context).textTheme;
  }

  /// Utility method to get color scheme from current context
  /// This provides easy access to color palette throughout the app
  static ColorScheme getColorScheme(BuildContext context) {
    return Theme.of(context).colorScheme;
  }

  /// Checks if the current theme is dark mode
  /// Useful for conditional rendering based on theme
  static bool isDarkMode(BuildContext context) {
    return Theme.of(context).brightness == Brightness.dark;
  }

  /// Gets the primary color from current theme
  /// Provides quick access to the main brand color
  static Color getPrimaryColor(BuildContext context) {
    return Theme.of(context).primaryColor;
  }

  /// Gets the background color from current theme
  /// Useful for setting consistent background colors
  static Color getBackgroundColor(BuildContext context) {
    return Theme.of(context).scaffoldBackgroundColor;
  }

  /// Gets the card color from current theme
  /// Ensures consistent card styling across the app
  static Color getCardColor(BuildContext context) {
    return Theme.of(context).cardColor;
  }

  /// Gets the error color from current theme
  /// Provides consistent error state styling
  static Color getErrorColor(BuildContext context) {
    return Theme.of(context).colorScheme.error;
  }

  /// Gets the success color from theme constants
  /// Since Flutter doesn't have a built-in success color, we use our custom one
  static Color getSuccessColor(BuildContext context) {
    return isDarkMode(context) 
        ? ThemeConstants.successColorDark 
        : ThemeConstants.successColorLight;
  }

  /// Gets the warning color from theme constants
  /// Since Flutter doesn't have a built-in warning color, we use our custom one
  static Color getWarningColor(BuildContext context) {
    return isDarkMode(context) 
        ? ThemeConstants.warningColorDark 
        : ThemeConstants.warningColorLight;
  }

  /// Creates a custom TextStyle with theme-aware colors
  /// This method helps create consistent text styles throughout the app
  static TextStyle createTextStyle({
    required BuildContext context,
    double? fontSize,
    FontWeight? fontWeight,
    Color? color,
    double? letterSpacing,
    double? height,
  }) {
    return TextStyle(
      fontSize: fontSize ?? 14.0,
      fontWeight: fontWeight ?? FontWeight.normal,
      color: color ?? getTextColor(context),
      letterSpacing: letterSpacing,
      height: height,
      fontFamily: ThemeConstants.fontFamily,
    );
  }

  /// Gets the appropriate text color based on current theme
  /// Ensures text is readable in both light and dark modes
  static Color getTextColor(BuildContext context) {
    return Theme.of(context).textTheme.bodyLarge?.color ?? 
           (isDarkMode(context) ? Colors.white : Colors.black);
  }

  /// Gets the appropriate secondary text color based on current theme
  /// Used for less prominent text elements
  static Color getSecondaryTextColor(BuildContext context) {
    return Theme.of(context).textTheme.bodyMedium?.color ?? 
           (isDarkMode(context) ? Colors.white70 : Colors.black54);
  }

  /// Creates a BoxDecoration with theme-aware styling
  /// Useful for creating consistent container decorations
  static BoxDecoration createBoxDecoration({
    required BuildContext context,
    Color? color,
    double borderRadius = 8.0,
    Color? borderColor,
    double borderWidth = 1.0,
    List<BoxShadow>? boxShadow,
  }) {
    return BoxDecoration(
      color: color ?? getCardColor(context),
      borderRadius: BorderRadius.circular(borderRadius),
      border: borderColor != null 
          ? Border.all(color: borderColor, width: borderWidth)
          : null,
      boxShadow: boxShadow ?? [
        BoxShadow(
          color: isDarkMode(context) 
              ? Colors.black26 
              : Colors.grey.withOpacity(0.1),
          blurRadius: 4.0,
          offset: const Offset(0, 2),
        ),
      ],
    );
  }

  /// Creates an InputDecoration with consistent theme styling
  /// Ensures all text fields have the same look and feel
  static InputDecoration createInputDecoration({
    required BuildContext context,
    String? labelText,
    String? hintText,
    IconData? prefixIcon,
    IconData? suffixIcon,
    VoidCallback? onSuffixIconPressed,
  }) {
    return InputDecoration(
      labelText: labelText,
      hintText: hintText,
      prefixIcon: prefixIcon != null ? Icon(prefixIcon) : null,
      suffixIcon: suffixIcon != null 
          ? IconButton(
              icon: Icon(suffixIcon),
              onPressed: onSuffixIconPressed,
            )
          : null,
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
      ),
      enabledBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: isDarkMode(context) 
              ? Colors.grey.shade600 
              : Colors.grey.shade300,
        ),
      ),
      focusedBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: getPrimaryColor(context),
          width: 2.0,
        ),
      ),
      errorBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: getErrorColor(context),
        ),
      ),
      focusedErrorBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: getErrorColor(context),
          width: 2.0,
        ),
      ),
      contentPadding: const EdgeInsets.symmetric(
        horizontal: 16.0,
        vertical: 12.0,
      ),
    );
  }

  /// Creates a ButtonStyle with consistent theme styling
  /// Ensures all buttons have the same look and feel
  static ButtonStyle createButtonStyle({
    required BuildContext context,
    Color? backgroundColor,
    Color? foregroundColor,
    double? elevation,
    EdgeInsetsGeometry? padding,
    Size? minimumSize,
  }) {
    return ElevatedButton.styleFrom(
      backgroundColor: backgroundColor ?? getPrimaryColor(context),
      foregroundColor: foregroundColor ?? Colors.white,
      elevation: elevation ?? 2.0,
      padding: padding ?? const EdgeInsets.symmetric(
        horizontal: 24.0,
        vertical: 12.0,
      ),
      minimumSize: minimumSize ?? const Size(100, 44),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
      ),
    );
  }

  /// Creates an AppBar theme with consistent styling
  /// Ensures all app bars have the same look and feel
  static AppBarTheme createAppBarTheme(BuildContext context) {
    return AppBarTheme(
      backgroundColor: getPrimaryColor(context),
      foregroundColor: Colors.white,
      elevation: 4.0,
      centerTitle: true,
      titleTextStyle: createTextStyle(
        context: context,
        fontSize: 20.0,
        fontWeight: FontWeight.w600,
        color: Colors.white,
      ),
      iconTheme: const IconThemeData(
        color: Colors.white,
        size: 24.0,
      ),
    );
  }

  /// Creates a Card theme with consistent styling
  /// Ensures all cards have the same look and feel
  static CardTheme createCardTheme(BuildContext context) {
    return CardTheme(
      color: getCardColor(context),
      elevation: 2.0,
      margin: const EdgeInsets.all(8.0),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
      ),
    );
  }
}
```

## presentation/themes/light_theme.dart

**What issue it solves**: Centralizes light theme configuration, ensuring consistent UI appearance across the entire application. Separates theme logic from business logic and provides easy maintenance of visual design system.

**Benefits**: 
- Consistent color scheme and typography throughout the app
- Easy to modify theme properties in one place
- Supports theme switching functionality
- Better maintainability and design system management

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import '../../core/constants/theme_constants.dart';

/// Light theme configuration for the application
/// This class defines all visual elements for light mode including
/// colors, typography, component themes, and system UI styling
class LightTheme {
  
  /// Primary light theme data configuration
  /// Contains all theme specifications for light mode
  static ThemeData get theme {
    return ThemeData(
      // Basic theme properties
      brightness: Brightness.light, // Sets overall brightness for the theme
      useMaterial3: true, // Enables Material 3 design system
      
      // Color scheme configuration
      colorScheme: _lightColorScheme,
      
      // Primary color configuration
      primarySwatch: Colors.blue, // Material design primary swatch
      primaryColor: ThemeConstants.primaryColor, // Custom primary color
      
      // Background and surface colors
      scaffoldBackgroundColor: ThemeConstants.lightBackgroundColor,
      backgroundColor: ThemeConstants.lightBackgroundColor, // Deprecated but still useful
      canvasColor: ThemeConstants.lightSurfaceColor,
      
      // Typography theme configuration
      textTheme: _lightTextTheme,
      
      // AppBar theme customization
      appBarTheme: _lightAppBarTheme,
      
      // Bottom navigation bar theme
      bottomNavigationBarTheme: _lightBottomNavTheme,
      
      // Button themes
      elevatedButtonTheme: _lightElevatedButtonTheme,
      textButtonTheme: _lightTextButtonTheme,
      outlinedButtonTheme: _lightOutlinedButtonTheme,
      
      // Input decoration theme for text fields
      inputDecorationTheme: _lightInputDecorationTheme,
      
      // Card theme configuration
      cardTheme: _lightCardTheme,
      
      // Floating action button theme
      floatingActionButtonTheme: _lightFABTheme,
      
      // Icon theme configuration
      iconTheme: _lightIconTheme,
      
      // Divider theme
      dividerTheme: _lightDividerTheme,
      
      // System UI overlay style for status bar
      appBarTheme: _lightAppBarTheme.copyWith(
        systemOverlayStyle: SystemUiOverlayStyle.dark, // Dark icons on light status bar
      ),
    );
  }

  /// Light color scheme definition
  /// Defines all colors used in Material 3 design system
  static const ColorScheme _lightColorScheme = ColorScheme.light(
    primary: ThemeConstants.primaryColor, // Main brand color
    onPrimary: Colors.white, // Text/icons on primary color
    secondary: ThemeConstants.secondaryColor, // Secondary brand color
    onSecondary: Colors.white, // Text/icons on secondary color
    error: ThemeConstants.errorColor, // Error state color
    onError: Colors.white, // Text/icons on error color
    background: ThemeConstants.lightBackgroundColor, // Main background
    onBackground: ThemeConstants.lightTextColor, // Text on background
    surface: ThemeConstants.lightSurfaceColor, // Surface color (cards, sheets)
    onSurface: ThemeConstants.lightTextColor, // Text on surface
    surfaceVariant: Color(0xFFF5F5F5), // Variant surface color
    onSurfaceVariant: Color(0xFF757575), // Text on variant surface
  );

  /// Text theme configuration for light mode
  /// Defines typography hierarchy and text styles
  static const TextTheme _lightTextTheme = TextTheme(
    // Display text styles (largest)
    displayLarge: TextStyle(
      fontSize: 32,
      fontWeight: FontWeight.bold,
      color: ThemeConstants.lightTextColor,
      letterSpacing: -0.5,
    ),
    displayMedium: TextStyle(
      fontSize: 28,
      fontWeight: FontWeight.bold,
      color: ThemeConstants.lightTextColor,
      letterSpacing: -0.25,
    ),
    
    // Headline text styles
    headlineLarge: TextStyle(
      fontSize: 24,
      fontWeight: FontWeight.w600,
      color: ThemeConstants.lightTextColor,
    ),
    headlineMedium: TextStyle(
      fontSize: 20,
      fontWeight: FontWeight.w600,
      color: ThemeConstants.lightTextColor,
    ),
    
    // Title text styles
    titleLarge: TextStyle(
      fontSize: 18,
      fontWeight: FontWeight.w500,
      color: ThemeConstants.lightTextColor,
    ),
    titleMedium: TextStyle(
      fontSize: 16,
      fontWeight: FontWeight.w500,
      color: ThemeConstants.lightTextColor,
    ),
    
    // Body text styles (most common)
    bodyLarge: TextStyle(
      fontSize: 16,
      fontWeight: FontWeight.normal,
      color: ThemeConstants.lightTextColor,
      height: 1.5, // Line height for readability
    ),
    bodyMedium: TextStyle(
      fontSize: 14,
      fontWeight: FontWeight.normal,
      color: ThemeConstants.lightTextColor,
      height: 1.4,
    ),
    
    // Label text styles (smallest)
    labelLarge: TextStyle(
      fontSize: 14,
      fontWeight: FontWeight.w500,
      color: ThemeConstants.lightTextColor,
    ),
    labelMedium: TextStyle(
      fontSize: 12,
      fontWeight: FontWeight.w500,
      color: Color(0xFF757575), // Slightly muted
    ),
  );

  /// AppBar theme for light mode
  /// Configures app bar appearance and behavior
  static const AppBarTheme _lightAppBarTheme = AppBarTheme(
    backgroundColor: ThemeConstants.lightSurfaceColor, // App bar background
    foregroundColor: ThemeConstants.lightTextColor, // Text and icon color
    elevation: 1, // Shadow depth
    centerTitle: true, // Center the title
    titleTextStyle: TextStyle(
      fontSize: 18,
      fontWeight: FontWeight.w600,
      color: ThemeConstants.lightTextColor,
    ),
    iconTheme: IconThemeData(
      color: ThemeConstants.lightTextColor, // Icon color
      size: 24, // Icon size
    ),
  );

  /// Bottom navigation bar theme
  /// Configures bottom navigation appearance
  static const BottomNavigationBarTheme _lightBottomNavTheme = BottomNavigationBarTheme(
    backgroundColor: ThemeConstants.lightSurfaceColor,
    selectedItemColor: ThemeConstants.primaryColor, // Active item color
    unselectedItemColor: Color(0xFF757575), // Inactive item color
    type: BottomNavigationBarType.fixed, // Fixed layout
    elevation: 8, // Shadow elevation
    selectedLabelStyle: TextStyle(fontWeight: FontWeight.w500),
    unselectedLabelStyle: TextStyle(fontWeight: FontWeight.normal),
  );

  /// Elevated button theme configuration
  /// Defines primary button appearance
  static final ElevatedButtonThemeData _lightElevatedButtonTheme = ElevatedButtonThemeData(
    style: ElevatedButton.styleFrom(
      backgroundColor: ThemeConstants.primaryColor, // Button background
      foregroundColor: Colors.white, // Text color
      elevation: 2, // Shadow depth
      padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(8), // Rounded corners
      ),
      textStyle: const TextStyle(
        fontSize: 16,
        fontWeight: FontWeight.w500,
      ),
    ),
  );

  /// Text button theme configuration
  /// Defines secondary button appearance
  static final TextButtonThemeData _lightTextButtonTheme = TextButtonThemeData(
    style: TextButton.styleFrom(
      foregroundColor: ThemeConstants.primaryColor, // Text color
      padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
      textStyle: const TextStyle(
        fontSize: 16,
        fontWeight: FontWeight.w500,
      ),
    ),
  );

  /// Outlined button theme configuration
  /// Defines tertiary button appearance
  static final OutlinedButtonThemeData _lightOutlinedButtonTheme = OutlinedButtonThemeData(
    style: OutlinedButton.styleFrom(
      foregroundColor: ThemeConstants.primaryColor, // Text and border color
      side: const BorderSide(color: ThemeConstants.primaryColor, width: 1),
      padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(8),
      ),
      textStyle: const TextStyle(
        fontSize: 16,
        fontWeight: FontWeight.w500,
      ),
    ),
  );

  /// Input decoration theme for text fields
  /// Standardizes form input appearance
  static const InputDecorationTheme _lightInputDecorationTheme = InputDecorationTheme(
    border: OutlineInputBorder(
      borderRadius: BorderRadius.all(Radius.circular(8)),
      borderSide: BorderSide(color: Color(0xFFE0E0E0)),
    ),
    enabledBorder: OutlineInputBorder(
      borderRadius: BorderRadius.all(Radius.circular(8)),
      borderSide: BorderSide(color: Color(0xFFE0E0E0)),
    ),
    focusedBorder: OutlineInputBorder(
      borderRadius: BorderRadius.all(Radius.circular(8)),
      borderSide: BorderSide(color: ThemeConstants.primaryColor, width: 2),
    ),
    errorBorder: OutlineInputBorder(
      borderRadius: BorderRadius.all(Radius.circular(8)),
      borderSide: BorderSide(color: ThemeConstants.errorColor),
    ),
    filled: true, // Fill background
    fillColor: Color(0xFFFAFAFA), // Light fill color
    contentPadding: EdgeInsets.symmetric(horizontal: 16, vertical: 12),
    labelStyle: TextStyle(color: Color(0xFF757575)),
    hintStyle: TextStyle(color: Color(0xFFBDBDBD)),
  );

  /// Card theme configuration
  /// Defines card component appearance
  static const CardTheme _lightCardTheme = CardTheme(
    color: ThemeConstants.lightSurfaceColor, // Card background
    elevation: 2, // Shadow depth
    margin: EdgeInsets.all(8), // Default margin
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.all(Radius.circular(12)), // Rounded corners
    ),
  );

  /// Floating action button theme
  /// Configures FAB appearance
  static const FloatingActionButtonThemeData _lightFABTheme = FloatingActionButtonThemeData(
    backgroundColor: ThemeConstants.primaryColor, // FAB background
    foregroundColor: Colors.white, // Icon color
    elevation: 6, // Shadow depth
    shape: CircleBorder(), // Circular shape
  );

  /// Icon theme configuration
  /// Sets default icon appearance
  static const IconThemeData _lightIconTheme = IconThemeData(
    color: ThemeConstants.lightTextColor, // Default icon color
    size: 24, // Default icon size
  );

  /// Divider theme configuration
  /// Defines separator line appearance
  static const DividerThemeData _lightDividerTheme = DividerThemeData(
    color: Color(0xFFE0E0E0), // Divider color
    thickness: 1, // Line thickness
    space: 16, // Space around divider
  );
}
```

## presentation/themes/dark_theme.dart

**What issue it solves/Benefits of separation:**
- ✅ Provides a complete dark theme configuration separated from light theme
- ✅ Ensures consistent dark mode styling with proper contrast ratios
- ✅ Makes it easy to modify dark theme without affecting light theme
- ✅ Follows Material Design 3 guidelines for dark themes
- ✅ Improves user experience in low-light conditions and saves battery on OLED screens

```dart
// Dark theme configuration for the application
// This file contains all the styling for dark mode, including colors,
// typography, and component-specific theming with proper contrast ratios
import 'package:flutter/material.dart';
import '../../core/constants/theme_constants.dart';

/// DarkTheme class provides the complete dark theme configuration
/// It defines colors, typography, and component themes for dark mode
class DarkTheme {
  // Private constructor to prevent instantiation
  DarkTheme._();

  /// Primary color scheme for dark theme
  /// These colors follow Material Design 3 guidelines for dark themes
  static const ColorScheme _darkColorScheme = ColorScheme.dark(
    // Primary colors - main brand colors (lighter variants for dark theme)
    primary: Color(0xFF90CAF9), // Blue 200
    onPrimary: Color(0xFF0D47A1), // Blue 900
    primaryContainer: Color(0xFF1565C0), // Blue 800
    onPrimaryContainer: Color(0xFFE3F2FD), // Blue 50
    
    // Secondary colors - accent colors (lighter variants)
    secondary: Color(0xFF80CBC4), // Teal 200
    onSecondary: Color(0xFF004D40), // Teal 900
    secondaryContainer: Color(0xFF00695C), // Teal 800
    onSecondaryContainer: Color(0xFFE0F2F1), // Teal 50
    
    // Tertiary colors - additional accent (lighter variants)
    tertiary: Color(0xFFFFB74D), // Orange 300
    onTertiary: Color(0xFFE65100), // Orange 900
    tertiaryContainer: Color(0xFFFF8F00), // Orange 800
    onTertiaryContainer: Color(0xFFFFF3E0), // Orange 50
    
    // Error colors (lighter variants for dark theme)
    error: Color(0xFFEF5350), // Red 400
    onError: Color(0xFFB71C1C), // Red 900
    errorContainer: Color(0xFFD32F2F), // Red 700
    onErrorContainer: Color(0xFFFFEBEE), // Red 50
    
    // Background colors (dark variants)
    background: Color(0xFF121212), // Very dark grey
    onBackground: Color(0xFFE0E0E0), // Light grey
    
    // Surface colors (dark variants)
    surface: Color(0xFF1E1E1E), // Dark grey
    onSurface: Color(0xFFE0E0E0), // Light grey
    surfaceVariant: Color(0xFF424242), // Grey 800
    onSurfaceVariant: Color(0xFFBDBDBD), // Grey 400
    
    // Outline colors (adjusted for dark theme)
    outline: Color(0xFF757575), // Grey 600
    outlineVariant: Color(0xFF424242), // Grey 800
    
    // Other colors
    shadow: Colors.black87,
    scrim: Colors.black87,
    inverseSurface: Color(0xFFE0E0E0), // Light grey
    onInverseSurface: Color(0xFF121212), // Very dark grey
    inversePrimary: Color(0xFF1976D2), // Blue 700
  );

  /// Text theme for dark mode
  /// Defines typography hierarchy with appropriate colors for dark backgrounds
  static const TextTheme _darkTextTheme = TextTheme(
    // Display styles - largest text
    displayLarge: TextStyle(
      fontSize: 57.0,
      fontWeight: FontWeight.w400,
      letterSpacing: -0.25,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    displayMedium: TextStyle(
      fontSize: 45.0,
      fontWeight: FontWeight.w400,
      letterSpacing: 0.0,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    displaySmall: TextStyle(
      fontSize: 36.0,
      fontWeight: FontWeight.w400,
      letterSpacing: 0.0,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    
    // Headline styles - large text
    headlineLarge: TextStyle(
      fontSize: 32.0,
      fontWeight: FontWeight.w600,
      letterSpacing: 0.0,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    headlineMedium: TextStyle(
      fontSize: 28.0,
      fontWeight: FontWeight.w600,
      letterSpacing: 0.0,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    headlineSmall: TextStyle(
      fontSize: 24.0,
      fontWeight: FontWeight.w600,
      letterSpacing: 0.0,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    
    // Title styles - medium text
    titleLarge: TextStyle(
      fontSize: 22.0,
      fontWeight: FontWeight.w500,
      letterSpacing: 0.0,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    titleMedium: TextStyle(
      fontSize: 16.0,
      fontWeight: FontWeight.w500,
      letterSpacing: 0.15,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    titleSmall: TextStyle(
      fontSize: 14.0,
      fontWeight: FontWeight.w500,
      letterSpacing: 0.1,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    
    // Body styles - regular text
    bodyLarge: TextStyle(
      fontSize: 16.0,
      fontWeight: FontWeight.w400,
      letterSpacing: 0.5,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    bodyMedium: TextStyle(
      fontSize: 14.0,
      fontWeight: FontWeight.w400,
      letterSpacing: 0.25,
      color: Color(0xFFBDBDBD),
      fontFamily: ThemeConstants.fontFamily,
    ),
    bodySmall: TextStyle(
      fontSize: 12.0,
      fontWeight: FontWeight.w400,
      letterSpacing: 0.4,
      color: Color(0xFF9E9E9E),
      fontFamily: ThemeConstants.fontFamily,
    ),
    
    // Label styles - small text
    labelLarge: TextStyle(
      fontSize: 14.0,
      fontWeight: FontWeight.w500,
      letterSpacing: 0.1,
      color: Color(0xFFE0E0E0),
      fontFamily: ThemeConstants.fontFamily,
    ),
    labelMedium: TextStyle(
      fontSize: 12.0,
      fontWeight: FontWeight.w500,
      letterSpacing: 0.5,
      color: Color(0xFFBDBDBD),
      fontFamily: ThemeConstants.fontFamily,
    ),
    labelSmall: TextStyle(
      fontSize: 11.0,
      fontWeight: FontWeight.w500,
      letterSpacing: 0.5,
      color: Color(0xFF9E9E9E),
      fontFamily: ThemeConstants.fontFamily,
    ),
  );

  /// Complete dark theme configuration
  /// This combines all theme elements into a single ThemeData object for dark mode
  static ThemeData get theme {
    return ThemeData(
      // Use Material 3 design system
      useMaterial3: true,
      
      // Set brightness to dark
      brightness: Brightness.dark,
      
      // Color scheme
      colorScheme: _darkColorScheme,
      
      // Typography
      textTheme: _darkTextTheme,
      
      // AppBar theme
      appBarTheme: const AppBarTheme(
        backgroundColor: Color(0xFF1E1E1E),
        foregroundColor: Color(0xFFE0E0E0),
        elevation: 4.0,
        centerTitle: true,
        titleTextStyle: TextStyle(
          fontSize: 20.0,
          fontWeight: FontWeight.w600,
          color: Color(0xFFE0E0E0),
          fontFamily: ThemeConstants.fontFamily,
        ),
        iconTheme: IconThemeData(
          color: Color(0xFFE0E0E0),
          size: 24.0,
        ),
      ),
      
      // Card theme
      cardTheme: CardTheme(
        color: const Color(0xFF1E1E1E),
        elevation: 2.0,
        margin: const EdgeInsets.all(8.0),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        ),
      ),
      
      // Button themes
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: ElevatedButton.styleFrom(
          backgroundColor: _darkColorScheme.primary,
          foregroundColor: _darkColorScheme.onPrimary,
          elevation: 2.0,
          padding: const EdgeInsets.symmetric(horizontal: 24.0, vertical: 12.0),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          ),
          textStyle: const TextStyle(
            fontSize: 16.0,
            fontWeight: FontWeight.w500,
            fontFamily: ThemeConstants.fontFamily,
          ),
        ),
      ),
      
      outlinedButtonTheme: OutlinedButtonThemeData(
        style: OutlinedButton.styleFrom(
          foregroundColor: _darkColorScheme.primary,
          side: BorderSide(color: _darkColorScheme.primary),
          padding: const EdgeInsets.symmetric(horizontal: 24.0, vertical: 12.0),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          ),
          textStyle: const TextStyle(
            fontSize: 16.0,
            fontWeight: FontWeight.w500,
            fontFamily: ThemeConstants.fontFamily,
          ),
        ),
      ),
      
      textButtonTheme: TextButtonThemeData(
        style: TextButton.styleFrom(
          foregroundColor: _darkColorScheme.primary,
          padding: const EdgeInsets.symmetric(horizontal: 16.0, vertical: 8.0),
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          ),
          textStyle: const TextStyle(
            fontSize: 16.0,
            fontWeight: FontWeight.w500,
            fontFamily: ThemeConstants.fontFamily,
          ),
        ),
      ),
      
      // Input decoration theme
      inputDecorationTheme: InputDecorationTheme(
        border: OutlineInputBorder(
          borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          borderSide: const BorderSide(color: Color(0xFF424242)),
        ),
        enabledBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          borderSide: const BorderSide(color: Color(0xFF424242)),
        ),
        focusedBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          borderSide: const BorderSide(color: Color(0xFF90CAF9), width: 2.0),
        ),
        errorBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          borderSide: const BorderSide(color: Color(0xFFEF5350)),
        ),
        focusedErrorBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
          borderSide: const BorderSide(color: Color(0xFFEF5350), width: 2.0),
        ),
        contentPadding: const EdgeInsets.symmetric(horizontal: 16.0, vertical: 12.0),
        hintStyle: const TextStyle(
          color: Color(0xFF757575),
          fontSize: 16.0,
          fontFamily: ThemeConstants.fontFamily,
        ),
        labelStyle: const TextStyle(
          color: Color(0xFF9E9E9E),
          fontSize: 16.0,
          fontFamily: ThemeConstants.fontFamily,
        ),
      ),
      
      // Icon theme
      iconTheme: const IconThemeData(
        color: Color(0xFFBDBDBD),
        size: 24.0,
      ),
      
      // Floating action button theme
      floatingActionButtonTheme: const FloatingActionButtonThemeData(
        backgroundColor: Color(0xFF90CAF9),
        foregroundColor: Color(0xFF0D47A1),
        elevation: 6.0,
        shape: CircleBorder(),
      ),
      
      // Bottom navigation bar theme
      bottomNavigationBarTheme: const BottomNavigationBarThemeData(
        backgroundColor: Color(0xFF1E1E1E),
        selectedItemColor: Color(0xFF90CAF9),
        unselectedItemColor: Color(0xFF9E9E9E),
        type: BottomNavigationBarType.fixed,
        elevation: 8.0,
        selectedLabelStyle: TextStyle(
          fontSize: 12.0,
          fontWeight: FontWeight.w500,
          fontFamily: ThemeConstants.fontFamily,
        ),
        unselectedLabelStyle: TextStyle(
          fontSize: 12.0,
          fontWeight: FontWeight.w400,
          fontFamily: ThemeConstants.fontFamily,
        ),
      ),
      
      // Divider theme
      dividerTheme: const DividerThemeData(
        color: Color(0xFF424242),
        thickness: 1.0,
        space: 1.0,
      ),
      
      // Chip theme
      chipTheme: ChipThemeData(
        backgroundColor: const Color(0xFF424242),
        selectedColor: _darkColorScheme.primary,
        disabledColor: const Color(0xFF2E2E2E),
        labelStyle: const TextStyle(
          color: Color(0xFFE0E0E0),
          fontSize: 14.0,
          fontFamily: ThemeConstants.fontFamily,
        ),
        secondaryLabelStyle: const TextStyle(
          color: Color(0xFF0D47A1),
          fontSize: 14.0,
          fontFamily: ThemeConstants.fontFamily,
        ),
        padding: const EdgeInsets.symmetric(horizontal: 12.0, vertical: 4.0),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(16.0),
        ),
        elevation: 1.0,
      ),
      
      // Scaffold background color
      scaffoldBackgroundColor: const Color(0xFF121212),
      
      // Background color
      backgroundColor: const Color(0xFF121212),
      
      // Primary color
      primaryColor: const Color(0xFF90CAF9),
      
      // Accent color (deprecated but still used by some widgets)
      accentColor: const Color(0xFF80CBC4),
      
      // Visual density
      visualDensity: VisualDensity.adaptivePlatformDensity,
      
      // Material tap target size
      materialTapTargetSize: MaterialTapTargetSize.padded,
      
      // Page transitions
      pageTransitionsTheme: const PageTransitionsTheme(
        builders: {
          TargetPlatform.android: CupertinoPageTransitionsBuilder(),
          TargetPlatform.iOS: CupertinoPageTransitionsBuilder(),
        },
      ),
    );
  }
}
```

## shared/enums/app_state.dart

**What issue it solves/Benefits of separation:**
- ✅ Centralizes all application state definitions in one place
- ✅ Provides type-safe state management with clear naming conventions
- ✅ Makes it easy to track and debug application states
- ✅ Enables consistent state handling across different parts of the app
- ✅ Reduces bugs by preventing invalid state assignments

```dart
// Application state enums for consistent state management
// This file defines all possible states that the application can be in
// It helps maintain consistency across different parts of the app
enum AppState {
  // Initial state when app is starting up
  initial,
  
  // Loading state when performing operations
  loading,
  
  // Success state when operations complete successfully
  success,
  
  // Error state when operations fail
  error,
  
  // Empty state when no data is available
  empty,
  
  // Offline state when network is unavailable
  offline,
  
  // Authenticated state when user is logged in
  authenticated,
  
  // Unauthenticated state when user is not logged in
  unauthenticated,
}

/// Network connection states
enum NetworkState {
  // Connected to the internet
  connected,
  
  // Disconnected from the internet
  disconnected,
  
  // Connection status unknown
  unknown,
}

/// Data loading states for more granular control
enum LoadingState {
  // Initial state before any loading
  idle,
  
  // Currently loading data
  loading,
  
  // Loading more data (pagination)
  loadingMore,
  
  // Refreshing existing data
  refreshing,
  
  // Data loaded successfully
  loaded,
  
  // Loading failed with error
  error,
}

/// Authentication states
enum AuthState {
  // Initial state
  initial,
  
  // Currently authenticating
  authenticating,
  
  // User is authenticated
  authenticated,
  
  // User is not authenticated
  unauthenticated,
  
  // Authentication failed
  authenticationFailed,
  
  // User session expired
  sessionExpired,
}

/// Theme mode states
enum ThemeState {
  // Light theme
  light,
  
  // Dark theme
  dark,
  
  // Follow system theme
  system,
}

/// Form validation states
enum ValidationState {
  // Initial state, not validated yet
  initial,
  
  // Currently validating
  validating,
  
  // Validation passed
  valid,
  
  // Validation failed
  invalid,
}

/// Upload/Download states
enum TransferState {
  // Initial state
  idle,
  
  // Transfer in progress
  inProgress,
  
  // Transfer completed successfully
  completed,
  
  // Transfer failed
  failed,
  
  // Transfer cancelled
  cancelled,
  
  // Transfer paused
  paused,
}

/// Search states
enum SearchState {
  // Initial state, no search performed
  initial,
  
  // Currently searching
  searching,
  
  // Search completed with results
  completed,
  
  // Search completed but no results found
  empty,
  
  // Search failed
  error,
}

/// Extension methods for AppState enum to provide additional functionality
extension AppStateExtension on AppState {
  /// Returns true if the current state is loading
  bool get isLoading => this == AppState.loading;
  
  /// Returns true if the current state is success
  bool get isSuccess => this == AppState.success;
  
  /// Returns true if the current state is error
  bool get isError => this == AppState.error;
  
  /// Returns true if the current state is empty
  bool get isEmpty => this == AppState.empty;
  
  /// Returns true if the current state is offline
  bool get isOffline => this == AppState.offline;
  
  /// Returns true if the user is authenticated
  bool get isAuthenticated => this == AppState.authenticated;
  
  /// Returns true if the user is unauthenticated
  bool get isUnauthenticated => this == AppState.unauthenticated;
  
  /// Returns a human-readable description of the state
  String get description {
    switch (this) {
      case AppState.initial:
        return 'Initializing application';
      case AppState.loading:
        return 'Loading data';
      case AppState.success:
        return 'Operation completed successfully';
      case AppState.error:
        return 'An error occurred';
      case AppState.empty:
        return 'No data available';
      case AppState.offline:
        return 'No internet connection';
      case AppState.authenticated:
        return 'User is logged in';
      case AppState.unauthenticated:
        return 'User is not logged in';
    }
  }
}

/// Extension methods for LoadingState enum
extension LoadingStateExtension on LoadingState {
  /// Returns true if any loading is in progress
  bool get isLoading => [
    LoadingState.loading,
    LoadingState.loadingMore,
    LoadingState.refreshing,
  ].contains(this);
  
  /// Returns true if data is loaded successfully
  bool get isLoaded => this == LoadingState.loaded;
  
  /// Returns true if loading failed
  bool get hasError => this == LoadingState.error;
}

/// Extension methods for AuthState enum
extension AuthStateExtension on AuthState {
  /// Returns true if user is authenticated
  bool get isAuthenticated => this == AuthState.authenticated;
  
  /// Returns true if authentication is in progress
  bool get isAuthenticating => this == AuthState.authenticating;
  
  /// Returns true if authentication failed
  bool get hasAuthError => [
    AuthState.authenticationFailed,
    AuthState.sessionExpired,
  ].contains(this);
}
```

## shared/enums/user_role.dart

**What issue it solves/Benefits of separation:**
- ✅ Defines user roles and permissions in a type-safe manner
- ✅ Centralizes role-based access control logic
- ✅ Makes it easy to check user permissions throughout the app
- ✅ Provides extensibility for adding new roles and permissions
- ✅ Prevents role-related bugs with clear enum definitions

```dart
// User role enums for role-based access control
// This file defines different user roles and their associated permissions
// It helps implement proper authorization throughout the application

/// Main user roles in the application
enum UserRole {
  // Guest user with limited access
  guest,
  
  // Regular user with standard access
  user,
  
  // Premium user with additional features
  premium,
  
  // Moderator with content management access
  moderator,
  
  // Administrator with full access
  admin,
  
  // Super administrator with system-level access
  superAdmin,
}

/// Specific permissions that can be granted to users
enum Permission {
  // Content permissions
  readContent,
  writeContent,
  editContent,
  deleteContent,
  publishContent,
  
  // User management permissions
  viewUsers,
  editUsers,
  deleteUsers,
  manageRoles,
  
  // Administrative permissions
  accessAdminPanel,
  manageSettings,
  viewAnalytics,
  manageSystem,
  
  // Premium features
  accessPremiumFeatures,
  unlimitedAccess,
  prioritySupport,
  
  // Moderation permissions
  moderateContent,
  banUsers,
  handleReports,
}

/// User account status
enum AccountStatus {
  // Account is active and in good standing
  active,
  
  // Account is temporarily suspended
  suspended,
  
  // Account is permanently banned
  banned,
  
  // Account is pending verification
  pending,
  
  // Account is inactive/dormant
  inactive,
  
  // Account is deleted
  deleted,
}

/// Subscription types for premium features
enum SubscriptionType {
  // Free tier
  free,
  
  // Basic paid subscription
  basic,
  
  // Premium paid subscription
  premium,
  
  // Professional subscription
  professional,
  
  // Enterprise subscription
  enterprise,
}

/// Extension methods for UserRole enum to provide additional functionality
extension UserRoleExtension on UserRole {
  /// Returns the display name for the user role
  String get displayName {
    switch (this) {
      case UserRole.guest:
        return 'Guest';
      case UserRole.user:
        return 'User';
      case UserRole.premium:
        return 'Premium User';
      case UserRole.moderator:
        return 'Moderator';
      case UserRole.admin:
        return 'Administrator';
      case UserRole.superAdmin:
        return 'Super Administrator';
    }
  }
  
  /// Returns the hierarchy level of the role (higher number = more privileges)
  int get hierarchyLevel {
    switch (this) {
      case UserRole.guest:
        return 0;
      case UserRole.user:
        return 1;
      case UserRole.premium:
        return 2;
      case UserRole.moderator:
        return 3;
      case UserRole.admin:
        return 4;
      case UserRole.superAdmin:
        return 5;
    }
  }
  
  /// Returns the list of permissions associated with this role
  List<Permission> get permissions {
    switch (this) {
      case UserRole.guest:
        return [
          Permission.readContent,
        ];
      case UserRole.user:
        return [
          Permission.readContent,
          Permission.writeContent,
          Permission.editContent,
        ];
      case UserRole.premium:
        return [
          Permission.readContent,
          Permission.writeContent,
          Permission.editContent,
          Permission.accessPremiumFeatures,
          Permission.unlimitedAccess,
          Permission.prioritySupport,
        ];
      case UserRole.moderator:
        return [
          Permission.readContent,
          Permission.writeContent,
          Permission.editContent,
          Permission.deleteContent,
          Permission.moderateContent,
          Permission.handleReports,
          Permission.viewUsers,
        ];
      case UserRole.admin:
        return [
          Permission.readContent,
          Permission.writeContent,
          Permission.editContent,
          Permission.deleteContent,
          Permission.publishContent,
          Permission.moderateContent,
          Permission.handleReports,
          Permission.viewUsers,
          Permission.editUsers,
          Permission.manageRoles,
          Permission.accessAdminPanel,
          Permission.viewAnalytics,
        ];
      case UserRole.superAdmin:
        return Permission.values; // All permissions
    }
  }
  
  /// Checks if this role has a specific permission
  bool hasPermission(Permission permission) {
    return permissions.contains(permission);
  }
  
  /// Checks if this role has higher or equal hierarchy than another role
  bool hasHigherOrEqualHierarchy(UserRole otherRole) {
    return hierarchyLevel >= otherRole.hierarchyLevel;
  }
  
  /// Returns true if this is an administrative role
  bool get isAdmin {
    return [UserRole.admin, UserRole.superAdmin].contains(this);
  }
  
  /// Returns true if this is a premium role
  bool get isPremium {
    return [UserRole.premium, UserRole.moderator, UserRole.admin, UserRole.superAdmin]
        .contains(this);
  }
  
  /// Returns true if this role can moderate content
  bool get canModerate {
    return [UserRole.moderator, UserRole.admin, UserRole.superAdmin]
        .contains(this);
  }
}

/// Extension methods for Permission enum
extension PermissionExtension on Permission {
  /// Returns the display name for the permission
  String get displayName {
    switch (this) {
      case Permission.readContent:
        return 'Read Content';
      case Permission.writeContent:
        return 'Write Content';
      case Permission.editContent:
        return 'Edit Content';
      case Permission.deleteContent:
        return 'Delete Content';
      case Permission.publishContent:
        return 'Publish Content';
      case Permission.viewUsers:
        return 'View Users';
      case Permission.editUsers:
        return 'Edit Users';
      case Permission.deleteUsers:
        return 'Delete Users';
      case Permission.manageRoles:
        return 'Manage Roles';
      case Permission.accessAdminPanel:
        return 'Access Admin Panel';
      case Permission.manageSettings:
        return 'Manage Settings';
      case Permission.viewAnalytics:
        return 'View Analytics';
      case Permission.manageSystem:
        return 'Manage System';
      case Permission.accessPremiumFeatures:
        return 'Access Premium Features';
      case Permission.unlimitedAccess:
        return 'Unlimited Access';
      case Permission.prioritySupport:
        return 'Priority Support';
      case Permission.moderateContent:
        return 'Moderate Content';
      case Permission.banUsers:
        return 'Ban Users';
      case Permission.handleReports:
        return 'Handle Reports';
    }
  }
  
  /// Returns the description of what this permission allows
  String get description {
    switch (this) {
      case Permission.readContent:
        return 'Allows viewing and reading content';
      case Permission.writeContent:
        return 'Allows creating new content';
      case Permission.editContent:
        return 'Allows editing existing content';
      case Permission.deleteContent:
        return 'Allows deleting content';
      case Permission.publishContent:
        return 'Allows publishing content for public viewing';
      case Permission.viewUsers:
        return 'Allows viewing user profiles and information';
      case Permission.editUsers:
        return 'Allows editing user profiles and information';
      case Permission.deleteUsers:
        return 'Allows deleting user accounts';
      case Permission.manageRoles:
        return 'Allows assigning and managing user roles';
      case Permission.accessAdminPanel:
        return 'Allows access to the administrative interface';
      case Permission.manageSettings:
        return 'Allows modifying application settings';
      case Permission.viewAnalytics:
        return 'Allows viewing analytics and reports';
      case Permission.manageSystem:
        return 'Allows system-level management and configuration';
      case Permission.accessPremiumFeatures:
        return 'Allows access to premium-only features';
      case Permission.unlimitedAccess:
        return 'Removes limitations on usage and access';
      case Permission.prioritySupport:
        return 'Provides priority customer support';
      case Permission.moderateContent:
        return 'Allows moderating and reviewing content';
      case Permission.banUsers:
        return 'Allows banning and suspending users';
      case Permission.handleReports:
        return 'Allows managing user reports and complaints';
    }
  }
}

/// Extension methods for AccountStatus enum
extension AccountStatusExtension on AccountStatus {dart
// Main theme configuration that orchestrates light and dark themes
// This file acts as the central hub for all theme-related configurations
import 'package:flutter/material.dart';
import 'light_theme.dart';
import 'dark_theme.dart';
import '../../core/constants/theme_constants.dart';

/// AppTheme class manages the overall theme configuration for the application
/// It provides static methods to get light and dark themes, and utility methods
/// for accessing theme-specific properties
class AppTheme {
  // Private constructor to prevent instantiation
  // This class is designed to be used as a static utility class
  AppTheme._();

  /// Returns the light theme configuration
  /// This method creates and returns a complete ThemeData object for light mode
  static ThemeData get lightTheme {
    return LightTheme.theme;
  }

  /// Returns the dark theme configuration
  /// This method creates and returns a complete ThemeData object for dark mode
  static ThemeData get darkTheme {
    return DarkTheme.theme;
  }

  /// Gets the current theme mode based on system settings or user preference
  /// This can be extended to include user preference from shared preferences
  static ThemeMode get themeMode {
    // For now, we follow system theme, but this can be customized
    // to read from user preferences stored in local storage
    return ThemeMode.system;
  }

  /// Utility method to get text theme from current context
  /// This provides easy access to text styles throughout the app
  static TextTheme getTextTheme(BuildContext context) {
    return Theme.of(context).textTheme;
  }

  /// Utility method to get color scheme from current context
  /// This provides easy access to color palette throughout the app
  static ColorScheme getColorScheme(BuildContext context) {
    return Theme.of(context).colorScheme;
  }

  /// Checks if the current theme is dark mode
  /// Useful for conditional rendering based on theme
  static bool isDarkMode(BuildContext context) {
    return Theme.of(context).brightness == Brightness.dark;
  }

  /// Gets the primary color from current theme
  /// Provides quick access to the main brand color
  static Color getPrimaryColor(BuildContext context) {
    return Theme.of(context).primaryColor;
  }

  /// Gets the background color from current theme
  /// Useful for setting consistent background colors
  static Color getBackgroundColor(BuildContext context) {
    return Theme.of(context).scaffoldBackgroundColor;
  }

  /// Gets the card color from current theme
  /// Ensures consistent card styling across the app
  static Color getCardColor(BuildContext context) {
    return Theme.of(context).cardColor;
  }

  /// Gets the error color from current theme
  /// Provides consistent error state styling
  static Color getErrorColor(BuildContext context) {
    return Theme.of(context).colorScheme.error;
  }

  /// Gets the success color from theme constants
  /// Since Flutter doesn't have a built-in success color, we use our custom one
  static Color getSuccessColor(BuildContext context) {
    return isDarkMode(context) 
        ? ThemeConstants.successColorDark 
        : ThemeConstants.successColorLight;
  }

  /// Gets the warning color from theme constants
  /// Since Flutter doesn't have a built-in warning color, we use our custom one
  static Color getWarningColor(BuildContext context) {
    return isDarkMode(context) 
        ? ThemeConstants.warningColorDark 
        : ThemeConstants.warningColorLight;
  }

  /// Creates a custom TextStyle with theme-aware colors
  /// This method helps create consistent text styles throughout the app
  static TextStyle createTextStyle({
    required BuildContext context,
    double? fontSize,
    FontWeight? fontWeight,
    Color? color,
    double? letterSpacing,
    double? height,
  }) {
    return TextStyle(
      fontSize: fontSize ?? 14.0,
      fontWeight: fontWeight ?? FontWeight.normal,
      color: color ?? getTextColor(context),
      letterSpacing: letterSpacing,
      height: height,
      fontFamily: ThemeConstants.fontFamily,
    );
  }

  /// Gets the appropriate text color based on current theme
  /// Ensures text is readable in both light and dark modes
  static Color getTextColor(BuildContext context) {
    return Theme.of(context).textTheme.bodyLarge?.color ?? 
           (isDarkMode(context) ? Colors.white : Colors.black);
  }

  /// Gets the appropriate secondary text color based on current theme
  /// Used for less prominent text elements
  static Color getSecondaryTextColor(BuildContext context) {
    return Theme.of(context).textTheme.bodyMedium?.color ?? 
           (isDarkMode(context) ? Colors.white70 : Colors.black54);
  }

  /// Creates a BoxDecoration with theme-aware styling
  /// Useful for creating consistent container decorations
  static BoxDecoration createBoxDecoration({
    required BuildContext context,
    Color? color,
    double borderRadius = 8.0,
    Color? borderColor,
    double borderWidth = 1.0,
    List<BoxShadow>? boxShadow,
  }) {
    return BoxDecoration(
      color: color ?? getCardColor(context),
      borderRadius: BorderRadius.circular(borderRadius),
      border: borderColor != null 
          ? Border.all(color: borderColor, width: borderWidth)
          : null,
      boxShadow: boxShadow ?? [
        BoxShadow(
          color: isDarkMode(context) 
              ? Colors.black26 
              : Colors.grey.withOpacity(0.1),
          blurRadius: 4.0,
          offset: const Offset(0, 2),
        ),
      ],
    );
  }

  /// Creates an InputDecoration with consistent theme styling
  /// Ensures all text fields have the same look and feel
  static InputDecoration createInputDecoration({
    required BuildContext context,
    String? labelText,
    String? hintText,
    IconData? prefixIcon,
    IconData? suffixIcon,
    VoidCallback? onSuffixIconPressed,
  }) {
    return InputDecoration(
      labelText: labelText,
      hintText: hintText,
      prefixIcon: prefixIcon != null ? Icon(prefixIcon) : null,
      suffixIcon: suffixIcon != null 
          ? IconButton(
              icon: Icon(suffixIcon),
              onPressed: onSuffixIconPressed,
            )
          : null,
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
      ),
      enabledBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: isDarkMode(context) 
              ? Colors.grey.shade600 
              : Colors.grey.shade300,
        ),
      ),
      focusedBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: getPrimaryColor(context),
          width: 2.0,
        ),
      ),
      errorBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: getErrorColor(context),
        ),
      ),
      focusedErrorBorder: OutlineInputBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
        borderSide: BorderSide(
          color: getErrorColor(context),
          width: 2.0,
        ),
      ),
      contentPadding: const EdgeInsets.symmetric(
        horizontal: 16.0,
        vertical: 12.0,
      ),
    );
  }

  /// Creates a ButtonStyle with consistent theme styling
  /// Ensures all buttons have the same look and feel
  static ButtonStyle createButtonStyle({
    required BuildContext context,
    Color? backgroundColor,
    Color? foregroundColor,
    double? elevation,
    EdgeInsetsGeometry? padding,
    Size? minimumSize,
  }) {
    return ElevatedButton.styleFrom(
      backgroundColor: backgroundColor ?? getPrimaryColor(context),
      foregroundColor: foregroundColor ?? Colors.white,
      elevation: elevation ?? 2.0,
      padding: padding ?? const EdgeInsets.symmetric(
        horizontal: 24.0,
        vertical: 12.0,
      ),
      minimumSize: minimumSize ?? const Size(100, 44),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
      ),
    );
  }

  /// Creates an AppBar theme with consistent styling
  /// Ensures all app bars have the same look and feel
  static AppBarTheme createAppBarTheme(BuildContext context) {
    return AppBarTheme(
      backgroundColor: getPrimaryColor(context),
      foregroundColor: Colors.white,
      elevation: 4.0,
      centerTitle: true,
      titleTextStyle: createTextStyle(
        context: context,
        fontSize: 20.0,
        fontWeight: FontWeight.w600,
        color: Colors.white,
      ),
      iconTheme: const IconThemeData(
        color: Colors.white,
        size: 24.0,
      ),
    );
  }

  /// Creates a Card theme with consistent styling
  /// Ensures all cards have the same look and feel
  static CardTheme createCardTheme(BuildContext context) {
    return CardTheme(
      color: getCardColor(context),
      elevation: 2.0,
      margin: const EdgeInsets.all(8.0),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(ThemeConstants.borderRadius),
      ),
    );
  }
}
```


## shared/mixins/validation_mixin.dart

**What issue it solves**: Provides reusable validation logic across different controllers and forms. Eliminates code duplication and ensures consistent validation rules throughout the application.

**Benefits**:
- Code reusability across multiple controllers
- Consistent validation rules
- Easy to maintain and update validation logic
- Separation of validation concerns from business logic

```dart
/// Validation mixin that provides common validation methods
/// This mixin can be used by controllers to validate user input
/// without duplicating validation logic across different classes
mixin ValidationMixin {
  
  /// Validates email format using regular expression
  /// Returns error message if invalid, null if valid
  /// 
  /// Example usage:
  /// ```dart
  /// String? error = validateEmail('user@example.com');
  /// if (error != null) {
  ///   // Handle validation error
  /// }
  /// ```
  String? validateEmail(String? email) {
    // Check if email is null or empty
    if (email == null || email.trim().isEmpty) {
      return 'Email is required';
    }
    
    // Regular expression for email validation
    // Matches standard email format: username@domain.extension
    final emailRegExp = RegExp(
      r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$',
    );
    
    // Check if email matches the pattern
    if (!emailRegExp.hasMatch(email.trim())) {
      return 'Please enter a valid email address';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates password strength and requirements
  /// Returns error message if invalid, null if valid
  /// 
  /// Password requirements:
  /// - Minimum 8 characters
  /// - At least one uppercase letter
  /// - At least one lowercase letter
  /// - At least one number
  /// - At least one special character
  String? validatePassword(String? password) {
    // Check if password is null or empty
    if (password == null || password.isEmpty) {
      return 'Password is required';
    }
    
    // Check minimum length
    if (password.length < 8) {
      return 'Password must be at least 8 characters long';
    }
    
    // Check for uppercase letter
    if (!password.contains(RegExp(r'[A-Z]'))) {
      return 'Password must contain at least one uppercase letter';
    }
    
    // Check for lowercase letter
    if (!password.contains(RegExp(r'[a-z]'))) {
      return 'Password must contain at least one lowercase letter';
    }
    
    // Check for number
    if (!password.contains(RegExp(r'[0-9]'))) {
      return 'Password must contain at least one number';
    }
    
    // Check for special character
    if (!password.contains(RegExp(r'[!@#$%^&*(),.?":{}|<>]'))) {
      return 'Password must contain at least one special character';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates phone number format
  /// Accepts various phone number formats
  /// Returns error message if invalid, null if valid
  String? validatePhoneNumber(String? phoneNumber) {
    // Check if phone number is null or empty
    if (phoneNumber == null || phoneNumber.trim().isEmpty) {
      return 'Phone number is required';
    }
    
    // Remove all non-digit characters for validation
    final digitsOnly = phoneNumber.replaceAll(RegExp(r'[^\d]'), '');
    
    // Check if phone number has valid length (10-15 digits)
    if (digitsOnly.length < 10 || digitsOnly.length > 15) {
      return 'Please enter a valid phone number';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates required text fields
  /// Returns error message if empty, null if valid
  /// 
  /// Parameters:
  /// - value: The text to validate
  /// - fieldName: Name of the field for error message
  /// - minLength: Minimum required length (optional)
  String? validateRequired(String? value, String fieldName, {int? minLength}) {
    // Check if value is null or empty
    if (value == null || value.trim().isEmpty) {
      return '$fieldName is required';
    }
    
    // Check minimum length if specified
    if (minLength != null && value.trim().length < minLength) {
      return '$fieldName must be at least $minLength characters long';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates numeric input
  /// Returns error message if invalid, null if valid
  /// 
  /// Parameters:
  /// - value: The text to validate as number
  /// - fieldName: Name of the field for error message
  /// - min: Minimum allowed value (optional)
  /// - max: Maximum allowed value (optional)
  String? validateNumber(String? value, String fieldName, {double? min, double? max}) {
    // Check if value is null or empty
    if (value == null || value.trim().isEmpty) {
      return '$fieldName is required';
    }
    
    // Try to parse as double
    final number = double.tryParse(value.trim());
    if (number == null) {
      return 'Please enter a valid number for $fieldName';
    }
    
    // Check minimum value
    if (min != null && number < min) {
      return '$fieldName must be at least $min';
    }
    
    // Check maximum value
    if (max != null && number > max) {
      return '$fieldName must not exceed $max';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates age input
  /// Returns error message if invalid, null if valid
  String? validateAge(String? age) {
    // Use number validation with age-specific constraints
    final numberError = validateNumber(age, 'Age', min: 1, max: 150);
    if (numberError != null) {
      return numberError;
    }
    
    // Additional age-specific validation
    final ageValue = int.parse(age!);
    if (ageValue < 13) {
      return 'You must be at least 13 years old';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates URL format
  /// Returns error message if invalid, null if valid
  String? validateUrl(String? url) {
    // Check if URL is null or empty
    if (url == null || url.trim().isEmpty) {
      return 'URL is required';
    }
    
    // Try to parse as URI
    try {
      final uri = Uri.parse(url.trim());
      
      // Check if URI has scheme (http/https)
      if (!uri.hasScheme || (!uri.scheme.startsWith('http') && !uri.scheme.startsWith('https'))) {
        return 'Please enter a valid URL starting with http:// or https://';
      }
      
      // Check if URI has host
      if (!uri.hasAuthority || uri.host.isEmpty) {
        return 'Please enter a valid URL with a domain name';
      }
      
    } catch (e) {
      return 'Please enter a valid URL';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates that two password fields match
  /// Returns error message if they don't match, null if valid
  /// 
  /// Commonly used for password confirmation fields
  String? validatePasswordConfirmation(String? password, String? confirmPassword) {
    // Check if confirmation password is null or empty
    if (confirmPassword == null || confirmPassword.isEmpty) {
      return 'Please confirm your password';
    }
    
    // Check if passwords match
    if (password != confirmPassword) {
      return 'Passwords do not match';
    }
    
    // Return null if validation passes
    return null;
  }

  /// Validates multiple fields at once
  /// Returns a map of field names to error messages
  /// Only includes fields that have validation errors
  /// 
  /// Example usage:
  /// ```dart
  /// final errors = validateMultiple({
  ///   'email': () => validateEmail(emailController.text),
  ///   'password': () => validatePassword(passwordController.text),
  /// });
  /// 
  /// if (errors.isNotEmpty) {
  ///   // Handle validation errors
  /// }
  /// ```
  Map<String, String> validateMultiple(Map<String, String? Function()> validators) {
    final errors = <String, String>{};
    
    // Run each validator and collect errors
    validators.forEach((fieldName, validator) {
      final error = validator();
      if (error != null) {
        errors[fieldName] = error;
      }
    });
    
    return errors;
  }
}
```

## test/home_screen_test.dart

**What issue it solves**: Ensures the home screen widget functions correctly through automated testing. Catches bugs early and ensures UI components render and behave as expected.

**Benefits**:
- Automated quality assurance
- Regression testing
- Documentation of expected behavior
- Confidence in code changes

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:provider/provider.dart';
import 'package:mockito/mockito.dart';
import 'package:mockito/annotations.dart';

// Import the files to be tested
import '../lib/presentation/views/screens/home_screen.dart';
import '../lib/presentation/controllers/home_controller.dart';
import '../lib/data/providers/user_provider.dart';
import '../lib/data/providers/product_provider.dart';
import '../lib/data/models/user_model.dart';
import '../lib/data/models/product_model.dart';

// Generate mock classes using mockito
// Run 'flutter packages pub run build_runner build' to generate mocks
@GenerateMocks([HomeController, UserProvider, ProductProvider])
import 'home_screen_test.mocks.dart';

/// Test suite for HomeScreen widget
/// Tests various scenarios and user interactions on the home screen
void main() {
  // Test group for HomeScreen widget tests
  group('HomeScreen Widget Tests', () {
    // Mock objects for testing
    late MockHomeController mockHomeController;
    late MockUserProvider mockUserProvider;
    late MockProductProvider mockProductProvider;

    // Setup method that runs before each test
    setUp(() {
      // Initialize mock objects
      mockHomeController = MockHomeController();
      mockUserProvider = MockUserProvider();
      mockProductProvider = MockProductProvider();
      
      // Setup default mock behaviors
      _setupDefaultMockBehaviors();
    });

    /// Helper method to create a testable widget
    /// Wraps the HomeScreen with necessary providers and MaterialApp
    Widget createTestableWidget() {
      return MultiProvider(
        providers: [
          // Provide mock controllers and providers
          ChangeNotifierProvider<UserProvider>.value(value: mockUserProvider),
          ChangeNotifierProvider<ProductProvider>.value(value: mockProductProvider),
          Provider<HomeController>.value(value: mockHomeController),
        ],
        child: MaterialApp(
          // Wrap in MaterialApp for proper widget testing
          home: HomeScreen(),
          // Add theme for consistent testing
          theme: ThemeData(
            primarySwatch: Colors.blue,
          ),
        ),
      );
    }

    /// Test 1: Verify that HomeScreen renders without crashing
    testWidgets('should render HomeScreen without errors', (WidgetTester tester) async {
      // Arrange: Create the testable widget
      final widget = createTestableWidget();
      
      // Act: Pump the widget into the test environment
      await tester.pumpWidget(widget);
      
      // Assert: Verify the widget is rendered
      expect(find.byType(HomeScreen), findsOneWidget);
      
      // Verify key UI elements are present
      expect(find.byType(AppBar), findsOneWidget);
      expect(find.byType(Scaffold), findsOneWidget);
    });

    /// Test 2: Verify app bar title is displayed correctly
    testWidgets('should display correct app bar title', (WidgetTester tester) async {
      // Arrange
      final widget = createTestableWidget();
      
      // Act
      await tester.pumpWidget(widget);
      
      // Assert: Check if app bar title is present
      expect(find.text('Home'), findsOneWidget);
      
      // Verify app bar structure
      final appBar = tester.widget<AppBar>(find.byType(AppBar));
      expect(appBar.title, isA<Text>());
    });

    /// Test 3: Test loading state display
    testWidgets('should show loading indicator when data is loading', (WidgetTester tester) async {
      // Arrange: Setup loading state
      when(mockUserProvider.isLoading).thenReturn(true);
      when(mockProductProvider.isLoading).thenReturn(true);
      
      final widget = createTestableWidget();
      
      // Act
      await tester.pumpWidget(widget);
      
      // Assert: Verify loading indicator is shown
      expect(find.byType(CircularProgressIndicator), findsOneWidget);
      
      // Verify loading text is displayed
      expect(find.text('Loading...'), findsOneWidget);
    });

    /// Test 4: Test data display when loaded successfully
    testWidgets('should display user and product data when loaded', (WidgetTester tester) async {
      // Arrange: Setup loaded state with mock data
      when(mockUserProvider.isLoading).thenReturn(false);
      when(mockProductProvider.isLoading).thenReturn(false);
      when(mockUserProvider.currentUser).thenReturn(_createMockUser());
      when(mockProductProvider.products).thenReturn(_createMockProducts());
      
      final widget = createTestableWidget();
      
      // Act
      await tester.pumpWidget(widget);
      
      // Assert: Verify data is displayed
      expect(find.text('Welcome, John Doe'), findsOneWidget);
      expect(find.text('Featured Products'), findsOneWidget);
      
      // Verify product cards are displayed
      expect(find.byType(Card), findsAtLeastNWidgets(2));
    });

    /// Test 5: Test error state display
    testWidgets('should show error message when data loading fails', (WidgetTester tester) async {
      // Arrange: Setup error state
      when(mockUserProvider.isLoading).thenReturn(false);
      when(mockUserProvider.error).thenReturn('Failed to load user data');
      when(mockProductProvider.isLoading).thenReturn(false);
      when(mockProductProvider.error).thenReturn(null);
      
      final widget = createTestableWidget();
      
      // Act
      await tester.pumpWidget(widget);
      
      // Assert: Verify error message is shown
      expect(find.text('Error: Failed to load user data'), findsOneWidget);
      
      // Verify error icon is displayed
      expect(find.byIcon(Icons.error), findsOneWidget);
    });

    /// Test 6: Test refresh functionality
    testWidgets('should trigger refresh when pull-to-refresh is used', (WidgetTester tester) async {
      // Arrange
      when(mockUserProvider.isLoading).thenReturn(false);
      when(mockProductProvider.isLoading).thenReturn(false);
      
      final widget = createTestableWidget();
      await tester.pumpWidget(widget);
      
      // Act: Simulate pull-to-refresh gesture
      await tester.drag(find.byType(RefreshIndicator), const Offset(0, 200));
      await tester.pump();
      
      // Assert: Verify refresh methods were called
      verify(mockHomeController.refreshData()).called(1);
    });

    /// Test 7: Test navigation to profile screen
    testWidgets('should navigate to profile when profile button is tapped', (WidgetTester tester) async {
      // Arrange
      when(mockUserProvider.isLoading).thenReturn(false);
      when(mockUserProvider.currentUser).thenReturn(_createMockUser());
      
      final widget = createTestableWidget();
      await tester.pumpWidget(widget);
      
      // Act: Tap on profile icon/button
      await tester.tap(find.byIcon(Icons.person));
      await tester.pumpAndSettle();
      
      // Assert: Verify navigation method was called
      verify(mockHomeController.navigateToProfile()).called(1);
    });

    /// Test 8: Test product card interaction
    testWidgets('should handle product card tap', (WidgetTester tester) async {
      // Arrange
      when(mockProductProvider.isLoading).thenReturn(false);
      when(mockProductProvider.products).thenReturn(_createMockProducts());
      
      final widget = createTestableWidget();
      await tester.pumpWidget(widget);
      
      // Act: Tap on first product card
      final productCard = find.byType(Card).first;
      await tester.tap(productCard);
      await tester.pumpAndSettle();
      
      // Assert: Verify product selection method was called
      verify(mockHomeController.selectProduct(any)).called(1);
    });

    /// Test 9: Test bottom navigation interaction
    testWidgets('should handle bottom navigation tap', (WidgetTester tester) async {
      // Arrange
      final widget = createTestableWidget();
      await tester.pumpWidget(widget);
      
      // Act: Tap on second bottom navigation item
      final bottomNavItems = find.byType(BottomNavigationBarItem);
      if (bottomNavItems.evaluate().isNotEmpty) {
        await tester.tap(find.byIcon(Icons.search));
        await tester.pumpAndSettle();
        
        // Assert: Verify navigation method was called
        verify(mockHomeController.changeTab(1)).called(1);
      }
    });

    /// Test 10: Test widget disposal and cleanup
    testWidgets('should dispose resources properly', (WidgetTester tester) async {
      // Arrange
      final widget = createTestableWidget();
      await tester.pumpWidget(widget);
      
      // Act: Remove widget from tree (simulate disposal)
      await tester.pumpWidget(Container());
      
      // Assert: Verify cleanup methods were called
      // This would verify that controllers dispose of resources properly
      verify(mockHomeController.dispose()).called(1);
    });
  });

  /// Group for testing HomeScreen integration with real data
  group('HomeScreen Integration Tests', () {
    
    /// Test real API integration (if needed)
    testWidgets('should handle real API data loading', (WidgetTester tester) async {
      // This test would use real providers instead of mocks
      // to test actual API integration
      
      // Skip this test if running in CI environment
      // where real API calls are not desired
      if (const bool.fromEnvironment('SKIP_INTEGRATION_TESTS')) {
        return;
      }
 ```
      
## test/widget_test.dart

**What issue it solves**: Provides comprehensive widget testing for the entire application, ensuring UI components work correctly in isolation and integration. Tests user interactions, state changes, and widget behavior.

**Benefits**:
- Comprehensive UI testing coverage
- Early detection of widget-related bugs
- Ensures accessibility and usability
- Documents expected widget behavior

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:provider/provider.dart';
import 'package:mockito/mockito.dart';
import 'package:mockito/annotations.dart';

// Import all necessary files for testing
import '../lib/main.dart';
import '../lib/presentation/views/widgets/common/custom_button.dart';
import '../lib/presentation/views/widgets/common/custom_text_field.dart';
import '../lib/presentation/views/widgets/common/loading_widget.dart';
import '../lib/presentation/views/widgets/common/error_widget.dart';
import '../lib/presentation/views/widgets/specific/user_card.dart';
import '../lib/presentation/views/widgets/specific/product_card.dart';
import '../lib/data/models/user_model.dart';
import '../lib/data/models/product_model.dart';
import '../lib/data/providers/user_provider.dart';
import '../lib/data/providers/product_provider.dart';
import '../lib/data/providers/theme_provider.dart';

// Generate mocks for testing
@GenerateMocks([UserProvider, ProductProvider, ThemeProvider])
import 'widget_test.mocks.dart';

/// Main widget test suite
/// Tests various widgets and their interactions
void main() {
  
  /// Test group for main app functionality
  group('Main App Tests', () {
    
    testWidgets('should create main app without errors', (WidgetTester tester) async {
      // Test that the main app can be created and rendered
      await tester.pumpWidget(MyApp());
      
      // Verify the app is created
      expect(find.byType(MaterialApp), findsOneWidget);
    });
    
    testWidgets('should have correct app title', (WidgetTester tester) async {
      // Pump the main app
      await tester.pumpWidget(MyApp());
      
      // Get the MaterialApp widget
      final materialApp = tester.widget<MaterialApp>(find.byType(MaterialApp));
      
      // Verify app title
      expect(materialApp.title, 'Flutter MVC Demo');
    });
  });

  /// Test group for custom button widget
  group('CustomButton Widget Tests', () {
    
    testWidgets('should display button text correctly', (WidgetTester tester) async {
      // Arrange: Create button with test text
      const buttonText = 'Test Button';
      bool wasPressed = false;
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomButton(
              text: buttonText,
              onPressed: () {
                wasPressed = true;
              },
            ),
          ),
        ),
      );
      
      // Assert: Verify button text is displayed
      expect(find.text(buttonText), findsOneWidget);
      expect(find.byType(ElevatedButton), findsOneWidget);
    });
    
    testWidgets('should handle button press correctly', (WidgetTester tester) async {
      // Arrange: Setup button press tracking
      bool wasPressed = false;
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomButton(
              text: 'Press Me',
              onPressed: () {
                wasPressed = true;
              },
            ),
          ),
        ),
      );
      
      // Act: Tap the button
      await tester.tap(find.byType(ElevatedButton));
      await tester.pumpAndSettle();
      
      // Assert: Verify callback was executed
      expect(wasPressed, true);
    });
    
    testWidgets('should be disabled when onPressed is null', (WidgetTester tester) async {
      // Arrange: Create disabled button
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomButton(
              text: 'Disabled Button',
              onPressed: null, // Disabled button
            ),
          ),
        ),
      );
      
      // Assert: Verify button is disabled
      final button = tester.widget<ElevatedButton>(find.byType(ElevatedButton));
      expect(button.onPressed, isNull);
    });
    
    testWidgets('should show loading state when isLoading is true', (WidgetTester tester) async {
      // Arrange: Create button in loading state
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomButton(
              text: 'Loading Button',
              onPressed: () {},
              isLoading: true,
            ),
          ),
        ),
      );
      
      // Assert: Verify loading indicator is shown
      expect(find.byType(CircularProgressIndicator), findsOneWidget);
      expect(find.text('Loading Button'), findsNothing);
    });
  });

  /// Test group for custom text field widget
  group('CustomTextField Widget Tests', () {
    
    testWidgets('should display hint text correctly', (WidgetTester tester) async {
      // Arrange: Create text field with hint
      const hintText = 'Enter your name';
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomTextField(
              hintText: hintText,
            ),
          ),
        ),
      );
      
      // Assert: Verify hint text is displayed
      expect(find.text(hintText), findsOneWidget);
      expect(find.byType(TextFormField), findsOneWidget);
    });
    
    testWidgets('should handle text input correctly', (WidgetTester tester) async {
      // Arrange: Create text field with controller
      final controller = TextEditingController();
      const inputText = 'Hello World';
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomTextField(
              controller: controller,
              hintText: 'Enter text',
            ),
          ),
        ),
      );
      
      // Act: Enter text
      await tester.enterText(find.byType(TextFormField), inputText);
      
      // Assert: Verify text was entered
      expect(controller.text, inputText);
      expect(find.text(inputText), findsOneWidget);
    });
    
    testWidgets('should display validation error', (WidgetTester tester) async {
      // Arrange: Create text field with validator
      const errorMessage = 'This field is required';
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: Form(
              child: CustomTextField(
                hintText: 'Required field',
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return errorMessage;
                  }
                  return null;
                },
              ),
            ),
          ),
        ),
      );
      
      // Act: Trigger validation by submitting empty form
      final formState = tester.state(find.byType(Form)) as FormState;
      formState.validate();
      await tester.pumpAndSettle();
      
      // Assert: Verify error message is displayed
      expect(find.text(errorMessage), findsOneWidget);
    });
    
    testWidgets('should toggle password visibility', (WidgetTester tester) async {
      // Arrange: Create password field
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomTextField(
              hintText: 'Password',
              isPassword: true,
            ),
          ),
        ),
      );
      
      // Assert: Initially password should be obscured
      final textField = tester.widget<TextFormField>(find.byType(TextFormField));
      expect(textField.obscureText, true);
      
      // Act: Tap visibility toggle
      await tester.tap(find.byIcon(Icons.visibility_off));
      await tester.pumpAndSettle();
      
      // Assert: Password should now be visible
      final updatedTextField = tester.widget<TextFormField>(find.byType(TextFormField));
      expect(updatedTextField.obscureText, false);
    });
  });

  /// Test group for loading widget
  group('LoadingWidget Tests', () {
    
    testWidgets('should display loading indicator', (WidgetTester tester) async {
      // Arrange & Act: Create loading widget
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: LoadingWidget(),
          ),
        ),
      );
      
      // Assert: Verify loading components are present
      expect(find.byType(CircularProgressIndicator), findsOneWidget);
      expect(find.text('Loading...'), findsOneWidget);
    });
    
    testWidgets('should display custom loading message', (WidgetTester tester) async {
      // Arrange: Create loading widget with custom message
      const customMessage = 'Please wait...';
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: LoadingWidget(message: customMessage),
          ),
        ),
      );
      
      // Assert: Verify custom message is displayed
      expect(find.text(customMessage), findsOneWidget);
    });
  });

  /// Test group for error widget
  group('ErrorWidget Tests', () {
    
    testWidgets('should display error message', (WidgetTester tester) async {
      // Arrange: Create error widget with message
      const errorMessage = 'Something went wrong';
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomErrorWidget(message: errorMessage),
          ),
        ),
      );
      
      // Assert: Verify error components are present
      expect(find.text(errorMessage), findsOneWidget);
      expect(find.byIcon(Icons.error), findsOneWidget);
    });
    
    testWidgets('should handle retry button press', (WidgetTester tester) async {
      // Arrange: Setup retry callback tracking
      bool retryPressed = false;
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: CustomErrorWidget(
              message: 'Error occurred',
              onRetry: () {
                retryPressed = true;
              },
            ),
          ),
        ),
      );
      
      // Act: Tap retry button
      await tester.tap(find.text('Retry'));
      await tester.pumpAndSettle();
      
      // Assert: Verify retry callback was executed
      expect(retryPressed, true);
    });
  });

  /// Test group for user card widget
  group('UserCard Widget Tests', () {
    late UserModel testUser;
    
    setUp(() {
      // Create test user data
      testUser = UserModel(
        id: '1',
        name: 'John Doe',
        email: 'john@example.com',
        avatarUrl: 'https://example.com/avatar.jpg',
        role: 'admin',
        createdAt: DateTime.now(),
      );
    });
    
    testWidgets('should display user information correctly', (WidgetTester tester) async {
      // Arrange & Act: Create user card
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserCard(user: testUser),
          ),
        ),
      );
      
      // Assert: Verify user information is displayed
      expect(find.text(testUser.name), findsOneWidget);
      expect(find.text(testUser.email), findsOneWidget);
      expect(find.text(testUser.role.toUpperCase()), findsOneWidget);
    });
    
    testWidgets('should handle user card tap', (WidgetTester tester) async {
      // Arrange: Setup tap callback tracking
      UserModel? tappedUser;
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserCard(
              user: testUser,
              onTap: (user) {
                tappedUser = user;
              },
            ),
          ),
        ),
      );
      
      // Act: Tap the user card
      await tester.tap(find.byType(Card));
      await tester.pumpAndSettle();
      
      // Assert: Verify tap callback was executed with correct user
      expect(tappedUser, equals(testUser));
    });
    
    testWidgets('should display placeholder when avatar URL is null', (WidgetTester tester) async {
      // Arrange: Create user without avatar
      final userWithoutAvatar = UserModel(
        id: '2',
        name: 'Jane Doe',
        email: 'jane@example.com',
        avatarUrl: null,
        role: 'user',
        createdAt: DateTime.now(),
      );
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: UserCard(user: userWithoutAvatar),
          ),
        ),
      );
      
      // Assert: Verify placeholder icon is displayed
      expect(find.byIcon(Icons.person), findsOneWidget);
    });
  });

  /// Test group for product card widget
  group('ProductCard Widget Tests', () {
    late ProductModel testProduct;
    
    setUp(() {
      // Create test product data
      testProduct = ProductModel(
        id: '1',
        name: 'Test Product',
        description: 'A great test product',
        price: 99.99,
        imageUrl: 'https://example.com/product.jpg',
        category: 'Electronics',
        inStock: true,
      );
    });
    
    testWidgets('should display product information correctly', (WidgetTester tester) async {
      // Arrange & Act: Create product card
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: ProductCard(product: testProduct),
          ),
        ),
      );
      
      // Assert: Verify product information is displayed
      expect(find.text(testProduct.name), findsOneWidget);
      expect(find.text(testProduct.description), findsOneWidget);
      expect(find.text('\${testProduct.price.toStringAsFixed(2)}'), findsOneWidget);
      expect(find.text(testProduct.category), findsOneWidget);
    });
    
    testWidgets('should show "In Stock" when product is available', (WidgetTester tester) async {
      // Arrange & Act: Create product card with in-stock product
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: ProductCard(product: testProduct),
          ),
        ),
      );
      
      // Assert: Verify stock status is displayed
      expect(find.text('In Stock'), findsOneWidget);
      expect(find.byIcon(Icons.check_circle), findsOneWidget);
    });
    
    testWidgets('should show "Out of Stock" when product is unavailable', (WidgetTester tester) async {
      // Arrange: Create out-of-stock product
      final outOfStockProduct = ProductModel(
        id: '2',
        name: 'Unavailable Product',
        description: 'This product is out of stock',
        price: 149.99,
        imageUrl: 'https://example.com/product2.jpg',
        category: 'Electronics',
        inStock: false,
      );
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: ProductCard(product: outOfStockProduct),
          ),
        ),
      );
      
      // Assert: Verify out-of-stock status is displayed
      expect(find.text('Out of Stock'), findsOneWidget);
      expect(find.byIcon(Icons.cancel), findsOneWidget);
    });
    
    testWidgets('should handle add to cart button press', (WidgetTester tester) async {
      // Arrange: Setup callback tracking
      ProductModel? addedProduct;
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: ProductCard(
              product: testProduct,
              onAddToCart: (product) {
                addedProduct = product;
              },
            ),
          ),
        ),
      );
      
      // Act: Tap add to cart button
      await tester.tap(find.text('Add to Cart'));
      await tester.pumpAndSettle();
      
      // Assert: Verify callback was executed with correct product
      expect(addedProduct, equals(testProduct));
    });
    
    testWidgets('should disable add to cart when out of stock', (WidgetTester tester) async {
      // Arrange: Create out-of-stock product
      final outOfStockProduct = testProduct.copyWith(inStock: false);
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: ProductCard(
              product: outOfStockProduct,
              onAddToCart: (product) {},
            ),
          ),
        ),
      );
      
      // Assert: Verify add to cart button is disabled
      final button = tester.widget<ElevatedButton>(
        find.widgetWithText(ElevatedButton, 'Add to Cart'),
      );
      expect(button.onPressed, isNull);
    });
  });

  /// Test group for provider integration
  group('Provider Integration Tests', () {
    late MockUserProvider mockUserProvider;
    late MockProductProvider mockProductProvider;
    late MockThemeProvider mockThemeProvider;
    
    setUp(() {
      // Initialize mock providers
      mockUserProvider = MockUserProvider();
      mockProductProvider = MockProductProvider();
      mockThemeProvider = MockThemeProvider();
      
      // Setup default mock behaviors
      when(mockUserProvider.isLoading).thenReturn(false);
      when(mockUserProvider.currentUser).thenReturn(null);
      when(mockProductProvider.isLoading).thenReturn(false);
      when(mockProductProvider.products).thenReturn([]);
      when(mockThemeProvider.isDarkMode).thenReturn(false);
    });
    
    testWidgets('should provide correct data to widgets', (WidgetTester tester) async {
      // Arrange: Setup providers with test data
      final testUser = UserModel(
        id: '1',
        name: 'Test User',
        email: 'test@example.com',
        avatarUrl: null,
        role: 'user',
        createdAt: DateTime.now(),
      );
      
      when(mockUserProvider.currentUser).thenReturn(testUser);
      
      await tester.pumpWidget(
        MultiProvider(
          providers: [
            ChangeNotifierProvider<UserProvider>.value(value: mockUserProvider),
            ChangeNotifierProvider<ProductProvider>.value(value: mockProductProvider),
            ChangeNotifierProvider<ThemeProvider>.value(value: mockThemeProvider),
          ],
          child: MaterialApp(
            home: Scaffold(
              body: Consumer<UserProvider>(
                builder: (context, userProvider, child) {
                  final user = userProvider.currentUser;
                  return user != null
                      ? Text('Hello, ${user.name}')
                      : Text('No user');
                },
              ),
            ),
          ),
        ),
      );
      
      // Assert: Verify provider data is used correctly
      expect(find.text('Hello, Test User'), findsOneWidget);
    });
    
    testWidgets('should handle provider state changes', (WidgetTester tester) async {
      // This test would verify that widgets update when provider state changes
      // Implementation depends on specific provider notification mechanisms
      
      // Arrange: Setup initial state
      when(mockUserProvider.isLoading).thenReturn(true);
      
      await tester.pumpWidget(
        MultiProvider(
          providers: [
            ChangeNotifierProvider<UserProvider>.value(value: mockUserProvider),
          ],
          child: MaterialApp(
            home: Scaffold(
              body: Consumer<UserProvider>(
                builder: (context, userProvider, child) {
                  return userProvider.isLoading
                      ? CircularProgressIndicator()
                      : Text('Content loaded');
                },
              ),
            ),
          ),
        ),
      );
      
      // Assert: Initially loading state
      expect(find.byType(CircularProgressIndicator), findsOneWidget);
      
      // Act: Change provider state
      when(mockUserProvider.isLoading).thenReturn(false);
      mockUserProvider.notifyListeners();
      await tester.pumpAndSettle();
      
      // Assert: State should be updated
      expect(find.text('Content loaded'), findsOneWidget);
      expect(find.byType(CircularProgressIndicator), findsNothing);
    });
  });

  /// Test group for accessibility
  group('Accessibility Tests', () {
    
    testWidgets('should have proper semantic labels', (WidgetTester tester) async {
      // Test that widgets have appropriate semantic labels for screen readers
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: Column(
              children: [
                Semantics(
                  label: 'Main navigation button',
                  child: ElevatedButton(
                    onPressed: () {},
                    child: Text('Navigate'),
                  ),
                ),
                Semantics(
                  label: 'User input field',
                  child: TextField(
                    decoration: InputDecoration(hintText: 'Enter text'),
                  ),
                ),
              ],
            ),
          ),
        ),
      );
      
      // Verify semantic labels are present
      expect(
        tester.getSemantics(find.text('Navigate')),
        matchesSemantics(label: 'Main navigation button'),
      );
    });
    
    testWidgets('should have proper contrast ratios', (WidgetTester tester) async {
      // This would test color contrast ratios for accessibility
      // Implementation would check if text colors have sufficient contrast
      // against their background colors
      
      await tester.pumpWidget(
        MaterialApp(
          theme: ThemeData.light(),
          home: Scaffold(
            body: Text(
              'Accessible text',
              style: TextStyle(
                color: Colors.black,
                backgroundColor: Colors.white,
              ),
            ),
          ),
        ),
      );
      
      // Verify text is rendered
      expect(find.text('Accessible text'), findsOneWidget);
    });
  });

  /// Test group for performance
  group('Performance Tests', () {
    
    testWidgets('should handle large lists efficiently', (WidgetTester tester) async {
      // Test widget performance with large datasets
      final largeProductList = List.generate(1000, (index) => 
        ProductModel(
          id: index.toString(),
          name: 'Product $index',
          description: 'Description for product $index',
          price: (index * 10.0),
          imageUrl: 'https://example.com/product$index.jpg',
          category: 'Category ${index % 5}',
          inStock: index % 2 == 0,
        ),
      );
      
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(
            body: ListView.builder(
              itemCount: largeProductList.length,
              itemBuilder: (context, index) {
                return ProductCard(product: largeProductList[index]);
              },
            ),
          ),
        ),
      );
      
      // Verify that the list renders without performance issues
      expect(find.byType(ListView), findsOneWidget);
      
      // Test scrolling performance
      await tester.drag(find.byType(ListView), const Offset(0, -500));
      await tester.pumpAndSettle();
      
      // Verify scroll completed successfully
      expect(find.byType(ProductCard), findsWidgets);
    });
  });
}
```

# Benefits of Flutter Project Structure: An Intuitive Analogy

Imagine building a Flutter project like organizing a large, bustling kitchen in a restaurant. Each file and folder in the project structure is like a specific area or tool in the kitchen, designed to make cooking (development) efficient, maintainable, and scalable. Below, we’ll explore the Flutter project structure, explain why files are separated this way, and provide an intuitive analogy with benefits for each separation.

---

## Project Structure Overview
The Flutter project structure is designed to promote **separation of concerns**, **modularity**, and **maintainability**. Each folder and file has a specific role, ensuring the codebase is organized, reusable, and easy to scale. Let’s break it down using the kitchen analogy.

---

## Analogy: The Restaurant Kitchen
Think of your Flutter project as a restaurant kitchen preparing a variety of dishes (features) for customers (users). The kitchen is divided into stations—prep area, cooking station, storage, serving area, and cleaning station—each with specific tools and ingredients. This organization ensures the chefs (developers) can work efficiently, avoid chaos, and deliver high-quality dishes (a polished app).

---

## File/Folder Breakdown and Benefits

### 1. `pubspec.yaml` - The Recipe Book
**Purpose**: Defines the project’s dependencies, assets, and configurations.  
**Analogy**: The recipe book lists all ingredients (dependencies like packages) and tools (fonts, images) needed to cook the dishes.  
**Benefits**:
- **Centralized Configuration**: Keeps all external dependencies and assets in one place, making it easy to manage and update.
- **Reusability**: Ensures the team knows exactly what’s needed to “cook” the app, avoiding missing ingredients.
- **Example**: Adding `http` package to `pubspec.yaml` is like adding olive oil to the recipe book—everyone knows it’s available for use.

### 2. `README.md` - The Kitchen Manual
**Purpose**: Documents the project’s purpose, setup, and usage.  
**Analogy**: The kitchen manual explains how the kitchen operates, from setup to cleaning procedures, so new chefs (developers) can get started quickly.  
**Benefits**:
- **Onboarding**: Helps new team members understand the project without digging through code.
- **Clarity**: Provides a high-level overview of the app’s purpose and structure.
- **Example**: A `README.md` explaining how to run the app is like a manual showing how to start the oven and where to find the pans.

### 3. `.gitignore` - The Cleaning Checklist
**Purpose**: Specifies which files (e.g., build artifacts, temporary files) Git should ignore.  
**Analogy**: The cleaning checklist tells the staff which items (like dirty dishes or scraps) to discard to keep the kitchen clean.  
**Benefits**:
- **Clean Repository**: Prevents unnecessary files from cluttering the version control system.
- **Efficiency**: Reduces repository size and avoids conflicts from auto-generated files.
- **Example**: Ignoring `build/` folder is like ensuring used napkins don’t end up in the storage room.

### 4. `assets/` - The Pantry
**Purpose**: Stores static resources like images, fonts, and translations.  
**Analogy**: The pantry holds ingredients (images, fonts) and pre-made sauces (translations) ready for use in dishes.  
**Benefits**:
- **Organization**: Keeps all static assets in one place, making them easy to access and manage.
- **Scalability**: Subfolders like `images/` or `translations/` allow for easy expansion as the app grows.
- **Example**: Storing `logo.png` in `assets/images/` is like keeping tomatoes in the pantry’s vegetable section—easy to find when needed.

### 5. `lib/` - The Cooking Stations
**Purpose**: Contains the core Dart code for the app, divided into logical modules.  
**Analogy**: The main kitchen area with specialized stations (prep, cooking, plating) where chefs create the dishes.  
**Benefits**:
- **Modularity**: Separates concerns (UI, logic, data) into distinct areas for better maintainability.
- **Scalability**: Makes it easy to add new features without disrupting existing code.
- **Example**: The `lib/` folder is like the kitchen’s main workspace, with stations for chopping (data), cooking (logic), and plating (UI).

#### 5.1 `lib/main.dart` - The Head Chef’s Station
**Purpose**: The entry point of the app, initializing the app’s core setup.  
**Analogy**: The head chef’s station where the cooking process starts, directing the team and setting the menu.  
**Benefits**:
- **Single Entry Point**: Provides a clear starting point for the app, reducing confusion.
- **Initialization**: Sets up the app’s core configuration (e.g., theme, routes).
- **Example**: `main.dart` calling `runApp(MyApp())` is like the head chef shouting “Start cooking!” to kick off service.

#### 5.2 `lib/app/` - The Kitchen Blueprint
**Purpose**: Contains app-level configurations and routing logic.  
**Analogy**: The kitchen blueprint outlines how stations connect and how dishes move from prep to serving.  
**Benefits**:
- **Centralized Navigation**: Keeps routing logic (`app_routes.dart`, `route_generator.dart`) in one place for consistent navigation.
- **Reusability**: Allows easy modification of app-wide settings in `app.dart`.
- **Example**: `route_generator.dart` defining paths is like a blueprint showing how to move plates from the prep station to the dining area.

#### 5.3 `lib/core/` - The Utility Station
**Purpose**: Holds reusable utilities, constants, and services.  
**Analogy**: The utility station with shared tools (knives, mixers) and ingredients (salt, pepper) used across dishes.  
**Benefits**:
- **Reusability**: Centralizes constants (`app_constants.dart`) and utilities (`validators.dart`, `extensions.dart`) to avoid duplication.
- **Consistency**: Ensures services like `api_service.dart` or `storage_service.dart` are accessible app-wide.
- **Example**: `api_constants.dart` storing API endpoints is like keeping a jar of salt that every chef can use.

#### 5.4 `lib/data/` - The Storage Room
**Purpose**: Manages data models, repositories, and state providers.  
**Analogy**: The storage room holds raw ingredients (data models) and prepped items (repositories) for cooking.  
**Benefits**:
- **Separation of Concerns**: Isolates data logic (`user_model.dart`, `user_repository.dart`) from UI and business logic.
- **Testability**: Makes it easier to test data-related logic independently.
- **Example**: `user_repository.dart` fetching user data is like a chef grabbing ingredients from the storage room.

#### 5.5 `lib/presentation/` - The Plating Station
**Purpose**: Contains UI-related code, including screens, widgets, and themes.  
**Analogy**: The plating station where dishes are arranged beautifully for customers.  
**Benefits**:
- **UI Isolation**: Separates UI (`home_screen.dart`, `custom_button.dart`) from business logic for cleaner code.
- **Reusability**: Widgets in `widgets/common/` (e.g., `custom_button.dart`) can be reused across screens.
- **Example**: `product_card.dart` is like a plating template for presenting a dish consistently across the menu.

#### 5.6 `lib/shared/` - The Shared Toolbox
**Purpose**: Stores enums and mixins used across the app.  
**Analogy**: The shared toolbox with universal tools (spoons, tongs) and labels (enums) used by all chefs.  
**Benefits**:
- **Consistency**: Enums like `app_state.dart` ensure consistent state management.
- **Reusability**: Mixins like `validation_mixin.dart` provide reusable functionality.
- **Example**: `user_role.dart` defining roles is like labeling chefs as “Sous Chef” or “Pastry Chef” for clarity.

### 6. `test/` - The Taste-Testing Station
**Purpose**: Contains unit and widget tests to ensure code quality.  
**Analogy**: The taste-testing station where dishes are checked for quality before serving.  
**Benefits**:
- **Reliability**: Tests (`home_screen_test.dart`) ensure features work as expected.
- **Maintainability**: Makes it easier to catch bugs when refactoring.
- **Example**: `widget_test.dart` testing a button is like tasting a sauce to ensure it’s not too salty.

### 7. Platform-Specific Folders (`android/`, `ios/`, etc.) - The Delivery Trucks
**Purpose**: Contains platform-specific configurations for Android, iOS, web, etc.  
**Analogy**: Delivery trucks customized to transport dishes to different locations (platforms).  
**Benefits**:
- **Platform Optimization**: Allows fine-tuning for each platform (e.g., Android’s `build.gradle`).
- **Isolation**: Keeps platform-specific code separate from the core Flutter code.
- **Example**: `android/` configuring permissions is like ensuring the truck has the right license for delivery.

---

## Why This Separation Matters
Just like a well-organized kitchen ensures chefs can work efficiently, serve high-quality dishes, and scale operations for a busy night, this Flutter project structure:
- **Promotes Collaboration**: Developers can work on specific areas (UI, data, services) without conflicts.
- **Enhances Maintainability**: Clear separation makes it easier to update or debug specific parts.
- **Supports Scalability**: New features (screens, models) can be added without restructuring the codebase.
- **Improves Testability**: Isolated components (e.g., repositories, widgets) are easier to test.

**Example in Action**: Imagine adding a new “Order History” feature. You’d:
- Add an `order_model.dart` in `data/models/` (new ingredient).
- Create an `order_repository.dart` in `data/repositories/` (prep the ingredient).
- Build an `order_screen.dart` in `presentation/views/screens/` (plate the dish).
- Update `route_generator.dart` in `app/routes/` (add to the delivery route).
This modular approach ensures the new feature integrates seamlessly without disrupting the existing codebase.

---

## Conclusion
The Flutter project structure is like a well-designed kitchen where every tool, ingredient, and station has a purpose. By separating files into `assets/`, `lib/`, `test/`, and platform-specific folders, the structure ensures the app is maintainable, scalable, and easy to collaborate on. Just as a restaurant delivers delicious meals through organization, this structure helps developers deliver a robust, user-friendly app.
