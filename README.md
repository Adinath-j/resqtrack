# ResQTrack - Disaster Management System

ResQTrack is a comprehensive disaster management system designed specifically for Maharashtra, India. It helps authorities and citizens report, track, and respond to disasters effectively through an interactive map-based interface with MongoDB backend integration for robust data persistence.

![ResQTrack Screenshot](./src/assets/screenshot.png)

## 🌟 Features

- **Interactive Map Interface**: Visualize disasters, safe zones, and authorities across Maharashtra
- **Disaster Reporting**: Report disasters with location, type, severity, and description
- **Disaster Management**: View detailed information about disasters and coordinate response efforts
- **Safe Zones**: Create and manage safe zones for disaster-affected areas
- **Authority Management**: Register and manage emergency response authorities
- **Disaster History**: Track past disasters with detailed information
- **Weather Information**: Get real-time weather updates for any location in Maharashtra
- **User Authentication**: Secure login and registration with JWT authentication
- **Role-Based Access Control**: Separate user and admin interfaces with appropriate permissions
- **MongoDB Integration**: Persistent data storage with geospatial queries
- **Responsive Design**: Modern UI with Tailwind CSS and Material UI components

## 📋 Project Structure

```
resqtrack/
├── src/                  # Frontend source code
│   ├── assets/           # Static assets (images, icons)
│   ├── components/       # Reusable UI components
│   │   ├── FallbackMap.jsx         # Simplified map component
│   │   ├── Hero.jsx                # Hero section component
│   │   ├── Navbar.jsx              # Navigation component
│   │   └── ProtectedRoute.jsx      # Auth protection component
│   ├── pages/            # Main page components
│   │   ├── AdminDashboard.jsx      # Admin dashboard
│   │   ├── AdminLogin.jsx          # Admin login page
│   │   ├── AuthorityManagement.jsx # Authority management interface
│   │   ├── DisasterDetails.jsx     # Detailed view of a disaster
│   │   ├── DisasterHistory.jsx     # History of past disasters
│   │   ├── Login.jsx               # User login page
│   │   ├── Register.jsx            # User registration page
│   │   ├── ReportDisaster.jsx      # Interface for reporting disasters
│   │   ├── SafeZones.jsx           # Safe zone management
│   │   ├── UserProfile.jsx         # User profile management
│   │   ├── ViewDisasters.jsx       # Main map view of disasters
│   │   └── WeatherInfo.jsx         # Weather information interface
│   ├── services/         # API service functions
│   │   └── api.js                  # API client configuration
│   ├── utils/            # Utility functions
│   │   ├── authorityUtils.js       # Authority data utilities
│   │   ├── disasterUtils.js        # Disaster data utilities
│   │   ├── leafletUtils.js         # Leaflet map utilities
│   │   ├── mapUtils.js             # Map helper functions
│   │   ├── safeZoneUtils.js        # Safe zone utilities
│   │   └── weatherUtils.js         # Weather data utilities
│   ├── App.jsx           # Main application component
│   ├── index.css         # Global styles with Tailwind
│   └── main.jsx          # Application entry point
├── server/               # Backend server code
│   ├── controllers/      # Request handlers
│   │   ├── authController.js       # Authentication controller
│   │   ├── authorityController.js  # Authority management
│   │   ├── disasterController.js   # Disaster management
│   │   └── safeZoneController.js   # Safe zone management
│   ├── models/           # MongoDB data models
│   │   ├── authorityModel.js       # Authority schema
│   │   ├── disasterModel.js        # Disaster schema
│   │   ├── safeZoneModel.js        # Safe zone schema
│   │   └── userModel.js            # User schema
│   ├── routes/           # API route definitions
│   │   ├── authorityRoutes.js      # Authority endpoints
│   │   ├── disasterRoutes.js       # Disaster endpoints
│   │   ├── safeZoneRoutes.js       # Safe zone endpoints
│   │   └── userRoutes.js           # User/auth endpoints
│   ├── scripts/          # Utility scripts
│   │   ├── seedAdmin.js            # Admin user creation
│   │   ├── seedAuthorities.js      # Sample authority data
│   │   └── updateDisasterStatus.js # Status update script
│   └── server.js         # Server entry point
├── public/               # Static files
├── .env                  # Frontend environment variables
├── server/.env           # Backend environment variables
├── tailwind.config.js    # Tailwind CSS configuration
├── postcss.config.js     # PostCSS configuration
├── vite.config.js        # Vite configuration
└── package.json          # Project dependencies
```

## 🚀 Getting Started

### Prerequisites

- Node.js (v16 or higher)
- npm (v8 or higher)
- MongoDB (v6.0 or higher)

### Installation

1. **Clone the repository**

```bash
git clone https://github.com/yourusername/resqtrack.git
cd resqtrack
```

2. **Install frontend dependencies**

```bash
npm install
```

3. **Install backend dependencies**

```bash
cd server
npm install
cd ..
```

4. **Set up environment variables**

Create a `.env` file in the root directory with the following variables:

```
VITE_API_URL=http://localhost:5000/api
VITE_OPENWEATHER_API_KEY=your_openweather_api_key
```

Create a `.env` file in the server directory:

```
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/resqtrack
NODE_ENV=development
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=90d
```

5. **Start MongoDB**

Make sure MongoDB is running on your system. If you're using MongoDB as a service:

```bash
# On Windows
net start MongoDB

# On macOS/Linux
sudo systemctl start mongod
```

6. **Run the application**

Start the backend server:

```bash
cd server
npm run dev
```

In a new terminal, start the frontend development server:

```bash
# From the project root
npm run dev
```

7. **Access the application**

Open your browser and navigate to:
- Frontend: http://localhost:5173
- Backend API: http://localhost:5000/api

### Seeding Initial Data (Optional)

To populate the database with sample authorities:

```bash
cd server
node scripts/seedAuthorities.js
```

To create an admin user:

```bash
cd server
node scripts/seedAdmin.js
```

Default login credentials for testing:
- Admin: admin@resqtrack.com / Admin@123
- Regular User: user@resqtrack.com / User@123

## 📱 Application Workflow

### 1. Authentication
- Register a new account or log in with existing credentials
- Admin users have access to additional management features
- JWT tokens are used for secure authentication

### 2. View Disasters
- The main page displays an interactive map of Maharashtra showing all reported disasters
- Toggle visibility of disasters, safe zones, and authorities using the layer controls
- Click on any disaster marker to view basic information and access detailed view

### 2. Report a Disaster
- Navigate to "Report Disaster" page
- Click on the map to select the disaster location
- Fill in details about the disaster (type, severity, description)
- Submit the report to register the disaster in the system

### 3. Manage Authorities
- Navigate to "Authority Management" page
- View all registered emergency response authorities
- Add new authorities with their location, contact information, and jurisdiction
- Edit or delete existing authorities

### 4. Create Safe Zones
- Navigate to "Safe Zones" page
- Click on the map to select a location for a new safe zone
- Provide details like name, capacity, and contact information
- Safe zones will be visible on the main disaster map

### 5. View Disaster Details
- Click on any disaster marker and select "View Details"
- See comprehensive information about the disaster
- View nearby authorities and safe zones
- Monitor response efforts and updates

### 7. Admin Dashboard (Admin Only)
- Access comprehensive statistics about disasters, authorities, and safe zones
- Approve or reject user-reported disasters
- Manage user accounts
- View system analytics

### 8. User Profile Management
- View and edit personal profile information
- Change password
- View history of reported disasters

### 6. Check Weather Information
- Navigate to "Weather Information" page
- Search for any location in Maharashtra
- View current weather conditions and forecast
- Use this information for disaster planning and response

## 🔧 Configuration

### Map Configuration

ResQTrack is configured specifically for Maharashtra with the following parameters:
- Map center: 19.7515°N, 75.7139°E (Maharashtra center)
- Map boundaries: 
  - Southwest: 15.6°N, 72.6°E
  - Northeast: 22.1°N, 80.9°E
- Zoom levels: min=6 (state level), max=18 (street level)
- Initial zoom: 7 (optimal state view)

### API Keys

To use the weather functionality, you need to obtain an API key from [OpenWeatherMap](https://openweathermap.org/api) and add it to your `.env` file.

## 💻 Technologies Used

### Frontend
- React 18
- Vite 4
- Material UI 5
- Tailwind CSS 3
- React Router 6
- Leaflet.js & React Leaflet
- Axios
- JWT Authentication

### Backend
- Express.js 4
- MongoDB 6 & Mongoose 7
- Node.js 16+
- JSON Web Tokens
- bcrypt for password hashing
- Geospatial indexing

## 🧪 Testing

Run tests with:

```bash
npm test
```

## 🏗️ Building for Production

Build the frontend:

```bash
npm run build
```

This will create a `dist` directory with production-ready files.

For the backend, set `NODE_ENV=production` in your server's `.env` file.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👥 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🙏 Acknowledgements

- [OpenStreetMap](https://www.openstreetmap.org/) for map data
- [OpenWeatherMap](https://openweathermap.org/) for weather data
- [Material UI](https://mui.com/) for UI components
- [Leaflet](https://leafletjs.com/) for interactive maps
