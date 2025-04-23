# ResQTrack Application Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Project Structure](#project-structure)
3. [Core Components](#core-components)
4. [Pages](#pages)
5. [Utility Functions](#utility-functions)
6. [API Services](#api-services)
7. [Authentication System](#authentication-system)
8. [Database Models](#database-models)
9. [Common Workflows](#common-workflows)
10. [Troubleshooting](#troubleshooting)

## Introduction

ResQTrack is a disaster management application designed for Maharashtra, India. It helps track, report, and manage disasters, authorities, and safe zones. The application has both user and admin interfaces with different permission levels.

### Key Features
- Disaster reporting and tracking
- Authority management
- Safe zone designation
- Weather information
- User authentication and authorization
- Interactive maps for location-based features

### Technology Stack
- **Frontend**: React, Material-UI, Tailwind CSS, Leaflet (maps)
- **Backend**: Node.js, Express
- **Database**: MongoDB
- **Authentication**: JWT (JSON Web Tokens)

## Project Structure

The project follows a standard React application structure with additional organization for the backend:

```
resqtrack/
├── src/                      # Frontend source code
│   ├── assets/               # Images and static assets
│   ├── components/           # Reusable UI components
│   ├── pages/                # Page components
│   ├── services/             
#API service functions
│   ├── utils/                # Utility functions
│   ├── App.jsx               # Main application component
│   └── main.jsx              # Application entry point
├── server/                   # Backend code
│   ├── controllers/          # Request handlers
│   ├── models/               # Database models
│   ├── routes/               
# API route definitions
│   ├── scripts/              # Utility scripts
│   └── server.js             # Server entry point
└── public/                   # Static files
```

## Core Components

### Navbar (`src/components/Navbar.jsx`)
The navigation bar component that appears on all pages. It provides links to different sections of the application and handles user authentication state. Recently converted from Material UI to Tailwind CSS for better performance.

**Key Functions:**

- `handleLogout()`: Handles user logout by clearing tokens and redirecting to login page
- `toggleDrawer()`: Controls mobile menu visibility with smooth animations
- `handleCloseNavMenu()`: Closes navigation menu after selection for better UX
- `isActive()`: Determines if a navigation item is active based on current route
- `getNavItems()`: Generates navigation items based on user role (admin/user)

**Key Features:**

- Responsive design with mobile-first approach
- Role-based navigation options (admin vs regular user)
- Transparent navbar with backdrop blur effect
- Active state indicators with hover effects
- Smooth transitions between states

### Hero (`src/components/Hero.jsx`)
A reusable hero section component used at the top of many pages. It displays a title, subtitle, and background image with parallax effect.

**Props:**

- `title`: Main heading text with support for HTML formatting
- `subtitle`: Secondary descriptive text with optional formatting
- `backgroundImage`: Image to display in the background (defaults to firefighter.jpg)
- `height`: Optional custom height for the hero section (default: 400px)
- `overlay`: Optional boolean to control gradient overlay (default: true)
- `textColor`: Optional text color override (default: white)

**Key Features:**

- Parallax scrolling effect for immersive experience
- Responsive text sizing across all device sizes
- Gradient overlay for better text readability
- Dynamic height adjustment
- Custom positioning of background image
- Optional CTA button with custom action

### ProtectedRoute (`src/components/ProtectedRoute.jsx`)
A higher-order component that restricts access to certain pages based on authentication status and user role.

**Key Functions:**
- `checkAuth()`: Verifies authentication status by checking JWT token validity
- `checkRole()`: Checks if user has required role (admin/user) for the requested route
- `handleRedirect()`: Redirects unauthorized users to the appropriate login page
- `renderFallback()`: Shows loading state while authentication is being verified

**Key Features:**
- Supports role-based access control (admin vs regular user)
- Prevents route access until authentication is confirmed
- Redirects expired sessions to login
- Preserves requested URL for post-login redirect
- Handles token refresh for extended sessions

### FallbackMap (`src/components/FallbackMap.jsx`)
A simplified map component used when the main map fails to load or when a minimal map display is needed. Serves as an error boundary fallback for the main map components.

**Props:**
- `center`: Map center coordinates as [latitude, longitude] array
- `zoom`: Initial zoom level (default: 7)
- `markers`: Array of marker positions to display with optional popup content
- `height`: Optional custom height (default: 400px)
- `width`: Optional custom width (default: 100%)
- `showControls`: Boolean to show/hide map controls (default: false)

**Key Features:**
- Lightweight alternative to full Leaflet map with minimal dependencies
- Static map display with customizable markers
- Error boundary integration for graceful failure handling
- Performance optimized with React.memo
- Accessibility support with keyboard navigation
- Fallback to static image when JavaScript is disabled

## Pages

### App (`src/App.jsx`)
The main entry point and router for the application. It sets up all routes and their corresponding components.

**Key Functions:**
- `Routes`: Defines all application routes
- `ProtectedRoute`: Wraps protected routes with authentication checks
- Sets up the main layout structure with Navbar
- Handles route-based code splitting for performance

### Main (`src/main.jsx`)
The application bootstrap file that renders the App component into the DOM.

**Key Features:**
- Sets up React StrictMode
- Configures BrowserRouter for routing
- Initializes the application theme
- Entry point for the entire application

### Login (`src/pages/Login.jsx`)
Handles user authentication with email and password using JWT authentication.

**Key Functions:**
- `handleSubmit()`: Processes login form submission with API integration
- `handleInputChange()`: Updates form state as user types with validation
- `validateForm()`: Performs client-side validation before submission
- `handleLoginSuccess()`: Stores user data and token in localStorage
- `handleLoginError()`: Displays appropriate error messages based on API response

**Key Features:**
- Form validation with real-time feedback
- Secure password handling
- JWT token storage and management
- Responsive design for all device sizes
- Redirect handling for protected routes

### AdminLogin (`src/pages/AdminLogin.jsx`)
Specialized login page for administrators with enhanced security measures.

**Key Functions:**
- `handleSubmit()`: Processes admin login with role verification
- `validateForm()`: Performs admin-specific validation with stricter requirements
- `checkAdminRole()`: Verifies admin role in JWT token payload
- `handleAdminLoginSuccess()`: Sets admin-specific session data
- `handleAdminLoginError()`: Provides specialized error handling for admin login attempts

**Key Features:**
- Enhanced security with role verification
- Stricter password requirements
- Rate limiting for failed attempts
- Secure redirect to admin dashboard
- No demo login option for admin accounts

### Register (`src/pages/Register.jsx`)
Allows new users to create an account with validation and API integration.

**Key Functions:**
- `handleSubmit()`: Processes registration form submission
- `validateForm()`: Performs comprehensive form validation
- `handleInputChange()`: Updates form state with validation
- `checkEmailAvailability()`: Verifies email is not already registered
- `handleRegistrationSuccess()`: Redirects to login page after successful registration

**Key Features:**
- Password strength validation
- Email format verification
- Confirm password matching
- Optional location information collection
- Terms and conditions acceptance

### ReportDisaster (`src/pages/ReportDisaster.jsx`)
Allows users to report new disasters with location, type, severity, and optional images.

**Key Functions:**
- `handleSubmit()`: Processes disaster report submission with data transformation
- `handleMapClick()`: Captures location coordinates from map with reverse geocoding
- `validateForm()`: Ensures all required fields are properly filled
- `handleImageUpload()`: Processes and validates disaster images
- `transformData()`: Converts form data to match API requirements
- `handleLocationSearch()`: Allows searching for specific locations

**Key Features:**
- Interactive map for precise location selection
- Disaster type categorization with icons
- Severity level selection with visual indicators
- Multiple image upload support
- Location name auto-completion
- Form data persistence during session

### ViewDisasters (`src/pages/ViewDisasters.jsx`)
Displays active disasters on an interactive map and in a list view with filtering options.

**Key Functions:**
- `fetchData()`: Retrieves disaster, authority, and safe zone data in parallel
- `handleMarkerClick()`: Shows detailed information when a disaster marker is clicked
- `toggleLayer()`: Controls visibility of different map layers (disasters, authorities, safe zones)
- `getSeverityColor()`: Determines color coding based on disaster severity
- `getAuthorityIcon()`: Creates custom icons for different authority types
- `handleUpdateStatus()`: Allows admins to update disaster status
- `handleDeleteDisaster()`: Provides admin functionality to remove disasters
- `handleCloseDialog()`: Manages the disaster details dialog

**Key Features:**
- Interactive map with custom markers and popups
- Layer controls for toggling different data types
- List view with disaster cards for easy browsing
- Color-coded severity indicators
- Admin controls for disaster management
- Responsive design for all screen sizes
- Real-time data updates

### DisasterHistory (`src/pages/DisasterHistory.jsx`)
Shows a history of all reported disasters, including resolved ones with comprehensive filtering options.

**Key Functions:**
- `fetchHistoricalDisasters()`: Gets historical disaster data with pagination
- `handleFilterChange()`: Updates filter criteria for disaster history
- `handleDateRangeChange()`: Filters disasters by specific date ranges
- `handleSortChange()`: Changes the sort order of displayed disasters
- `handleViewDetails()`: Shows detailed information about a historical disaster

**Key Features:**
- Advanced filtering by date, type, severity, and location
- Sortable data table with multiple column options
- Detailed view of historical disaster information
- Export functionality for disaster reports
- Timeline visualization of disaster frequency
- Table view with sorting and filtering options

### DisasterDetails (`src/pages/DisasterDetails.jsx`)
Detailed view of a specific disaster with all related information.

**Key Functions:**
- `fetchDisasterDetails()`: Gets comprehensive disaster data
- `fetchNearbyAuthorities()`: Finds authorities near the disaster
- `fetchSafeZones()`: Gets safe zones near the disaster
- `handleStatusChange()`: Updates disaster status
- `handleNotifyAuthority()`: Sends notifications to authorities
- Interactive map showing disaster location
- Tabs for authorities and safe zones

### WeatherInfo (`src/pages/WeatherInfo.jsx`)
Provides comprehensive weather information for locations in Maharashtra with fallback handling for unrecognized locations.

**Key Functions:**
- `fetchWeatherData()`: Gets weather data for a specified city using OpenWeatherMap API
- `handleSearch()`: Processes city search with input validation
- `normalizeLocationName()`: Handles Marathi location names by mapping to English equivalents
- `getFallbackLocation()`: Provides nearest major city when smaller towns aren't found
- `handleLocationError()`: Manages API errors with appropriate fallback
- `renderForecast()`: Displays 5-day weather forecast with daily breakdown

**Key Features:**
- Current weather conditions with visual indicators
- 5-day weather forecast with temperature trends
- Location search with auto-suggestions for Maharashtra cities
- Quick access buttons for popular cities
- Fallback system for unrecognized locations
- Weather map visualization
- Temperature unit conversion (Celsius/Fahrenheit)

### SafeZones (`src/pages/SafeZones.jsx`)
Manages safe zone locations and information with capacity tracking and proximity filtering.

**Key Functions:**
- `fetchSafeZones()`: Gets all safe zones with optional proximity filtering
- `handleAddSafeZone()`: Processes new safe zone creation with validation
- `handleEditSafeZone()`: Updates existing safe zone information
- `handleDeleteSafeZone()`: Removes a safe zone with confirmation
- `handleMapClick()`: Captures location coordinates for new safe zone
- `handleCapacityChange()`: Updates safe zone capacity and occupancy
- `calculateDistance()`: Determines distance between user location and safe zones

**Key Features:**
- Interactive map for safe zone visualization and selection
- Form for adding/editing safe zone details
- Capacity tracking with visual indicators
- Distance-based filtering from current location
- List view with sorting and filtering options
- Detailed information cards for each safe zone
- Admin controls for managing safe zones

### AuthorityManagement (`src/pages/AuthorityManagement.jsx`)
Admin interface for managing emergency response authorities with comprehensive CRUD operations.

**Key Functions:**

- `fetchAuthorities()`: Gets all registered authorities with type and area filtering
- `handleSubmit()`: Processes authority form submission with validation
- `handleEdit()`: Prepares form for editing existing authority data
- `handleDelete()`: Removes an authority with confirmation and dependency checking
- `handleMapClick()`: Captures location coordinates for new or edited authority
- `validateForm()`: Ensures all required fields are properly filled
- `handleTypeChange()`: Updates form fields based on selected authority type

**Key Features:**

- Table view with sorting, filtering, and pagination
- Interactive map showing authority locations with type-specific markers
- Form for adding/editing authority details with validation
- Authority type categorization (police, fire, hospital, etc.)
- Contact information management
- Jurisdiction area definition
- Bulk import/export capabilities selection

### AdminDashboard (`src/pages/AdminDashboard.jsx`)
Provides comprehensive administrative overview and management functions with real-time statistics.

**Key Functions:**

- `fetchStats()`: Gets statistical data for dashboard with category breakdowns
- `fetchPendingDisasters()`: Retrieves disasters awaiting approval
- `handleApprove()`: Approves a pending disaster with notification to authorities
- `handleReject()`: Rejects a pending disaster with reason documentation
- `generateReport()`: Creates downloadable reports in various formats
- `handleUserManagement()`: Provides access to user management functions
- `refreshData()`: Updates dashboard data in real-time

**Key Features:**

- Statistical cards showing disaster counts by type and status
- Interactive charts visualizing disaster trends over time
- Pending disaster approval queue with detailed information
- User management section for admin accounts
- System health monitoring indicators
- Export functionality for reports and statistics
- Quick access links to all management functions

### UserProfile (`src/pages/UserProfile.jsx`)
Allows users to view and edit their profile information with secure data handling.

**Key Functions:**

- `fetchUserData()`: Gets current user profile from API with authentication
- `handleSubmit()`: Processes profile update with field validation
- `handlePasswordChange()`: Manages secure password updates
- `handleImageUpload()`: Processes and validates profile image uploads
- `validateForm()`: Performs comprehensive form validation
- `handleDeleteAccount()`: Provides account deactivation functionality

**Key Features:**

- Personal information management (name, email, phone)
- Secure password changing mechanism
- Profile picture upload and management
- Location information updates
- Activity history showing reported disasters
- Account settings and preferences
- Data privacy controls

## Utility Functions

### authorityUtils.js (`src/utils/authorityUtils.js`)
Functions for managing authority data with proper data transformation and API integration.

This utility module handles all authority-related operations including data fetching, transformation, and state management. It provides a clean interface between the frontend components and the backend API.

**Key Functions:**
- `getAuthorities()`: Gets authorities near a location with distance filtering
- `getAllAuthorities()`: Gets all authorities without filtering
- `addAuthority()`: Creates a new authority with proper location formatting
- `updateAuthority()`: Updates an existing authority
- `deleteAuthority()`: Removes an authority
- `notifyAuthorities()`: Sends notifications to authorities about disasters

**Key Constants:**

- `AUTHORITY_TYPES`: Object defining all authority types with labels, icons, and color codes
- `AUTHORITY_JURISDICTIONS`: Predefined jurisdiction areas for different authority types
- `AUTHORITY_CONTACT_TEMPLATES`: Templates for authority contact information
- `MAX_NOTIFICATION_RADIUS`: Maximum radius for authority notifications (in kilometers)
- `AUTHORITY_ICONS`: Mapping of authority types to their respective icon components and colors

### disasterUtils.js (`src/utils/disasterUtils.js`)
Functions for managing disaster data with proper data transformation and API integration.

This utility module handles all disaster-related operations including reporting, status management, and historical tracking. It serves as the primary interface for disaster management features throughout the application.

**Key Functions:**

- `transformData(formData)`: Transforms form data to match MongoDB schema requirements with proper formatting
- `getDisasterById(id)`: Retrieves a specific disaster by ID with complete details including notifications
- `getAllDisasters(filters)`: Gets all active disasters with comprehensive filtering options
- `getHistoricalDisasters(dateRange)`: Retrieves resolved disasters with optional date range filtering
- `getDisastersNearby(lat, lng, radius)`: Finds disasters within a specified radius using geospatial queries
- `saveDisaster(disasterData)`: Creates a new disaster report with extensive validation and error handling
- `updateDisasterStatus(id, status, notes)`: Changes a disaster's status with timestamp tracking and optional notes
- `deleteDisaster(id)`: Removes a disaster with proper authorization checks
- `notifyAuthorities(disasterId)`: Automatically notifies relevant authorities about a disaster based on type and location
- `getPendingDisasters()`: Retrieves disasters awaiting admin approval
- `approveDisaster(id)`: Admin function to approve pending disaster reports
- `rejectDisaster(id, reason)`: Admin function to reject disaster reports with reason documentation
- `getDisasterStatistics(filters)`: Generates statistical data about disasters for reporting

### safeZoneUtils.js (`src/utils/safeZoneUtils.js`)
Functions for managing safe zone data with distance calculations and capacity tracking.

This utility module provides comprehensive functionality for safe zone management, including proximity searches, capacity tracking, and integration with disaster response workflows.

**Key Functions:**

- `getSafeZones(lat, lng, radius)`: Retrieves safe zones near a location with distance filtering and sorting by proximity
- `getAllSafeZones(filters)`: Gets all safe zones with optional filtering by capacity, status, and type
- `getSafeZoneById(id)`: Fetches detailed information about a specific safe zone including current occupancy
- `getSafeZonesByDisaster(disasterId)`: Finds safe zones associated with a specific disaster
- `addSafeZone(safeZoneData)`: Creates a new safe zone with proper location formatting and validation
- `updateSafeZone(id, safeZoneData)`: Updates an existing safe zone with change tracking
- `deleteSafeZone(id)`: Removes a safe zone with occupancy verification
- `updateSafeZoneOccupancy(id, count)`: Updates the current occupancy of a safe zone with capacity validation
- `resetSafeZoneOccupancy(id)`: Resets occupancy count to zero for a safe zone
- `calculateDistance(lat1, lng1, lat2, lng2)`: Calculates distance between coordinates using Haversine formula
### weatherUtils.js (`src/utils/weatherUtils.js`)
Functions for fetching and processing weather data with location normalization and fallback handling.

This utility module provides robust weather data retrieval with special handling for Maharashtra locations, including translation of Marathi location names and fallback mechanisms for unrecognized locations.

**Key Functions:**

- `fetchWeatherData(city)`: Retrieves weather data from OpenWeatherMap API with error handling
- `normalizeLocationName(name)`: Translates Marathi location names to their English equivalents
- `getFallbackLocation(name)`: Provides nearest major city if the specified location isn't found
- `getWeatherIcon(code)`: Maps weather condition codes to appropriate icon components
- `convertTemperature(temp, unit)`: Converts temperature between Celsius and Fahrenheit
- `formatWeatherData(data)`: Transforms API response into a consistent format for the UI
- `getPopularCities()`: Returns a list of major Maharashtra cities for quick selection
- `parseWeatherForecast(data)`: Processes 5-day forecast data into daily summaries
- `getWeatherDescription(code, language)`: Provides weather descriptions in English or Marathi

**Key Features:**
- OpenWeatherMap API integration
- Comprehensive location name mapping for Maharashtra
- Error handling with fallback to major cities
- Temperature unit conversion

### mapUtils.js (`src/utils/mapUtils.js`)
Helper functions for working with maps and coordinates with Maharashtra-specific configurations.

This utility module provides geographic calculations and map configuration specifically optimized for Maharashtra region, including boundary definitions, distance calculations, and coordinate validation.

**Key Functions:**

- `calculateDistance(lat1, lng1, lat2, lng2)`: Calculates distance between coordinates using Haversine formula with kilometer output
- `getMapBounds()`: Returns appropriate map boundaries for Maharashtra region
- `formatCoordinates(coords, format)`: Formats coordinates for API requests in various formats (array, object, string)
- `isWithinMaharashtra(lat, lng)`: Validates if coordinates are within Maharashtra state boundaries
- `reverseGeocode(lat, lng)`: Converts coordinates to location name using third-party services
- `getMapCenter()`: Returns the geographic center of Maharashtra (19.7515°N, 75.7139°E)
- `getOptimalZoom(boundingBox)`: Calculates the optimal zoom level for a given bounding box
- `createBoundingBox(center, radiusKm)`: Generates a bounding box around a center point with specified radius
- `getDistrictBoundaries(districtName)`: Returns GeoJSON boundaries for specific Maharashtra districts
- `getDistrictFromCoordinates()`: Attempts to determine district from coordinates

**Key Constants:**
- Maharashtra boundaries and center coordinates
- Default zoom levels for different views
- District boundary definitions

### leafletUtils.js (`src/utils/leafletUtils.js`)
Custom configurations and extensions for Leaflet maps with specialized markers and controls.

This utility module extends Leaflet functionality with custom markers, controls, and configurations specifically designed for disaster management visualization needs.

**Key Functions:**

- `createCustomMarker(type, options)`: Creates custom markers with type-specific styling (disaster, authority, safe zone)
- `createCustomIcon(type, severity)`: Generates icon objects with appropriate styling based on entity type and severity
- `createPopupContent(entity, type)`: Formats popup content with consistent styling and information display
- `initializeMap(container, options)`: Sets up a Leaflet map with Maharashtra-specific configuration
- `addLayerControls(map, layers)`: Adds custom layer controls for toggling different data types
- `createClusterGroup(markers, options)`: Groups markers into clusters for better performance with large datasets
- `addGeoJSONLayer(map, data, options)`: Adds GeoJSON data to map with styling and interaction handlers
- `createHeatmapLayer(points, options)`: Generates heatmap visualization for disaster density
- `fixLeafletIcon()`: Patches Leaflet icon paths for proper loading in various environments

## API Services

### api.js (`src/services/api.js`)
Centralized API service for making HTTP requests to the backend with authentication, error handling, and response transformation.

This service module provides a unified interface for all backend API interactions, handling authentication, request formatting, error processing, and response transformation.

**Core Configuration:**

- `apiClient`: Axios instance configured with base URL, timeout settings, and interceptors
- `authInterceptor`: Automatically adds authentication tokens to requests
- `errorInterceptor`: Processes API errors with consistent handling and retry logic
- `responseTransformer`: Standardizes API responses for consistent data structure

**Service Modules:**

- **disasterService**:
  - `getAll(filters)`: Retrieves disasters with comprehensive filtering options
  - `getNearby(lat, lng, radius)`: Finds disasters within specified radius
  - `getById(id)`: Fetches detailed disaster information by ID
  - `getHistorical(filters)`: Retrieves resolved disasters with filtering
  - `create(data)`: Creates new disaster report with validation
  - `update(id, data)`: Updates existing disaster information
  - `updateStatus(id, status)`: Changes disaster status with tracking
  - `delete(id)`: Removes a disaster with proper authorization
  - `getPending()`: Gets disasters awaiting approval
  - `approve(id)`: Approves pending disaster
  - `reject(id, reason)`: Rejects disaster with reason
  - `getStats(filters)`: Retrieves statistical data

- **authorityService**:
  - `getAll(filters)`: Gets authorities with type/area filtering
  - `getByArea(area)`: Retrieves authorities in specific area
  - `getNearby(lat, lng, radius)`: Finds authorities within radius
  - `getById(id)`: Gets detailed authority information
  - `getByType(type)`: Filters authorities by type
  - `create(data)`: Creates new authority with validation
  - `update(id, data)`: Updates authority information
  - `delete(id)`: Removes authority with dependency checking
  - `notify(authorityId, disasterId)`: Sends disaster notification

- **safeZoneService**:
  - `getAll(filters)`: Gets safe zones with capacity/status filtering
  - `getNearby(lat, lng, radius)`: Finds safe zones within radius
  - `getByDisaster(disasterId)`: Gets safe zones for specific disaster
  - `getById(id)`: Retrieves detailed safe zone information
  - `create(data)`: Creates new safe zone with validation
  - `update(id, data)`: Updates safe zone information
  - `updateOccupancy(id, count)`: Modifies current occupancy
  - `delete(id)`: Removes safe zone with verification

- **userService**:
  - `register(data)`: Creates new user account
  - `login(credentials)`: Authenticates user and returns token
  - `logout()`: Invalidates current session
  - `getProfile()`: Gets current user information
  - `updateProfile(data)`: Updates user profile
  - `changePassword(data)`: Securely changes password
  - `getAllUsers(filters)`: Admin function to list users
  - `getUserById(id)`: Gets specific user details
  - `updateUser(id, data)`: Admin function to update user
  - `deleteUser(id)`: Permanently removes user account

- **statsService**:
  - `getStats(filters)`: Retrieves comprehensive dashboard statistics
  - `getDisasterStats(filters)`: Gets disaster-specific statistics
  - `getAuthorityStats()`: Retrieves authority distribution data
  - `getSafeZoneStats()`: Gets safe zone capacity statistics
  - `getUserStats()`: Provides user activity metrics

**Authentication Handling:**
- Token management in request headers with Bearer authentication
- Request interceptors for attaching tokens
- Response error handling with detailed logging
- Token refresh mechanism for extended sessions
- Comprehensive error handling for authentication failures
- Role-based request validation
- Session timeout management
- Secure logout process with token invalidation

### JWT Implementation
The application uses JSON Web Tokens (JWT) for authentication.

**Key Components:**
- Token generation on login/registration
- Token storage with secure practices
- Automatic token inclusion in request headers via interceptors
- Token refresh mechanism for extended sessions
- Comprehensive error handling for authentication failures
- Role-based request validation
- Session timeout management
- Secure logout process with token invalidation

### User Roles
The system supports different user roles with varying permissions:

- **Regular Users**: Can report disasters, view information
- **Admins**: Can manage authorities, approve disasters, access all features

## Server-Side Components

### Server Entry Point (`server/server.js`)
The main entry point for the Express server application.

**Key Functions:**
- Server initialization and configuration
- Database connection setup
- Middleware configuration (CORS, body parsing, error handling)
- Route registration
- Error handling
- Server startup and port configuration

### Controllers

#### Authentication Controller (`server/controllers/authController.js`)
Handles user authentication, registration, and profile management.

**Helper Functions:**
- `signToken(id)`: Generates JWT token with user ID using environment variables for secret and expiration
- `createSendToken(user, statusCode, res)`: Creates token, removes password from response, and sends user data
- `filterObj(obj, ...allowedFields)`: Filters object to only include specified fields

**Main Functions:**
- `signup(req, res)`: Registers a new user with validation, prevents unauthorized admin creation
- `login(req, res)`: Authenticates a user with email and password, validates credentials
- `protect(req, res, next)`: Middleware to protect routes requiring authentication, verifies JWT token
- `restrictTo(...roles)`: Middleware to restrict access based on user role, returns 403 for unauthorized access
- `getMe(req, res, next)`: Gets current user profile by setting params ID to user ID
- `updateMe(req, res)`: Updates current user profile with filtered fields for security
- `deleteMe(req, res)`: Deactivates current user account by setting active flag to false
- `getAllUsers(req, res)`: Admin only - gets all users
- `getUser(req, res)`: Admin only - gets user by ID
- `updateUser(req, res)`: Admin only - updates any user
- `deleteUser(req, res)`: Admin only - deletes any user

#### Disaster Controller (`server/controllers/disasterController.js`)
Handles disaster data operations including reporting, status management, and geospatial queries.

**CRUD Functions:**
- `getAllDisasters(req, res)`: Gets all disasters with optional filtering by type, severity, and status
- `getDisaster(req, res)`: Gets a specific disaster by ID with populated authority notifications
- `createDisaster(req, res)`: Creates a new disaster report with location validation and proper formatting
- `updateDisaster(req, res)`: Updates an existing disaster with permission checks and validation
- `deleteDisaster(req, res)`: Deletes a disaster with permission checks and cascade deletion of related data

**Specialized Functions:**
- `getDisastersNearby(req, res)`: Gets disasters near a location using geospatial queries with configurable radius
- `getHistoricalDisasters(req, res)`: Gets resolved disasters with filtering options and pagination
- `updateDisasterStatus(req, res)`: Updates disaster status (active/resolved/pending) with timestamp tracking
- `notifyAuthority(req, res)`: Notifies authorities about a disaster and tracks notification status
- `getDisasterStats(req, res)`: Gets disaster statistics for admin dashboard including counts by type and severity
- `approveDisaster(req, res)`: Admin function to approve pending disaster reports
- `rejectDisaster(req, res)`: Admin function to reject pending disaster reports with reason

#### Authority Controller (`server/controllers/authorityController.js`)
Handles authority data operations including registration, management, and geospatial queries.

**CRUD Functions:**
- `getAllAuthorities(req, res)`: Gets all authorities with optional filtering by type and area
- `getAuthority(req, res)`: Gets a specific authority by ID with proper error handling
- `createAuthority(req, res)`: Creates a new authority with location validation and type verification
- `updateAuthority(req, res)`: Updates an existing authority with field validation
- `deleteAuthority(req, res)`: Deletes an authority with checks for related disasters

**Specialized Functions:**
- `getAuthoritiesNearby(req, res)`: Gets authorities near a location using geospatial queries with radius parameter
- `getAuthoritiesByArea(req, res)`: Gets authorities by administrative area with pagination
- `getAuthorityByType(req, res)`: Gets authorities filtered by type with sorting options
- `notifyAuthority(req, res)`: Sends notification to an authority about a disaster and tracks notification status
- `getAuthorityStats(req, res)`: Gets statistics about authorities by type and region
- `seedAuthorities(req, res)`: Admin function to seed initial authority data for testing

#### Safe Zone Controller (`server/controllers/safeZoneController.js`)
Handles safe zone data operations including capacity management and proximity searches.

**CRUD Functions:**
- `getAllSafeZones(req, res)`: Gets all safe zones with optional filtering by capacity and status
- `getSafeZone(req, res)`: Gets a specific safe zone by ID with capacity information
- `createSafeZone(req, res)`: Creates a new safe zone with location validation and capacity checks
- `updateSafeZone(req, res)`: Updates an existing safe zone with occupancy validation
- `deleteSafeZone(req, res)`: Deletes a safe zone with verification of current occupancy

**Specialized Functions:**
- `getSafeZonesNearby(req, res)`: Gets safe zones near a location with distance calculation and sorting
- `getSafeZonesByDisaster(req, res)`: Gets safe zones associated with a specific disaster
- `getAvailableCapacity(req, res)`: Gets safe zones with available capacity and occupancy percentage
- `updateSafeZoneOccupancy(req, res)`: Updates the current occupancy of a safe zone with capacity validation
- `resetSafeZoneOccupancy(req, res)`: Resets occupancy count to zero for a safe zone
- `getSafeZoneCapacityStats(req, res)`: Gets statistics about safe zone capacity and utilization
- `markSafeZoneUnavailable(req, res)`: Marks a safe zone as unavailable in emergency situations

### Routes

#### User Routes (`server/routes/userRoutes.js`)
Defines API endpoints for user authentication and management.

**Public Routes:**
- `POST /api/users/signup`: Register a new user
- `POST /api/users/login`: Authenticate user

**Protected Routes:**
- `GET /api/users/me`: Get current user profile
- `PATCH /api/users/updateMe`: Update current user profile
- `DELETE /api/users/deleteMe`: Deactivate current user account

**Admin Routes:**
- `GET /api/users`: Get all users
- `GET /api/users/:id`: Get user by ID
- `PATCH /api/users/:id`: Update any user
- `DELETE /api/users/:id`: Delete any user

#### Disaster Routes (`server/routes/disasterRoutes.js`)
Defines API endpoints for disaster operations.

**Public Routes:**
- `GET /api/disasters`: Get all disasters
- `GET /api/disasters/:id`: Get disaster by ID

**Protected Routes:**
- `POST /api/disasters`: Create new disaster
- `PATCH /api/disasters/:id`: Update disaster
- `DELETE /api/disasters/:id`: Delete disaster
- `PATCH /api/disasters/:id/status`: Update disaster status
- `POST /api/disasters/notify`: Notify authority about disaster

**Specialized Routes:**
- `GET /api/disasters/nearby`: Get disasters near location
- `GET /api/disasters/historical`: Get historical disasters
- `GET /api/disasters/stats`: Get disaster statistics

#### Authority Routes (`server/routes/authorityRoutes.js`)
Defines API endpoints for authority operations.

**Public Routes:**
- `GET /api/authorities`: Get all authorities
- `GET /api/authorities/:id`: Get authority by ID
- `GET /api/authorities/nearby`: Get authorities near location
- `GET /api/authorities/area`: Get authorities by area

**Admin Routes:**
- `POST /api/authorities`: Create new authority
- `PATCH /api/authorities/:id`: Update authority
- `DELETE /api/authorities/:id`: Delete authority

#### Safe Zone Routes (`server/routes/safeZoneRoutes.js`)
Defines API endpoints for safe zone operations.

**Public Routes:**
- `GET /api/safezones`: Get all safe zones
- `GET /api/safezones/:id`: Get safe zone by ID
- `GET /api/safezones/nearby`: Get safe zones near location
- `GET /api/safezones/disaster/:id`: Get safe zones for disaster

**Admin Routes:**
- `POST /api/safezones`: Create new safe zone
- `PATCH /api/safezones/:id`: Update safe zone
- `DELETE /api/safezones/:id`: Delete safe zone

### Scripts

#### Seed Admin (`server/scripts/seedAdmin.js`)
Script to create an initial admin user in the database.

**Key Functions:**
- Connects to database
- Checks if admin exists
- Creates admin user with predefined credentials

#### Seed Authorities (`server/scripts/seedAuthorities.js`)
Script to populate the database with initial authority data.

**Key Functions:**
- Connects to database
- Defines sample authority data
- Inserts authorities into database

#### Update Disaster Status (`server/scripts/updateDisasterStatus.js`)
Script to automatically update disaster status based on time elapsed.

**Key Functions:**
- Connects to database
- Finds disasters older than specified threshold
- Updates status to 'resolved' for old disasters

## Database Models

### User Model (`server/models/userModel.js`)
Stores user account information with authentication methods.

**Schema Definition:**
```javascript
const userSchema = new mongoose.Schema({
  name: { type: String, required: [true, 'Please provide your name'] },
  email: { type: String, required: [true, 'Please provide your email'], unique: true, lowercase: true },
  password: { type: String, required: [true, 'Please provide a password'], minlength: 8, select: false },
  role: { type: String, enum: ['user', 'admin'], default: 'user' },
  location: { type: [Number], default: [73.8567, 18.5204] }, // [longitude, latitude]
  address: { type: String },
  phone: { type: String },
  profilePicture: { type: String },
  active: { type: Boolean, default: true, select: false },
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date }
});
```

**Methods:**
- `userSchema.pre('save')`: Middleware to hash password before saving
- `userSchema.methods.matchPassword(candidatePassword)`: Compares provided password with stored hash
- `userSchema.methods.generateToken()`: Creates JWT authentication token
- `userSchema.pre(/^find/)`: Middleware to filter out inactive users

**Indexes:**
- Email field is indexed for faster lookups
- Location field has geospatial index for proximity queries

### Disaster Model (`server/models/disasterModel.js`)
Stores disaster information with geospatial capabilities.

**Schema Definition:**
```javascript
const disasterSchema = new mongoose.Schema({
  disasterType: { 
    type: String, 
    required: [true, 'A disaster must have a type'],
    enum: ['earthquake', 'flood', 'fire', 'cyclone', 'landslide', 'other']
  },
  severity: { 
    type: String, 
    required: [true, 'A disaster must have a severity level'],
    enum: ['low', 'medium', 'high', 'critical'],
    default: 'medium'
  },
  location: {
    type: [Number], // [longitude, latitude]
    required: [true, 'A disaster must have a location']
  },
  locationName: {
    type: String,
    required: [true, 'Please provide a location name']
  },
  description: {
    type: String,
    required: [true, 'Please provide a description']
  },
  reportedBy: {
    type: mongoose.Schema.ObjectId,
    ref: 'User',
    required: [true, 'A disaster must have a reporter']
  },
  status: {
    type: String,
    enum: ['active', 'resolved'],
    default: 'active'
  },
  images: [String],
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: Date
}, {
  toJSON: { virtuals: true },
  toObject: { virtuals: true }
});
```

**Indexes:**
- Location field has geospatial index for proximity queries
- Compound index on disasterType and status for filtered queries

**Virtual Properties:**
- `safeZones`: Virtual populate of related safe zones

**Middleware:**
- `disasterSchema.pre(/^find/)`: Populates reporter information
- `disasterSchema.pre('save')`: Sets updatedAt timestamp

### Authority Model (`server/models/authorityModel.js`)
Stores emergency response authority information with geospatial capabilities.

**Schema Definition:**
```javascript
const authoritySchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'An authority must have a name'],
    trim: true
  },
  type: {
    type: String,
    required: [true, 'An authority must have a type'],
    enum: ['police', 'fire', 'hospital', 'municipality', 'health', 'electricity', 'transport', 'other'],
    default: 'other'
  },
  location: {
    type: [Number], // [longitude, latitude]
    required: [true, 'An authority must have a location']
  },
  address: {
    type: String,
    required: [true, 'An authority must have an address']
  },
  contactNumber: {
    type: String,
    required: [true, 'An authority must have a contact number']
  },
  district: {
    type: String,
    required: [true, 'An authority must have a district']
  },
  division: String,
  taluka: String,
  jurisdiction: {
    type: Number, // radius in kilometers
    default: 10
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: Date
});
```

**Indexes:**
- Location field has geospatial index for proximity queries
- Compound index on type and district for filtered queries

**Middleware:**
- `authoritySchema.pre('save')`: Sets updatedAt timestamp
- `authoritySchema.pre('findOneAndUpdate')`: Updates updatedAt timestamp

**Static Methods:**
- `authoritySchema.statics.findNearby(coordinates, maxDistance)`: Finds authorities within radius

### SafeZone Model (`server/models/safeZoneModel.js`)
Stores safe zone information with geospatial capabilities and capacity tracking.

**Schema Definition:**
```javascript
const safeZoneSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'A safe zone must have a name'],
    trim: true
  },
  location: {
    type: [Number], // [longitude, latitude]
    required: [true, 'A safe zone must have a location']
  },
  description: {
    type: String,
    required: [true, 'A safe zone must have a description']
  },
  capacity: {
    type: Number,
    required: [true, 'A safe zone must have a capacity']
  },
  currentOccupancy: {
    type: Number,
    default: 0
  },
  facilities: [String],
  contactPerson: String,
  contactNumber: String,
  disasterId: {
    type: mongoose.Schema.ObjectId,
    ref: 'Disaster'
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: Date
});
```

**Indexes:**
- Location field has geospatial index for proximity queries
- Index on disasterId for faster lookup of disaster-specific safe zones

**Virtual Properties:**
- `availableCapacity`: Calculated as capacity - currentOccupancy
- `isAtCapacity`: Boolean indicating if safe zone is full

**Methods:**
- `safeZoneSchema.methods.updateOccupancy(count)`: Updates current occupancy
- `safeZoneSchema.methods.resetOccupancy()`: Resets occupancy to zero

**Middleware:**
- `safeZoneSchema.pre(/^find/)`: Populates associated disaster if requested
- `safeZoneSchema.pre('save')`: Validates occupancy doesn't exceed capacity

## Common Workflows

### Disaster Reporting Workflow
1. User navigates to Report Disaster page
2. Selects disaster location on map (clicks on map to set coordinates)
3. Fills out disaster details form (type, severity, description)
4. Optionally uploads images of the disaster
5. Submits report
6. System validates and transforms data to match schema
7. System saves disaster with "active" status
8. Disaster appears on View Disasters page
9. Nearby authorities are notified automatically

### Authority Management Workflow (Admin)
1. Admin navigates to Authority Management page
2. Views existing authorities in table
3. Clicks "Add Authority" button
4. Fills out authority details and selects location
5. Submits form
6. New authority is added to the system

### Safe Zone Creation Workflow
1. User navigates to Safe Zones page
2. Clicks "Add Safe Zone" button
3. Selects location on map
4. Fills out safe zone details
5. Submits form
6. New safe zone is added to the system

### User Registration and Login Workflow
1. User navigates to Register page
2. Fills out registration form
3. Submits form to create account
4. System redirects to Login page
5. User enters credentials
6. System authenticates and redirects to home page

## Troubleshooting

### Common Issues and Solutions

#### Authentication Problems
- **Issue**: 401 Unauthorized errors
- **Solution**: Check if token is valid, try logging out and back in
- **Solution**: Verify token expiration time hasn't passed
- **Solution**: Check if the token is properly included in API request headers

#### Map Not Loading
- **Issue**: Map container shows but map doesn't render
- **Solution**: Check internet connection, verify Leaflet CSS is loaded
- **Solution**: Ensure map container has a defined height
- **Solution**: Check browser console for Leaflet-specific errors
- **Solution**: Try using FallbackMap component as an alternative

#### Form Submission Errors
- **Issue**: Form submission fails with validation errors
- **Solution**: Check all required fields, ensure proper format for coordinates
- **Solution**: Verify location data is in the correct format ([lat, lng] array)
- **Solution**: Check for proper data transformation before API calls

#### Data Not Displaying
- **Issue**: Expected data doesn't appear in lists or maps
- **Solution**: Check console for API errors, verify data exists in database
- **Solution**: Verify API response format matches expected structure
- **Solution**: Check if data filtering is unintentionally excluding items
- **Solution**: Ensure component is properly subscribed to data changes

#### Grid Component Warnings
- **Issue**: MUI Grid component deprecation warnings
- **Solution**: Replace Grid components with Box components using flexbox layout
- **Solution**: Use `sx={{ display: 'flex', flexWrap: 'wrap' }}` instead of Grid container
- **Solution**: Use `sx={{ width: { xs: '100%', md: '50%' } }}` instead of Grid item props

#### MongoDB Connection Issues
- **Issue**: Server fails to connect to MongoDB
- **Solution**: Verify MongoDB service is running
- **Solution**: Check connection string in .env file
- **Solution**: Ensure network allows connection to MongoDB port
- **Solution**: Check MongoDB user credentials if authentication is enabled 