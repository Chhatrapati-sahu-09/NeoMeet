<div align="center">

<img src="./screenshots/logo.png" alt="NeoMeet Logo" width="200"/>

# NeoMeet – Video Conferencing Application

<p><em>Connect • Collaborate • Communicate</em></p>

</div>

---

# Overview

NeoMeet is a modern **real-time video conferencing application** that enables seamless communication through high-quality video calls, screen sharing, and instant messaging.

Built using **React, Node.js, WebRTC, and Socket.io**, NeoMeet provides a simple interface for hosting and joining meetings with minimal setup.

---

# Features

| Feature | Description |
|------|-------------|
| Video Calls | Real-time video and audio communication |
| Real-Time Chat | Send messages during meetings |
| Screen Sharing | Share screen with participants |
| Multi-Participant Meetings | Support for multiple users |
| Secure Authentication | Login and registration system |
| Dark / Light Mode | UI theme toggle |
| Meeting Code Join | Join meeting via code |
| Responsive Design | Works across devices |
| Real-Time Signaling | WebRTC signaling via Socket.io |

---

# Tech Stack

---

## Frontend

<p>
<img src="https://img.shields.io/badge/React-19.2.0-61DAFB?logo=react&logoColor=black"/>
<img src="https://img.shields.io/badge/Material--UI-v7-007FFF?logo=mui&logoColor=white"/>
<img src="https://img.shields.io/badge/Socket.io--Client-Real--time-black?logo=socketdotio&logoColor=white"/>
<img src="https://img.shields.io/badge/React%20Router-v7.9.4-CA4245?logo=reactrouter&logoColor=white"/>
<img src="https://img.shields.io/badge/WebRTC-P2P%20Streaming-333333?logo=webrtc&logoColor=white"/>
</p>

| Technology | Purpose |
|-----------|---------|
| React | Frontend UI framework |
| Material UI | UI component library |
| React Router | Client-side routing |
| Socket.io Client | Real-time communication |
| WebRTC | Peer-to-peer video/audio streaming |

---

## Backend

<p>
<img src="https://img.shields.io/badge/Node.js-Runtime-339933?logo=node.js&logoColor=white"/>
<img src="https://img.shields.io/badge/Express.js-5.1.0-000000?logo=express&logoColor=white"/>
<img src="https://img.shields.io/badge/Socket.io-4.8.1-black?logo=socketdotio&logoColor=white"/>
<img src="https://img.shields.io/badge/MongoDB-Database-47A248?logo=mongodb&logoColor=white"/>
<img src="https://img.shields.io/badge/JWT-Authentication-blue?logo=jsonwebtokens"/>
<img src="https://img.shields.io/badge/bcrypt-Password%20Hashing-orange"/>
</p>

| Technology | Purpose |
|-----------|---------|
| Node.js | Runtime environment |
| Express.js | Backend framework |
| Socket.io | WebSocket communication |
| MongoDB | NoSQL database |
| JWT | Authentication tokens |
| bcrypt | Password hashing |

---

# Folder Structure

```
ApnaVideoCall/

backend/
 └─ src/
    ├─ controllers/
    │  ├─ socketManager.js
    │  └─ user.controller.js
    │
    ├─ models/
    │  ├─ meeting.model.js
    │  └─ user.model.js
    │
    ├─ routes/
    │  └─ users.routes.js
    │
    └─ app.js

frontend/
 ├─ public/
 │  ├─ screenshots/
 │  └─ index.html
 │
 └─ src/
    ├─ contexts/
    │  ├─ AuthContext.jsx
    │  └─ ThemeContext.jsx
    │
    ├─ pages/
    │  ├─ landing.jsx
    │  ├─ authentication.jsx
    │  ├─ home.jsx
    │  ├─ VideoMeet.jsx
    │  └─ history.jsx
    │
    ├─ styles/
    │  └─ videoComponent.module.css
    │
    ├─ utils/
    │  └─ withAuth.jsx
    │
    ├─ App.js
    ├─ App.css
    └─ index.js

README.md
```

---

# Installation

## Prerequisites

- Node.js (v14 or higher)
- MongoDB (local or MongoDB Atlas)
- npm or yarn

---

# Backend Setup

### Navigate to backend

```
cd backend
```

### Install dependencies

```
npm install
```

### Create environment file

```
cp .env.example .env
```

### Configure environment variables

```
MONGODB_URI=your_mongodb_uri
JWT_SECRET=your_secret_key
PORT=8000
NODE_ENV=development
FRONTEND_URL=http://localhost:3000
```

### Start backend server

```
npm start
```

Server runs on

```
http://localhost:8000
```

---

# Frontend Setup

### Navigate to frontend

```
cd frontend
```

### Install dependencies

```
npm install
```

### Create environment file

```
cp .env.example .env
```

### Configure environment

```
REACT_APP_API_URL=http://localhost:8000
REACT_APP_SOCKET_URL=http://localhost:8000
```

### Start frontend

```
npm start
```

Frontend runs on

```
http://localhost:3000
```

---

# Application Workflow

---

## Landing Page

![Landing Page](./screenshots/image.png)

Users can

- Create an account
- Login to account
- Join meeting as guest

---

## Authentication

![Authentication](./screenshots/Screenshot%202025-12-04%20205129.png)

Users can

- Register new account
- Login using credentials

Passwords are securely hashed using **bcrypt**.

---

## Meeting Dashboard

![Home](./screenshots/Screenshot%202025-12-04%20205530.png)

Users can

- Create meeting
- Join meeting using meeting code

---

## Meeting Lobby

![Lobby](./screenshots/Gemini_Generated_Image_evf7rdevf7rdevf7.png)

Before joining users can

- Preview camera
- Enable microphone
- Enter display name

---

## Video Meeting Interface

![Meeting](./screenshots/image12.png)

Meeting interface includes

- Video grid
- Chat panel
- Meeting controls
- Participant display

---

## Meeting Controls

![Controls](./screenshots/final0mage.png)

Controls available

- Camera toggle
- Microphone mute/unmute
- Screen sharing
- Chat panel
- Copy meeting code
- Leave meeting

---

# Deployment

## Backend Deployment (Heroku Example)

Create Procfile

```
web: node src/app.js
```

Deploy

```
heroku create neomeet-backend
git push heroku main
```

Set environment variables

```
heroku config:set MONGODB_URI=your_uri
heroku config:set JWT_SECRET=your_secret
```

---

## Frontend Deployment

Build production

```
npm run build
```

Deploy using

```
vercel --prod
```

or

```
netlify deploy --prod
```

---

# Security Best Practices

Never commit `.env` files to Git.

Check

```
git status
```

If tracked remove

```
git rm --cached .env
git rm --cached backend/.env
git rm --cached frontend/.env
```

---

# Generate Secure JWT Secret

```
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```

---

# Contributing

1 Fork the repository

2 Create branch

```
git checkout -b feature/your-feature
```

3 Commit changes

```
git commit -m "Add feature"
```

4 Push branch

```
git push origin feature/your-feature
```

5 Open Pull Request

---

# License

This project is licensed under the **MIT License**.

---

# Author

NeoMeet Development Team

For support or issues open a GitHub issue.
