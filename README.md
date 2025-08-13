# Canvassing Application

**Live Deployment: 

## Description

The Canvassing Application is a comprehensive web-based tool designed for political campaigns and organizations to efficiently conduct door-to-door canvassing operations. The application enables canvassers to authenticate, access organized voter data by address, conduct surveys with voters, and track their canvassing progress. Built as a three-tier architecture, it combines a React frontend, Node.js/Express backend with PostgreSQL database, and a dedicated CSV API microservice for voter data management.

## Features

### ✅ User Authentication & Authorization

- **User Registration & Login**: Secure JWT-based authentication system
- **Protected Routes**: All canvassing features require user authentication
- **Session Management**: Persistent user sessions with token management

### ✅ Voter Data Management

- **CSV Data Integration**: Dedicated microservice for parsing Jersey City voter CSV files
- **Address-Based Organization**: Voters grouped by street address for efficient canvassing routes
- **Voter Search**: Search functionality by name and phone number
- **Real-time Data Access**: Live fetching from CSV API (port 3001)

### ✅ Address Management

- **Address Browsing**: View all addresses with voter counts
- **City Filtering**: Filter addresses by city for targeted canvassing
- **Address Search**: Quick search functionality for specific addresses
- **Voter Count Display**: See number of registered voters per address

### ✅ Survey Collection System

- **Interactive Survey Forms**: Three-question political preference surveys
  - **Question 1**: Support level (Strong Support, Lean Support, Undecided, Lean Opposed, Strong Opposed)
  - **Question 2**: Important issues (Healthcare, Economy, Jobs, Infrastructure, Public Health, Education)
  - **Question 3**: Voting method preference (Vote-by-Mail, Early Vote, Election Day)
- **Voter Auto-Population**: Voter information automatically populated from CSV data
- **Survey Validation**: Required field validation and error handling
- **Success Confirmation**: Visual feedback upon successful survey submission

### ✅ Survey Results & Analytics

- **Survey History**: View all surveys completed by the logged-in canvasser
- **Individual Survey Details**: Access specific survey responses by ID
- **Canvasser-Specific Data**: Each user only sees their own collected surveys

### ✅ Responsive Design

- **Mobile-Friendly**: Optimized for field use on mobile devices
- **Clean UI**: Professional styling with CSS for all components
- **Intuitive Navigation**: Clear breadcrumbs and back buttons

## Tests

### Location

- **Frontend Tests**: `/front-end/src/__tests__/`
- **Backend Tests**: Throughout `/back-end/` directory with `.test.js` files
- **API Tests**: `/csv-api/` (basic server tests)

### Running Tests

**Frontend Tests (React Testing Library + Jest):**

```bash
cd front-end
npm test
```

**Backend Tests (Jest + Supertest):**

```bash
cd back-end
npm test
```

**Test Coverage:**

```bash
cd back-end
npm run test:coverage
```

**Watch Mode:**

```bash
cd back-end
npm run test:watch
```

### Test Coverage Areas

- ✅ Authentication routes and middleware
- ✅ User model and database operations
- ✅ Survey model and CRUD operations
- ✅ API endpoints with mock data
- ✅ Component rendering and user interactions
- ✅ Error handling and validation

## User Flow

### 1. **Registration/Login**

- New users register with username, first name, last name, and password
- Existing users log in with username and password
- JWT token stored for session management

### 2. **Address Selection**

- View list of addresses with voter counts
- Filter by city (Jersey City areas)
- Search for specific addresses
- Select address to begin canvassing

### 3. **Voter Selection**

- View all registered voters at selected address
- See voter details: name, ID, phone number
- Search voters by name or phone
- Select specific voter to survey

### 4. **Survey Completion**

- Complete three-question survey form
- Voter information auto-populated
- Submit survey responses
- Receive confirmation of successful submission

### 5. **Survey Management**

- View history of completed surveys
- Access individual survey details
- Track canvassing progress

## API Documentation

### Authentication Endpoints

```http
POST /auth/register
Body: { username, firstName, lastName, password }
Returns: { user, token }

POST /auth/login
Body: { username, password }
Returns: { user, token }
```

### User Endpoints

```http
GET /users/:username
Headers: Authorization: Bearer <token>
Returns: { user }

PATCH /users/:username
Headers: Authorization: Bearer <token>
Body: { userData }
Returns: { user }
```

### Survey Endpoints

```http
POST /survey
Headers: Authorization: Bearer <token>
Body: { voter_id, first_name, last_name, city, question1, question2, question3 }
Returns: { survey }

GET /survey
Headers: Authorization: Bearer <token>
Returns: { surveys }

GET /survey/:id
Headers: Authorization: Bearer <token>
Returns: { survey }
```

### Voter Data Endpoints

```http
GET /voters
Headers: Authorization: Bearer <token>
Returns: { voters }

GET /voters/by-address
Headers: Authorization: Bearer <token>
Returns: { addresses }

GET /voters/address/:address
Headers: Authorization: Bearer <token>
Returns: { voters }
```

### CSV API Endpoints (Port 3001)

```http
GET /api/data
Returns: [{ "Voter File VANID", FirstName, LastName, Address, City, ... }]

GET /api/cities
Returns: [unique city names]
```

## Technology Stack

### Frontend

- **React 19.1.1** - Modern UI framework with hooks
- **React Router 6.30.1** - Client-side routing and navigation
- **React Context API** - User state management
- **Axios 1.7.9** - HTTP client for API communication
- **CSS3** - Custom styling with responsive design
- **Jest & React Testing Library** - Testing framework

### Backend

- **Node.js** - JavaScript runtime environment
- **Express 5.1.0** - Web application framework
- **PostgreSQL** - Relational database management
- **JWT (jsonwebtoken 9.0.2)** - Authentication tokens
- **bcrypt 6.0.0** - Password hashing
- **CORS 2.8.5** - Cross-origin resource sharing
- **Jest 30.0.5 & Supertest 7.1.4** - API testing

### CSV API Microservice

- **Express 5.1.0** - Lightweight API server
- **csv-parser** - CSV file processing
- **Node.js fs module** - File system operations

### Database

- **PostgreSQL** - Production database
- **Database Schema:**
  - `users` table: Authentication and user management
  - `survey` table: Survey responses with foreign key to users

### Development & Deployment

- **npm** - Package management
- **Nodemon** - Development server with hot reload
- **Environment Variables** - Configuration management
- **Git** - Version control

## Project Architecture

### Three-Tier Architecture

1. **Frontend (Port 3000)**: React SPA serving user interface
2. **Backend API (Port 3002)**: Express server handling authentication, surveys, and business logic
3. **CSV API (Port 3001)**: Dedicated microservice for voter data from CSV files

### Data Flow

1. **Authentication**: JWT tokens secure all API communications
2. **Voter Data**: CSV API serves voter information to backend routes
3. **Survey Collection**: Frontend → Backend API → PostgreSQL database
4. **Address Organization**: Real-time grouping of voters by address

## Installation & Setup

### Prerequisites

- Node.js (v16+)
- PostgreSQL
- npm

### Backend Setup

```bash
cd back-end
npm install
# Configure PostgreSQL database
# Run migrations/setup scripts
npm start
```

### Frontend Setup

```bash
cd front-end
npm install
npm start
```

### CSV API Setup

```bash
cd csv-api
npm install
npm start
```

## Important Notes

- **Data Privacy**: Voter information is handled securely with proper authentication
- **Scalability**: Microservice architecture allows independent scaling of components
- **Testing**: Comprehensive test coverage ensures reliability
- **Mobile Optimization**: Designed for field canvassing on mobile devices
- **Error Handling**: Robust error handling throughout the application stack
- **Security**: JWT authentication, password hashing, and input validation implemented

## Future Enhancements

- [ ] Deploy to cloud platform (Heroku, AWS, etc.)
- [ ] Add survey analytics dashboard
- [ ] Implement real-time canvassing progress tracking
- [ ] Add GPS integration for route optimization
- [ ] Implement bulk survey export functionality
- [ ] Add admin panel for user management

---

*This application was built as a capstone project demonstrating full-stack web development skills with React, Node.js, Express, PostgreSQL, and modern web development best practices.*
