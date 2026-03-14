# NeoMeet – Application Workflow

## Overview

This document describes the **complete user workflows and system processes** in the NeoMeet video conferencing application.

It explains how users move through the system from **landing on the platform → authentication → joining meetings → video communication → history tracking**.

---

# User Journey

```mermaid
flowchart LR

Landing[Landing Page]
Auth[Authentication Page]
Home[Home Dashboard]
Meeting[Video Meeting]
History[Meeting History]

Landing --> Auth
Auth --> Home
Home --> Meeting
Meeting --> History
History --> Home
```

The user journey follows this order:

1. User visits **Landing Page**
2. User **logs in or registers**
3. User reaches the **Home dashboard**
4. User **joins or creates a meeting**
5. After leaving the meeting the **history is saved**

---

# Landing Page Workflow

```mermaid
flowchart TD

User[User visits website] --> Landing[Landing Page Rendered]

Landing --> Features[View Features]
Landing --> Theme[Toggle Theme]
Landing --> AuthNav[Navigate to Authentication]

AuthNav --> SignIn[Login Page]
AuthNav --> SignUp[Register Page]
```

### User Actions

| Action | Trigger | Result |
|------|------|------|
| View landing page | `/` | Homepage loads |
| Toggle theme | Click theme icon | Dark/Light mode switch |
| Click Get Started | Button click | Navigate to `/auth?mode=signup` |
| Click Login | Button click | Navigate to `/auth` |

---

# Authentication Workflow

## Registration Flow

```mermaid
sequenceDiagram

participant User
participant Frontend
participant Backend
participant Database

User->>Frontend: Fill registration form
Frontend->>Backend: POST /users/register

Backend->>Database: Check if user exists

alt User exists
Database-->>Backend: User found
Backend-->>Frontend: Error response
Frontend-->>User: "User already exists"
else User not found
Backend->>Database: Save new user
Database-->>Backend: Success
Backend-->>Frontend: Registration successful
Frontend-->>User: Show success message
end
```

---

## Login Flow

```mermaid
sequenceDiagram

participant User
participant Frontend
participant Backend
participant Database

User->>Frontend: Enter username/password
Frontend->>Backend: POST /users/login

Backend->>Database: Find user

Database-->>Backend: User record

Backend->>Backend: Compare password using bcrypt

alt Password correct
Backend->>Backend: Generate token
Backend-->>Frontend: Login success + token
Frontend->>Frontend: Store token in localStorage
Frontend-->>User: Redirect to /home
else Invalid password
Backend-->>Frontend: Unauthorized
Frontend-->>User: Show error
end
```

---

# Home Dashboard Workflow

The home dashboard is a **protected route** accessible only after login.

```mermaid
flowchart TD

User[User navigates to /home] --> CheckAuth[Check token]

CheckAuth -->|Token exists| Home[Render Home Dashboard]

CheckAuth -->|No token| Redirect[Redirect to /auth]
```

---

# Join Meeting Workflow

```mermaid
sequenceDiagram

participant User
participant Frontend
participant Backend
participant Database

User->>Frontend: Enter meeting code
User->>Frontend: Click Join

Frontend->>Backend: POST /add_to_activity

Backend->>Database: Save meeting record

Database-->>Backend: Success

Backend-->>Frontend: Activity saved

Frontend-->>User: Redirect to meeting page
```

---

# Video Meeting Workflow

The meeting system uses:

- **Socket.io for signaling**
- **WebRTC for peer-to-peer video communication**

```mermaid
sequenceDiagram

participant UserA
participant SignalingServer
participant UserB

UserA->>SignalingServer: join-call(room)

SignalingServer-->>UserB: user-joined

UserA->>UserB: WebRTC Offer

UserB->>UserA: WebRTC Answer

UserA->>UserB: ICE Candidates
UserB->>UserA: ICE Candidates

UserA<->>UserB: Direct Video & Audio Stream
```

---

# Chat Messaging Workflow

```mermaid
sequenceDiagram

participant Sender
participant Server
participant Receiver

Sender->>Server: chat-message event

Server->>Server: Identify meeting room

Server-->>Receiver: Broadcast message

Receiver->>Receiver: Display message
```

---

# User Disconnect Workflow

```mermaid
sequenceDiagram

participant User
participant Server
participant OtherUsers

User->>Server: socket.disconnect()

Server->>Server: Remove user from room

Server-->>OtherUsers: user-left event

OtherUsers->>OtherUsers: Remove video stream
```

---

# Meeting History Workflow

```mermaid
sequenceDiagram

participant User
participant Frontend
participant Backend
participant Database

User->>Frontend: Open /history

Frontend->>Backend: GET /get_all_activity

Backend->>Database: Find meetings by user

Database-->>Backend: Meeting list

Backend-->>Frontend: Return meetings

Frontend-->>User: Display meeting cards
```

---

# Theme Toggle Workflow

```mermaid
flowchart TD

User[User clicks theme icon] --> Toggle[toggleTheme()]

Toggle --> UpdateState[Update darkMode state]

UpdateState --> Save[Save to localStorage]

Save --> Rerender[Components re-render with new theme]
```

---

# Logout Workflow

```mermaid
flowchart TD

User[User clicks logout] --> RemoveToken[Remove token from localStorage]

RemoveToken --> Redirect[Redirect to /auth]
```

---

# Error Handling

| Error Code | Meaning | UI Action |
|------|------|------|
| 200 | Success | Process response |
| 201 | Created | Show success message |
| 401 | Unauthorized | Show invalid credentials |
| 404 | Not Found | User not found |
| 500 | Server Error | Show generic error |

---

# WebRTC Error Handling

| Error | Handling |
|------|------|
| Permission denied | Disable camera/mic |
| ICE failure | Attempt reconnection |
| SDP negotiation error | Skip peer |
| Media track ended | Replace stream |

---

# Complete Application State Flow

```mermaid
flowchart TD

Landing[Landing Page]

Auth[Authentication]

Home[Home Dashboard]

Meeting[Video Meeting]

InCall[In-call Features]

History[Meeting History]

Landing --> Auth
Auth --> Home
Home --> Meeting
Meeting --> InCall
InCall --> Home
Home --> History
```

---

# Summary

NeoMeet's workflow architecture includes:

- Secure **token-based authentication**
- Real-time **Socket.io signaling**
- Peer-to-peer **WebRTC video communication**
- Persistent **MongoDB meeting history**
- Customizable **Dark/Light UI themes**

This workflow ensures **low latency communication, scalable architecture, and smooth user experience**.

---

*Workflow Documentation – NeoMeet Video Conferencing Platform*
