# NeoMeet - System Architecture

## Overview

NeoMeet is a real-time video conferencing application built using a modern MERN stack architecture with WebRTC for peer-to-peer video communication and Socket.io for real-time signaling.

---

## High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT LAYER                                    │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                     React Frontend (Port 3000)                       │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │   │
│  │  │   Landing    │  │    Auth      │  │    Home      │               │   │
│  │  │    Page      │  │    Page      │  │    Page      │               │   │
│  │  └──────────────┘  └──────────────┘  └──────────────┘               │   │
│  │  ┌──────────────┐  ┌──────────────┐                                 │   │
│  │  │  VideoMeet   │  │   History    │                                 │   │
│  │  │  Component   │  │    Page      │                                 │   │
│  │  └──────────────┘  └──────────────┘                                 │   │
│  │                                                                      │   │
│  │  ┌─────────────────────────────────────────────────────────────┐   │   │
│  │  │                    Context Providers                         │   │   │
│  │  │  ┌─────────────────┐    ┌─────────────────┐                 │   │   │
│  │  │  │  AuthContext    │    │  ThemeContext   │                 │   │   │
│  │  │  │  (Auth State)   │    │  (Dark/Light)   │                 │   │   │
│  │  │  └─────────────────┘    └─────────────────┘                 │   │   │
│  │  └─────────────────────────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
              HTTP/REST          Socket.io          WebRTC
                    │                 │                 │
                    ▼                 ▼                 ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              SERVER LAYER                                    │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                   Node.js Backend (Port 8000)                        │   │
│  │                                                                      │   │
│  │  ┌──────────────────────────────────────────────────────────────┐  │   │
│  │  │                      Express.js Server                        │  │   │
│  │  │  ┌─────────────────┐  ┌─────────────────┐                    │  │   │
│  │  │  │     Routes      │  │   Controllers   │                    │  │   │
│  │  │  │  /api/v1/users  │  │ user.controller │                    │  │   │
│  │  │  └─────────────────┘  └─────────────────┘                    │  │   │
│  │  └──────────────────────────────────────────────────────────────┘  │   │
│  │                                                                      │   │
│  │  ┌──────────────────────────────────────────────────────────────┐  │   │
│  │  │                    Socket.io Server                           │  │   │
│  │  │  ┌─────────────────┐                                         │  │   │
│  │  │  │ socketManager   │  - Room Management                      │  │   │
│  │  │  │                 │  - Signaling (WebRTC)                   │  │   │
│  │  │  │                 │  - Chat Messages                        │  │   │
│  │  │  └─────────────────┘                                         │  │   │
│  │  └──────────────────────────────────────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              DATA LAYER                                      │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                        MongoDB Database                              │   │
│  │  ┌─────────────────────┐    ┌─────────────────────┐                │   │
│  │  │    Users Collection │    │  Meetings Collection │                │   │
│  │  │  - name             │    │  - user_id           │                │   │
│  │  │  - username         │    │  - meetingCode       │                │   │
│  │  │  - password (hash)  │    │  - date              │                │   │
│  │  │  - token            │    │                      │                │   │
│  │  └─────────────────────┘    └─────────────────────┘                │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Technology Stack

### Frontend

| Technology       | Version | Purpose                  |
| ---------------- | ------- | ------------------------ |
| React            | 19.2.0  | UI Framework             |
| React Router DOM | 7.9.4   | Client-side routing      |
| Material-UI      | 7.3.4   | UI Component Library     |
| Socket.io-client | 4.8.1   | Real-time communication  |
| Axios            | 1.13.1  | HTTP requests            |
| WebRTC (Native)  | -       | Peer-to-peer video/audio |

### Backend

| Technology | Version | Purpose               |
| ---------- | ------- | --------------------- |
| Node.js    | -       | JavaScript runtime    |
| Express.js | 5.1.0   | Web framework         |
| Socket.io  | 4.8.1   | WebSocket server      |
| Mongoose   | 8.19.1  | MongoDB ODM           |
| bcrypt     | 6.0.0   | Password hashing      |
| crypto     | 1.0.1   | Token generation      |
| dotenv     | 17.2.3  | Environment variables |

### Database

| Technology | Purpose                 |
| ---------- | ----------------------- |
| MongoDB    | NoSQL document database |

---

## Component Architecture

### Frontend Structure

```
frontend/src/
├── App.js                    # Main app with routing
├── index.js                  # React entry point
├── environment.js            # API URL configuration
│
├── contexts/
│   ├── AuthContext.jsx       # Authentication state & methods
│   └── ThemeContext.jsx      # Dark/Light mode state
│
├── pages/
│   ├── landing.jsx           # Public landing page
│   ├── authentication.jsx    # Login/Register forms
│   ├── home.jsx              # Dashboard (protected)
│   ├── VideoMeet.jsx         # Video call interface
│   └── history.jsx           # Meeting history
│
├── utils/
│   └── withAuth.jsx          # HOC for route protection
│
└── styles/
    └── videoComponent.module.css
```

### Backend Structure

```
backend/src/
├── app.js                    # Express server & Socket.io setup
│
├── controllers/
│   ├── user.controller.js    # User authentication & history
│   └── socketManager.js      # Real-time room management
│
├── models/
│   ├── user.model.js         # User schema
│   └── meeting.model.js      # Meeting schema
│
└── routes/
    └── users.routes.js       # API route definitions
```

---

## API Architecture

### RESTful Endpoints

| Method | Endpoint                         | Description              |
| ------ | -------------------------------- | ------------------------ |
| POST   | `/api/v1/users/register`         | Register new user        |
| POST   | `/api/v1/users/login`            | Authenticate user        |
| POST   | `/api/v1/users/add_to_activity`  | Save meeting to history  |
| GET    | `/api/v1/users/get_all_activity` | Get user meeting history |

### Socket.io Events

| Event          | Direction       | Description                |
| -------------- | --------------- | -------------------------- |
| `join-call`    | Client → Server | Join a meeting room        |
| `user-joined`  | Server → Client | Notify when user joins     |
| `user-left`    | Server → Client | Notify when user leaves    |
| `signal`       | Bidirectional   | WebRTC signaling (SDP/ICE) |
| `chat-message` | Bidirectional   | Real-time chat messages    |

---

## WebRTC Architecture

```
┌──────────────────┐                              ┌──────────────────┐
│    User A        │                              │    User B        │
│  (Peer Client)   │                              │  (Peer Client)   │
├──────────────────┤                              ├──────────────────┤
│  RTCPeerConnection                              │  RTCPeerConnection
│  ┌─────────────┐ │                              │ ┌─────────────┐  │
│  │ Local Media │ │                              │ │ Local Media │  │
│  │   Stream    │ │                              │ │   Stream    │  │
│  └─────────────┘ │                              │ └─────────────┘  │
│        │         │                              │        │         │
│        ▼         │                              │        ▼         │
│  ┌─────────────┐ │    ICE Candidates/SDP       │ ┌─────────────┐  │
│  │    Offer/   │◄├──────────────────────────────┤►│   Answer/   │  │
│  │   Answer    │ │   (via Socket.io Server)     │ │    Offer    │  │
│  └─────────────┘ │                              │ └─────────────┘  │
│        │         │                              │        │         │
│        ▼         │                              │        ▼         │
│  ┌─────────────┐ │    Direct P2P Connection    │ ┌─────────────┐  │
│  │Remote Video │◄├──────────────────────────────┤►│Remote Video │  │
│  │   Stream    │ │    (After Negotiation)       │ │   Stream    │  │
│  └─────────────┘ │                              │ └─────────────┘  │
└──────────────────┘                              └──────────────────┘
```

### WebRTC Flow

1. **Get User Media** - Access camera/microphone
2. **Create Peer Connection** - Initialize RTCPeerConnection with STUN server
3. **Create Offer** - Generate SDP offer
4. **Signal via Socket.io** - Send offer through server
5. **Create Answer** - Recipient generates SDP answer
6. **Exchange ICE Candidates** - Network connectivity information
7. **Establish P2P Connection** - Direct media streaming

---

## Authentication Flow

```
┌──────────┐     ┌─────────────┐     ┌──────────────┐     ┌──────────┐
│  Client  │     │  Express    │     │  Controller  │     │ MongoDB  │
└────┬─────┘     └──────┬──────┘     └──────┬───────┘     └────┬─────┘
     │                  │                   │                  │
     │  POST /login     │                   │                  │
     │─────────────────►│                   │                  │
     │                  │  login()          │                  │
     │                  │──────────────────►│                  │
     │                  │                   │  findOne(user)   │
     │                  │                   │─────────────────►│
     │                  │                   │◄─────────────────│
     │                  │                   │                  │
     │                  │                   │  bcrypt.compare()
     │                  │                   │─────────────────►│
     │                  │                   │                  │
     │                  │                   │  Generate Token  │
     │                  │                   │  (crypto.random) │
     │                  │                   │                  │
     │                  │  { token }        │                  │
     │◄─────────────────│◄──────────────────│                  │
     │                  │                   │                  │
     │  Store in        │                   │                  │
     │  localStorage    │                   │                  │
     │                  │                   │                  │
```

---

## State Management

### Context Providers Hierarchy

```
<Router>
  <ThemeProvider>           ← Dark/Light mode state
    <AuthProvider>          ← Authentication state
      <App Routes />
    </AuthProvider>
  </ThemeProvider>
</Router>
```

### AuthContext State

```javascript
{
  userData: {},                    // Current user data
  setUserData: function,           // Update user data
  handleLogin: function,           // Login handler
  handleRegister: function,        // Register handler
  getHistoryOfUser: function,      // Fetch meeting history
  addToUserHistory: function       // Add meeting to history
}
```

### ThemeContext State

```javascript
{
  darkMode: boolean,               // Current theme
  toggleTheme: function            // Toggle theme
}
```

---

## Security Architecture

### Password Security

- **Hashing**: bcrypt with salt rounds of 10
- **Comparison**: bcrypt.compare() for login verification

### Token-Based Authentication

- **Token Generation**: crypto.randomBytes(20).toString('hex')
- **Storage**: Server-side in MongoDB, client-side in localStorage
- **Validation**: Token passed in query/body for protected routes

### CORS Configuration

```javascript
{
  origin: process.env.FRONTEND_URL || "*",
  methods: "GET,POST,PUT,DELETE",
  credentials: true
}
```

---

## Deployment Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Render.com Platform                          │
│                                                                     │
│  ┌─────────────────────┐         ┌─────────────────────┐          │
│  │   Frontend Service  │         │   Backend Service   │          │
│  │  (Static Build)     │◄───────►│  (Node.js Server)   │          │
│  │                     │  API    │                     │          │
│  │  neomeet.onrender   │ Calls   │ neomeet-api.render  │          │
│  └─────────────────────┘         └──────────┬──────────┘          │
│                                             │                      │
└─────────────────────────────────────────────┼──────────────────────┘
                                              │
                                              ▼
                               ┌─────────────────────────┐
                               │    MongoDB Atlas        │
                               │   (Cloud Database)      │
                               └─────────────────────────┘
```

---

## Environment Variables

### Backend (.env)

```
PORT=8000
MONGODB_URI=mongodb+srv://...
FRONTEND_URL=https://your-frontend-url.com
```

### Frontend (.env)

```
REACT_APP_API_URL=https://your-backend-url.com
REACT_APP_SOCKET_URL=https://your-backend-url.com
```

---

## Data Flow Summary

1. **User Registration/Login** → REST API → MongoDB
2. **Video Call Join** → Socket.io → Room Management
3. **WebRTC Signaling** → Socket.io Events → P2P Connection
4. **Chat Messages** → Socket.io → Broadcast to Room
5. **Meeting History** → REST API → MongoDB

---

## Scalability Considerations

- **Horizontal Scaling**: Stateless backend with load balancing
- **Database Indexing**: Unique index on username
- **WebRTC Limitations**: P2P scales well for small groups
- **Socket.io Scaling**: Can use Redis adapter for multi-instance

---

_Architecture Document - NeoMeet Video Conferencing Application_
