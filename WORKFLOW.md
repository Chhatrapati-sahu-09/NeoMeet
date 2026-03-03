# NeoMeet - Application Workflow

## Overview

This document describes the complete user workflows and system processes within the NeoMeet video conferencing application. It covers all user journeys from landing on the platform to completing a video call.

---

## User Journey Map

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           USER JOURNEY FLOW                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│    ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐        │
│    │  Landing │────►│   Auth   │────►│   Home   │────►│  Video   │        │
│    │   Page   │     │   Page   │     │   Page   │     │  Meeting │        │
│    └──────────┘     └──────────┘     └──────────┘     └──────────┘        │
│         │               │ ▲               │                 │              │
│         │               │ │               │                 │              │
│         │               ▼ │               ▼                 │              │
│         │          ┌──────────┐     ┌──────────┐           │              │
│         └─────────►│ Register │     │ History  │◄──────────┘              │
│                    └──────────┘     │   Page   │                          │
│                                     └──────────┘                          │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 1. Landing Page Workflow

### Overview

The landing page serves as the entry point for all users, showcasing NeoMeet's features and providing navigation options.

### Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         LANDING PAGE WORKFLOW                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                        User Visits NeoMeet                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  Landing Page      │                                  │
│                    │  Renders           │                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│           ┌───────────────────┼───────────────────┐                        │
│           │                   │                   │                        │
│           ▼                   ▼                   ▼                        │
│   ┌───────────────┐   ┌───────────────┐   ┌───────────────┐              │
│   │ View Features │   │ Toggle Theme  │   │ Navigate Auth │              │
│   │ & Info        │   │ (Dark/Light)  │   │               │              │
│   └───────────────┘   └───────────────┘   └───────┬───────┘              │
│                                                    │                        │
│                                       ┌────────────┴────────────┐          │
│                                       │                         │          │
│                                       ▼                         ▼          │
│                               ┌─────────────┐           ┌─────────────┐   │
│                               │   Sign In   │           │   Sign Up   │   │
│                               │   /auth     │           │  /auth?mode │   │
│                               └─────────────┘           │   =signup   │   │
│                                                         └─────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### User Actions

| Action              | Trigger          | Result                          |
| ------------------- | ---------------- | ------------------------------- |
| View landing page   | URL: `/`         | Display homepage                |
| Toggle theme        | Click theme icon | Switch dark/light mode          |
| Click "Get Started" | Button click     | Navigate to `/auth?mode=signup` |
| Click "Login"       | Button click     | Navigate to `/auth`             |

---

## 2. Authentication Workflow

### Overview

Users must authenticate to access meeting features. The system supports both registration (new users) and login (existing users).

### Registration Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         REGISTRATION WORKFLOW                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     User                    Frontend                    Backend             │
│       │                        │                           │                │
│       │  Fill registration     │                           │                │
│       │  form (name, user,     │                           │                │
│       │  password)             │                           │                │
│       │───────────────────────►│                           │                │
│       │                        │                           │                │
│       │                        │  handleRegister()         │                │
│       │                        │  POST /api/v1/users/      │                │
│       │                        │  register                 │                │
│       │                        │──────────────────────────►│                │
│       │                        │                           │                │
│       │                        │                           │ Check if       │
│       │                        │                           │ user exists    │
│       │                        │                           │                │
│       │                        │                    ┌──────┴──────┐         │
│       │                        │                    │             │         │
│       │                        │               EXISTS?       NOT EXISTS     │
│       │                        │                    │             │         │
│       │                        │                    ▼             ▼         │
│       │                        │              Return 302    Hash password   │
│       │                        │              "User         Save to DB      │
│       │                        │              exists"       Return 201      │
│       │                        │◄──────────────────┴─────────────┘         │
│       │                        │                           │                │
│       │  Show success/error    │                           │                │
│       │  Snackbar message      │                           │                │
│       │◄───────────────────────│                           │                │
│       │                        │                           │                │
│       │  [If success]          │                           │                │
│       │  Switch to login form  │                           │                │
│       │                        │                           │                │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Login Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            LOGIN WORKFLOW                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     User                    Frontend                    Backend             │
│       │                        │                           │                │
│       │  Enter credentials     │                           │                │
│       │  (username, password)  │                           │                │
│       │───────────────────────►│                           │                │
│       │                        │                           │                │
│       │                        │  handleLogin()            │                │
│       │                        │  POST /api/v1/users/login │                │
│       │                        │──────────────────────────►│                │
│       │                        │                           │                │
│       │                        │                           │ Find user by   │
│       │                        │                           │ username       │
│       │                        │                           │                │
│       │                        │                    ┌──────┴──────┐         │
│       │                        │                USER FOUND?  NOT FOUND      │
│       │                        │                    │             │         │
│       │                        │                    ▼             ▼         │
│       │                        │              Compare pwd    Return 404     │
│       │                        │              with bcrypt    "Not found"    │
│       │                        │                    │                       │
│       │                        │             ┌──────┴──────┐                │
│       │                        │          MATCH?        NO MATCH            │
│       │                        │             │             │                │
│       │                        │             ▼             ▼                │
│       │                        │        Generate      Return 401            │
│       │                        │        token         "Unauthorized"        │
│       │                        │        Return 200                          │
│       │                        │        {token}                             │
│       │                        │◄─────────────────────────┘                │
│       │                        │                           │                │
│       │                        │  Store token in           │                │
│       │                        │  localStorage             │                │
│       │                        │                           │                │
│       │  Navigate to /home     │                           │                │
│       │◄───────────────────────│                           │                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Token Storage

```javascript
// On successful login
localStorage.setItem("token", response.data.token);

// Token format (40 hex characters)
// Example: "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0"
```

---

## 3. Home Dashboard Workflow

### Overview

The home page serves as the main dashboard for authenticated users, allowing them to join meetings.

### Protected Route Check

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      ROUTE PROTECTION (withAuth HOC)                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                    User navigates to /home                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  withAuth HOC      │                                  │
│                    │  checks token      │                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│                    ┌──────────┴──────────┐                                 │
│                    │                     │                                  │
│             TOKEN EXISTS?          NO TOKEN                                 │
│                    │                     │                                  │
│                    ▼                     ▼                                  │
│           ┌───────────────┐     ┌───────────────┐                         │
│           │ Render Home   │     │ Redirect to   │                         │
│           │ Component     │     │ /auth         │                         │
│           └───────────────┘     └───────────────┘                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Join Meeting Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          JOIN MEETING WORKFLOW                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     User                    Frontend                    Backend             │
│       │                        │                           │                │
│       │  Enter meeting code    │                           │                │
│       │  (e.g., "team-standup")│                           │                │
│       │───────────────────────►│                           │                │
│       │                        │                           │                │
│       │  Click "Join"          │                           │                │
│       │───────────────────────►│                           │                │
│       │                        │                           │                │
│       │                        │  addToUserHistory()       │                │
│       │                        │  POST /add_to_activity    │                │
│       │                        │  {token, meeting_code}    │                │
│       │                        │──────────────────────────►│                │
│       │                        │                           │                │
│       │                        │                           │ Find user by   │
│       │                        │                           │ token          │
│       │                        │                           │                │
│       │                        │                           │ Create Meeting │
│       │                        │                           │ record         │
│       │                        │                           │                │
│       │                        │  201 "Added to history"   │                │
│       │                        │◄──────────────────────────│                │
│       │                        │                           │                │
│       │                        │  navigate(`/${meetingCode}`)               │
│       │                        │                           │                │
│       │  Redirect to video     │                           │                │
│       │  meeting page          │                           │                │
│       │◄───────────────────────│                           │                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Video Meeting Workflow

### Overview

The video meeting page establishes WebRTC peer-to-peer connections through Socket.io signaling.

### Complete Video Call Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        VIDEO MEETING WORKFLOW                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │  PHASE 1: INITIALIZATION                                            │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│     User enters meeting page (/:url)                                        │
│                     │                                                       │
│                     ▼                                                       │
│     ┌─────────────────────────────────┐                                    │
│     │  getPermissions()               │                                    │
│     │  - Request camera access        │                                    │
│     │  - Request microphone access    │                                    │
│     │  - Check screen share support   │                                    │
│     └─────────────────────────────────┘                                    │
│                     │                                                       │
│                     ▼                                                       │
│     ┌─────────────────────────────────┐                                    │
│     │  Pre-call Setup Modal           │                                    │
│     │  - Enter username               │                                    │
│     │  - Preview video                │                                    │
│     │  - Toggle camera/mic            │                                    │
│     └─────────────────────────────────┘                                    │
│                     │                                                       │
│                     ▼                                                       │
│     ┌─────────────────────────────────┐                                    │
│     │  User clicks "Join"             │                                    │
│     │  getMedia() called              │                                    │
│     └─────────────────────────────────┘                                    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │  PHASE 2: SOCKET CONNECTION                                         │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│     ┌───────────────┐         ┌───────────────┐         ┌─────────────┐  │
│     │    Client     │         │   Socket.io   │         │Other Clients│  │
│     │   (User A)    │         │    Server     │         │  (Users)    │  │
│     └───────┬───────┘         └───────┬───────┘         └──────┬──────┘  │
│             │                         │                        │          │
│             │  io.connect(server_url) │                        │          │
│             │────────────────────────►│                        │          │
│             │                         │                        │          │
│             │  emit("join-call", URL) │                        │          │
│             │────────────────────────►│                        │          │
│             │                         │                        │          │
│             │                         │  Add to connections[]  │          │
│             │                         │  Track timeOnline      │          │
│             │                         │                        │          │
│             │                         │  emit("user-joined")   │          │
│             │◄────────────────────────│───────────────────────►│          │
│             │                         │  (to all in room)      │          │
│             │                         │                        │          │
│             │  Send existing messages │                        │          │
│             │  (if any)               │                        │          │
│             │◄────────────────────────│                        │          │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │  PHASE 3: WebRTC PEER CONNECTION                                    │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│     ┌───────────────┐                               ┌───────────────┐     │
│     │    User A     │                               │    User B     │     │
│     │  (New User)   │                               │ (In Meeting)  │     │
│     └───────┬───────┘                               └───────┬───────┘     │
│             │                                               │              │
│             │  1. Create RTCPeerConnection                  │              │
│             │     with STUN server config                   │              │
│             │                                               │              │
│             │  2. Add local media stream                    │              │
│             │                                               │              │
│             │  3. createOffer() → SDP                       │              │
│             │                                               │              │
│             │  4. setLocalDescription(offer)                │              │
│             │                                               │              │
│             │  5. emit("signal", {sdp: offer})              │              │
│             │─────────────────────────────────────────────►│              │
│             │         (via Socket.io server)                │              │
│             │                                               │              │
│             │                    6. setRemoteDescription(offer)            │
│             │                                               │              │
│             │                    7. createAnswer() → SDP    │              │
│             │                                               │              │
│             │                    8. setLocalDescription(answer)            │
│             │                                               │              │
│             │  9. emit("signal", {sdp: answer})             │              │
│             │◄─────────────────────────────────────────────│              │
│             │                                               │              │
│             │  10. setRemoteDescription(answer)             │              │
│             │                                               │              │
│             │                                               │              │
│             │  ═══════ ICE Candidate Exchange ═══════      │              │
│             │                                               │              │
│             │  emit("signal", {ice: candidate})             │              │
│             │◄────────────────────────────────────────────►│              │
│             │                                               │              │
│             │                                               │              │
│             │  ════════ P2P Connection Established ════════│              │
│             │                                               │              │
│             │  Direct video/audio streaming                 │              │
│             │◄═══════════════════════════════════════════►│              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### WebRTC Signaling State Machine

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      WebRTC SIGNALING STATE MACHINE                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                         ┌─────────────────┐                                │
│                         │     STABLE      │                                │
│                         │   (Initial)     │                                │
│                         └────────┬────────┘                                │
│                                  │                                          │
│                    createOffer() │                                          │
│                                  ▼                                          │
│                         ┌─────────────────┐                                │
│                         │   HAVE_LOCAL    │                                │
│                         │     OFFER       │                                │
│                         └────────┬────────┘                                │
│                                  │                                          │
│              setRemoteDescription│(answer)                                  │
│                                  ▼                                          │
│                         ┌─────────────────┐                                │
│                         │     STABLE      │──────────────────┐             │
│                         │  (Connected)    │                  │             │
│                         └─────────────────┘                  │             │
│                                  ▲                           │             │
│                                  │                           │             │
│              setRemoteDescription│(offer)         createAnswer()           │
│                                  │                           │             │
│                         ┌────────┴────────┐                  │             │
│                         │  HAVE_REMOTE    │                  │             │
│                         │     OFFER       │◄─────────────────┘             │
│                         └─────────────────┘                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. In-Meeting Features Workflow

### Video/Audio Toggle

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     VIDEO/AUDIO TOGGLE WORKFLOW                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     User clicks camera/mic toggle button                                    │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  Toggle video/audio state           │                                │
│     │  setVideo(!video) / setAudio(!audio)│                                │
│     └─────────────────────────────────────┘                                │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  getUserMedia() with new settings   │                                │
│     │  navigator.mediaDevices.getUserMedia│                                │
│     └─────────────────────────────────────┘                                │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  Update local stream                │                                │
│     │  localVideoref.current.srcObject    │                                │
│     └─────────────────────────────────────┘                                │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  For each peer connection:          │                                │
│     │  - addStream(newStream)             │                                │
│     │  - createOffer()                    │                                │
│     │  - emit("signal", {sdp})            │                                │
│     └─────────────────────────────────────┘                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Screen Share Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       SCREEN SHARE WORKFLOW                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     User clicks screen share button                                         │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  setScreen(true)                    │                                │
│     └─────────────────────────────────────┘                                │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  getDisplayMedia()                  │                                │
│     │  - System screen picker dialog      │                                │
│     │  - User selects screen/window       │                                │
│     └─────────────────────────────────────┘                                │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  Replace local stream with screen   │                                │
│     │  Update all peer connections        │                                │
│     │  Broadcast new stream to peers      │                                │
│     └─────────────────────────────────────┘                                │
│                       │                                                     │
│                       ▼                                                     │
│     ┌─────────────────────────────────────┐                                │
│     │  On screen share end:               │                                │
│     │  - track.onended triggered          │                                │
│     │  - Revert to camera stream          │                                │
│     │  - getUserMedia() for camera        │                                │
│     └─────────────────────────────────────┘                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Chat Message Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CHAT MESSAGE WORKFLOW                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     Sender                    Server                    Receivers           │
│       │                         │                           │               │
│       │  Type message           │                           │               │
│       │  Press Enter/Send       │                           │               │
│       │                         │                           │               │
│       │  emit("chat-message",   │                           │               │
│       │       data, sender)     │                           │               │
│       │────────────────────────►│                           │               │
│       │                         │                           │               │
│       │                         │  Find matching room       │               │
│       │                         │  from connections[]       │               │
│       │                         │                           │               │
│       │                         │  Store in messages[]      │               │
│       │                         │                           │               │
│       │                         │  Broadcast to all in room │               │
│       │                         │──────────────────────────►│               │
│       │                         │                           │               │
│       │                         │  emit("chat-message",     │               │
│       │                         │       data, sender, id)   │               │
│       │◄────────────────────────│                           │               │
│       │                         │                           │               │
│       │  addMessage()           │           addMessage()    │               │
│       │  Update UI              │           Update UI       │               │
│       │                         │                           │               │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. User Disconnect Workflow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        USER DISCONNECT WORKFLOW                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     User A                    Server                    Other Users         │
│       │                         │                           │               │
│       │  Click "Leave" or       │                           │               │
│       │  close browser tab      │                           │               │
│       │                         │                           │               │
│       │  socket.disconnect()    │                           │               │
│       │────────────────────────►│                           │               │
│       │                         │                           │               │
│       │                         │  Calculate time online    │               │
│       │                         │  (timeOnline[socket.id])  │               │
│       │                         │                           │               │
│       │                         │  Find room with user      │               │
│       │                         │                           │               │
│       │                         │  emit("user-left", id)    │               │
│       │                         │──────────────────────────►│               │
│       │                         │                           │               │
│       │                         │  Remove from connections[]│               │
│       │                         │                           │               │
│       │                         │  If room empty:           │               │
│       │                         │  delete connections[room] │               │
│       │                         │                           │               │
│       │                         │                           │  Remove video │
│       │                         │                           │  from grid    │
│       │                         │                           │               │
│       │  navigate("/home")      │                           │  Close peer   │
│       │                         │                           │  connection   │
│       │                         │                           │               │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Meeting History Workflow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       MEETING HISTORY WORKFLOW                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     User                    Frontend                    Backend             │
│       │                        │                           │                │
│       │  Navigate to /history  │                           │                │
│       │───────────────────────►│                           │                │
│       │                        │                           │                │
│       │                        │  getHistoryOfUser()       │                │
│       │                        │  GET /get_all_activity    │                │
│       │                        │  ?token=xxx               │                │
│       │                        │──────────────────────────►│                │
│       │                        │                           │                │
│       │                        │                           │ Find user by   │
│       │                        │                           │ token          │
│       │                        │                           │                │
│       │                        │                           │ Find meetings  │
│       │                        │                           │ by user_id     │
│       │                        │                           │                │
│       │                        │  [meetings array]         │                │
│       │                        │◄──────────────────────────│                │
│       │                        │                           │                │
│       │                        │  setMeetings(history)     │                │
│       │                        │                           │                │
│       │  Display meeting cards │                           │                │
│       │  with code & date      │                           │                │
│       │◄───────────────────────│                           │                │
│       │                        │                           │                │
│       │  Click to rejoin       │                           │                │
│       │  (copy code / join)    │                           │                │
│       │───────────────────────►│                           │                │
│       │                        │                           │                │
│       │  Navigate to meeting   │                           │                │
│       │                        │                           │                │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 8. Theme Toggle Workflow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         THEME TOGGLE WORKFLOW                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                    User clicks theme toggle icon                            │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  toggleTheme()     │                                  │
│                    │  from ThemeContext │                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  setDarkMode(      │                                  │
│                    │    !darkMode       │                                  │
│                    │  )                 │                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  useEffect saves   │                                  │
│                    │  to localStorage   │                                  │
│                    │  "darkMode": bool  │                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  All components    │                                  │
│                    │  re-render with    │                                  │
│                    │  new theme colors  │                                  │
│                    └────────────────────┘                                  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 9. Logout Workflow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           LOGOUT WORKFLOW                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                      User clicks "Logout"                                   │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  handleLogout()    │                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  localStorage.     │                                  │
│                    │  removeItem("token")                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  navigate("/auth") │                                  │
│                    └─────────┬──────────┘                                  │
│                               │                                             │
│                               ▼                                             │
│                    ┌────────────────────┐                                  │
│                    │  User redirected   │                                  │
│                    │  to login page     │                                  │
│                    └────────────────────┘                                  │
│                                                                             │
│  Note: Token remains in database until user logs in again                  │
│  (generates new token) - Consider implementing token invalidation          │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Error Handling Workflows

### API Error Handling

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        API ERROR HANDLING                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     API Response Code        Frontend Action              User Feedback     │
│     ─────────────────        ───────────────              ─────────────     │
│                                                                             │
│     200 OK                   Process data                 -                 │
│     201 Created              Show success                Snackbar message   │
│     302 Found                Show "User exists"          Error message      │
│     400 Bad Request          Prompt for input            Form validation    │
│     401 Unauthorized         Show "Invalid creds"        Error message      │
│     404 Not Found            Show "User not found"       Error message      │
│     500 Server Error         Show generic error          Error message      │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### WebRTC Error Handling

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       WebRTC ERROR HANDLING                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     Error Type               Handling Strategy                              │
│     ──────────               ─────────────────                              │
│                                                                             │
│     Permission Denied        Set videoAvailable/audioAvailable = false      │
│                              Continue with available media                  │
│                                                                             │
│     ICE Connection Failed    Log error, attempt reconnection                │
│                                                                             │
│     SDP Negotiation Error    Log error, skip peer                           │
│                                                                             │
│     Media Track Ended        Replace with black/silent stream               │
│                              Renegotiate with peers                         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Complete Application State Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      COMPLETE STATE TRANSITIONS                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                         ┌─────────────┐                                    │
│                         │   LANDING   │                                    │
│                         │    PAGE     │                                    │
│                         └──────┬──────┘                                    │
│                                │                                            │
│              ┌─────────────────┼─────────────────┐                         │
│              │                 │                 │                          │
│              ▼                 ▼                 ▼                          │
│     ┌─────────────┐   ┌─────────────┐   ┌─────────────┐                   │
│     │   SIGN IN   │   │   SIGN UP   │   │   GUEST     │                   │
│     │             │   │             │   │   ACCESS    │                   │
│     └──────┬──────┘   └──────┬──────┘   └──────┬──────┘                   │
│            │                 │                 │                            │
│            │    ┌────────────┘                 │                            │
│            │    │                              │                            │
│            ▼    ▼                              │                            │
│     ┌─────────────┐                           │                            │
│     │AUTHENTICATED│                           │                            │
│     │    HOME     │                           │                            │
│     └──────┬──────┘                           │                            │
│            │                                   │                            │
│     ┌──────┼───────────────────────────────────┘                           │
│     │      │                                                                │
│     │      ▼                                                                │
│     │ ┌─────────────┐                                                      │
│     │ │   VIDEO     │◄────────────────┐                                    │
│     │ │   MEETING   │                 │                                     │
│     │ └──────┬──────┘                 │                                     │
│     │        │                        │                                     │
│     │        ▼                        │                                     │
│     │ ┌─────────────┐          ┌─────────────┐                             │
│     │ │  IN-CALL    │─────────►│   LEAVE     │                             │
│     │ │  FEATURES   │          │   MEETING   │                             │
│     │ │             │          └──────┬──────┘                             │
│     │ │ • Camera    │                 │                                     │
│     │ │ • Mic       │                 ▼                                     │
│     │ │ • Screen    │          ┌─────────────┐                             │
│     │ │ • Chat      │          │    HOME     │─────┐                       │
│     │ └─────────────┘          │   (Return)  │     │                       │
│     │                          └─────────────┘     │                       │
│     │                                              │                        │
│     │                                              ▼                        │
│     │                                       ┌─────────────┐                │
│     └──────────────────────────────────────►│   HISTORY   │                │
│                                             │    PAGE     │                │
│                                             └─────────────┘                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Summary

NeoMeet's workflow architecture provides:

1. **Seamless Authentication** - Simple register/login with token-based sessions
2. **Real-time Communication** - Socket.io for signaling and chat
3. **P2P Video Calls** - WebRTC for direct media streaming
4. **Persistent History** - MongoDB storage for meeting records
5. **Theme Customization** - Dark/Light mode with localStorage persistence

---

_Workflow Document - NeoMeet Video Conferencing Application_
