# NeoMeet - Video Conferencing Application

## Overview

NeoMeet is a modern, real-time video conferencing application that enables seamless communication through high-quality video calls, screen sharing, and instant messaging. Built with React and Node.js, it provides an intuitive interface for hosting and joining virtual meetings with minimal setup.

## Features

- 🎥 **High-Quality Video Calls** - Crystal clear video and audio communication
- 💬 **Real-Time Chat** - Send and receive messages during meetings
- 🖥️ **Screen Sharing** - Share your screen with other participants
- 👥 **Multiple Participants** - Support for multi-user video conferences
- 🔐 **Secure Authentication** - User registration and login system
- 🌓 **Dark/Light Mode** - Toggle between dark and light themes
- 📋 **Easy Meeting Access** - Join as guest or registered user
- 🔗 **Meeting Code Sharing** - Copy and share meeting links instantly
- 📱 **Responsive Design** - Works seamlessly across devices
- ⚡ **Real-Time Signaling** - WebRTC with Socket.io for instant connectivity

## Tech Stack

### Frontend
- **React** 19.2.0 - UI library
- **Material-UI** v7 - Component library
- **Socket.io-client** - Real-time communication
- **React Router** v7.9.4 - Navigation
- **WebRTC** - Peer-to-peer video/audio streaming

### Backend
- **Node.js** - Runtime environment
- **Express.js** 5.1.0 - Web framework
- **Socket.io** 4.8.1 - WebSocket communication
- **MongoDB** - Database
- **JWT** - Authentication tokens
- **bcrypt** - Password hashing

## Folder Structure

```
ApnaVideoCall/
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   │   ├── socketManager.js
│   │   │   └── user.controller.js
│   │   ├── models/
│   │   │   ├── meeting.model.js
│   │   │   └── user.model.js
│   │   ├── routes/
│   │   │   └── users.routes.js
│   │   └── app.js
│   └── package.json
├── frontend/
│   ├── public/
│   │   ├── screenshots/
│   │   └── index.html
│   ├── src/
│   │   ├── contexts/
│   │   │   ├── AuthContext.jsx
│   │   │   └── ThemeContext.jsx
│   │   ├── pages/
│   │   │   ├── landing.jsx
│   │   │   ├── authentication.jsx
│   │   │   ├── home.jsx
│   │   │   ├── VideoMeet.jsx
│   │   │   └── history.jsx
│   │   ├── styles/
│   │   │   └── videoComponent.module.css
│   │   ├── utils/
│   │   │   └── withAuth.jsx
│   │   ├── App.js
│   │   ├── App.css
│   │   └── index.js
│   └── package.json
└── README.md
```

## Installation

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (local or MongoDB Atlas)
- npm or yarn package manager

### Backend Setup

1. Navigate to the backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the backend directory:
```env
PORT=8000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
```

4. Start the backend server:
```bash
npm start
```

The backend server will run on `http://localhost:8000`

### Frontend Setup

1. Navigate to the frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the frontend directory (if needed):
```env
REACT_APP_API_URL=http://localhost:8000
```

4. Start the React development server:
```bash
npm start
```

The frontend will run on `http://localhost:3000`

## How It Works (Step-by-Step With Images)

### Step 1 – Landing Page

![Landing Page](./frontend/public/screenshots/image.png)

When you first visit NeoMeet, you'll see the landing page with options to:
- **Get Started** - Create a new account and start a meeting
- **Join as Guest** - Join a meeting without registration
- **Login/Register** - Access your account

The page features a clean, modern design with dark mode support.

---

### Step 2 – Login or Sign Up

![Authentication Page](./frontend/public/screenshots/Screenshot%202025-12-04%20205129.png)

On this screen, you can either sign in to your NeoMeet account or create a new one:

**If you already have an account:**
- Enter your email and password
- Click **Sign In** to continue

**If you're new:**
- Click **Sign Up** button
- Fill in your name, email, and password
- Create your account in just a few seconds

---

### Step 3 – Create or Join a Meeting

![Home Page](./frontend/public/screenshots/Screenshot%202025-12-04%20205530.png)

After logging in, you'll reach the home page where you can:

**Host a Meeting:**
- The system automatically generates a unique meeting code
- Share this code with others to start a new video call

**Join a Meeting:**
- Already have a meeting code?
- Simply enter the code in the text field
- Click **JOIN** to join the ongoing call

---

### Step 4 – Lobby (Camera Preview)

![Lobby Preview](./frontend/public/screenshots/Gemini_Generated_Image_evf7rdevf7rdevf7.png)

Before joining the meeting, you'll enter the lobby where you can:

1. **Allow camera & mic** - Grant permissions for video and audio
2. **Check your preview** - See how you look before joining
3. **Enter your name** - Set your display name for the meeting
4. **Click CONNECT** - Join the video call when ready

This ensures you're fully prepared before entering the meeting room.

---

### Step 5 – Video Call Interface

![Video Meeting](./frontend/public/screenshots/image12.png)

**You're Now in the Meeting!**

Once connected, you'll see:
- **Large video display** - Main participant video feed
- **Participant thumbnails** - Other meeting participants in a grid
- **Control bar** - Easy access to all meeting controls at the bottom
- **Chat panel** - Messaging sidebar on the left (toggleable)

You've successfully joined the call and are ready to communicate!

---

### Step 6 – Chat System

The chat panel allows you to:
- **Send messages** - Type and send text messages to all participants
- **Receive messages** - See messages from other participants in real-time
- **Copy meeting code** - Share the meeting link with others directly from the chat panel
- **Toggle visibility** - Show or hide the chat panel as needed

Messages appear in a modern bubble design with sender names clearly displayed.

---

### Step 7 – Meeting Controls (Video, Audio, Share Screen)

![Meeting Controls](./frontend/public/screenshots/final0mage.png)

**Use Meeting Tools**

Inside the meeting, you can use all essential tools from the control bar:

- **📹 Camera Toggle** - Turn your video on/off
- **🎤 Mic Toggle** - Mute or unmute yourself
- **📺 Screen Share** - Present your screen to others
- **💬 Chat Panel** - Send and receive messages with participants
- **📋 Copy Code** - Share your meeting code to invite others
- **📞 End Call** - Leave the meeting anytime

Everything you need is available right in the bottom control bar for easy access.

---

## Deployment

### Backend Deployment (Example: Heroku)

1. Create a `Procfile` in the backend directory:
```
web: node src/app.js
```

2. Deploy to Heroku:
```bash
heroku create your-app-name-backend
git push heroku main
```

3. Set environment variables:
```bash
heroku config:set MONGODB_URI=your_mongodb_uri
heroku config:set JWT_SECRET=your_jwt_secret
```

### Frontend Deployment (Example: Vercel/Netlify)

1. Build the production version:
```bash
npm run build
```

2. Deploy using Vercel:
```bash
vercel --prod
```

Or Netlify:
```bash
netlify deploy --prod
```

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/YourFeature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/YourFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Made with ❤️ by NeoMeet Team**

For questions or support, please open an issue on GitHub.
