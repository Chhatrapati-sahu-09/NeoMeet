# NeoMeet - Database Structure

## Overview

NeoMeet uses **MongoDB** as its primary database, leveraging Mongoose ODM for schema definition and data validation. The database consists of two main collections that handle user authentication and meeting history.

---

## Database Configuration

### Connection

```javascript
// Connection URI from environment variable
mongoose.connect(process.env.MONGODB_URI);

// Example MongoDB Atlas URI format:
// mongodb+srv://<username>:<password>@cluster.mongodb.net/<database>?retryWrites=true&w=majority
```

### Mongoose Configuration

- **ODM Version**: Mongoose 8.19.1
- **Connection Options**: Default (auto-reconnect enabled)

---

## Collections Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      NeoMeet Database                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────────────────┐    ┌───────────────────────┐       │
│  │    Users Collection   │    │  Meetings Collection  │       │
│  │                       │    │                       │       │
│  │  ┌─────────────────┐ │    │  ┌─────────────────┐  │       │
│  │  │ _id (ObjectId)  │ │    │  │ _id (ObjectId)  │  │       │
│  │  │ name            │ │    │  │ user_id         │  │       │
│  │  │ username        │◄┼────┼──┤ meetingCode     │  │       │
│  │  │ password        │ │    │  │ date            │  │       │
│  │  │ token           │ │    │  │                 │  │       │
│  │  └─────────────────┘ │    │  └─────────────────┘  │       │
│  │                       │    │                       │       │
│  └───────────────────────┘    └───────────────────────┘       │
│                                                                 │
│        Relationship: user_id (Meetings) → username (Users)     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Schema Definitions

### 1. User Schema

**Location**: `backend/src/models/user.model.js`

```javascript
const userSchema = new Schema({
  name: {
    type: String,
    required: true,
  },
  username: {
    type: String,
    required: true,
    unique: true,
  },
  password: {
    type: String,
    required: true,
  },
  token: {
    type: String,
  },
});
```

#### Field Details

| Field      | Type     | Required | Unique | Description                             |
| ---------- | -------- | -------- | ------ | --------------------------------------- |
| `_id`      | ObjectId | Auto     | Yes    | MongoDB auto-generated ID               |
| `name`     | String   | Yes      | No     | User's display name                     |
| `username` | String   | Yes      | Yes    | Login identifier (unique constraint)    |
| `password` | String   | Yes      | No     | Bcrypt hashed password (10 salt rounds) |
| `token`    | String   | No       | No     | Session authentication token            |

#### Sample Document

```json
{
  "_id": "507f1f77bcf86cd799439011",
  "name": "John Doe",
  "username": "johndoe123",
  "password": "$2b$10$XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "token": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0"
}
```

---

### 2. Meeting Schema

**Location**: `backend/src/models/meeting.model.js`

```javascript
const meetingSchema = new Schema({
  user_id: {
    type: String,
  },
  meetingCode: {
    type: String,
    required: true,
  },
  date: {
    type: Date,
    default: Date.now,
    required: true,
  },
});
```

#### Field Details

| Field         | Type     | Required | Default  | Description                       |
| ------------- | -------- | -------- | -------- | --------------------------------- |
| `_id`         | ObjectId | Auto     | -        | MongoDB auto-generated ID         |
| `user_id`     | String   | No       | -        | Reference to user's username      |
| `meetingCode` | String   | Yes      | -        | Unique meeting room identifier    |
| `date`        | Date     | Yes      | Date.now | Timestamp when meeting was joined |

#### Sample Document

```json
{
  "_id": "507f1f77bcf86cd799439022",
  "user_id": "johndoe123",
  "meetingCode": "abc-xyz-123",
  "date": "2026-03-03T10:30:00.000Z"
}
```

---

## Entity Relationship Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                        USERS                                        │
├─────────────────────────────────────────────────────────────────────┤
│  PK  │  _id         │  ObjectId       │  Auto-generated           │
│      │  name        │  String         │  NOT NULL                 │
│  UK  │  username    │  String         │  NOT NULL, UNIQUE         │
│      │  password    │  String         │  NOT NULL (hashed)        │
│      │  token       │  String         │  NULLABLE                 │
└──────┴──────────────┴─────────────────┴───────────────────────────┘
                                  │
                                  │ 1:N (One to Many)
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                       MEETINGS                                      │
├─────────────────────────────────────────────────────────────────────┤
│  PK  │  _id         │  ObjectId       │  Auto-generated           │
│  FK  │  user_id     │  String         │  References users.username│
│      │  meetingCode │  String         │  NOT NULL                 │
│      │  date        │  Date           │  NOT NULL, DEFAULT NOW    │
└──────┴──────────────┴─────────────────┴───────────────────────────┘

Legend:
PK = Primary Key
UK = Unique Key
FK = Foreign Key (Logical, not enforced by MongoDB)
```

---

## Data Operations

### User Operations

#### 1. Create User (Register)

```javascript
// Register new user
const hashedPassword = await bcrypt.hash(password, 10);
const newUser = new User({
  name: name,
  username: username,
  password: hashedPassword,
});
await newUser.save();
```

#### 2. Find User (Login)

```javascript
// Find user by username
const user = await User.findOne({ username });

// Verify password
const isValid = await bcrypt.compare(password, user.password);

// Update token
user.token = crypto.randomBytes(20).toString("hex");
await user.save();
```

#### 3. Find User by Token

```javascript
// Get user by authentication token
const user = await User.findOne({ token: token });
```

---

### Meeting Operations

#### 1. Create Meeting Record

```javascript
// Add meeting to user's history
const newMeeting = new Meeting({
  user_id: user.username,
  meetingCode: meeting_code,
});
await newMeeting.save();
```

#### 2. Get User's Meeting History

```javascript
// Retrieve all meetings for a user
const meetings = await Meeting.find({ user_id: user.username });
```

---

## Query Examples

### Find User by Username

```javascript
// MongoDB Command
db.users.findOne({ username: "johndoe123" });

// Mongoose
const user = await User.findOne({ username: "johndoe123" });
```

### Get All Meetings for User

```javascript
// MongoDB Command
db.meetings.find({ user_id: "johndoe123" });

// Mongoose
const meetings = await Meeting.find({ user_id: "johndoe123" });
```

### Update User Token

```javascript
// MongoDB Command
db.users.updateOne(
  { username: "johndoe123" },
  { $set: { token: "new-token-value" } },
);

// Mongoose
user.token = "new-token-value";
await user.save();
```

---

## Indexes

### Automatic Indexes

| Collection | Field      | Type    | Created By        |
| ---------- | ---------- | ------- | ----------------- |
| users      | `_id`      | Primary | MongoDB           |
| users      | `username` | Unique  | Schema Definition |
| meetings   | `_id`      | Primary | MongoDB           |

### Recommended Indexes (For Performance)

```javascript
// Add index for faster meeting lookups by user_id
db.meetings.createIndex({ user_id: 1 });

// Add compound index for user_id and date
db.meetings.createIndex({ user_id: 1, date: -1 });
```

---

## Data Validation

### Mongoose Built-in Validation

| Field       | Validation       | Error Message                                     |
| ----------- | ---------------- | ------------------------------------------------- |
| name        | required         | Path `name` is required                           |
| username    | required, unique | Path `username` is required / Duplicate key error |
| password    | required         | Path `password` is required                       |
| meetingCode | required         | Path `meetingCode` is required                    |
| date        | required         | Path `date` is required                           |

---

## Data Flow Diagrams

### Registration Flow

```
Client                     Server                      MongoDB
   │                          │                           │
   │  POST /register          │                           │
   │  {name, username, pwd}   │                           │
   │─────────────────────────►│                           │
   │                          │  findOne({username})      │
   │                          │──────────────────────────►│
   │                          │◄──────────────────────────│
   │                          │  (Check if exists)        │
   │                          │                           │
   │                          │  bcrypt.hash(password)    │
   │                          │                           │
   │                          │  new User().save()        │
   │                          │──────────────────────────►│
   │                          │◄──────────────────────────│
   │  201: "User Registered"  │                           │
   │◄─────────────────────────│                           │
```

### Login Flow

```
Client                     Server                      MongoDB
   │                          │                           │
   │  POST /login             │                           │
   │  {username, password}    │                           │
   │─────────────────────────►│                           │
   │                          │  findOne({username})      │
   │                          │──────────────────────────►│
   │                          │◄──────────────────────────│
   │                          │                           │
   │                          │  bcrypt.compare()         │
   │                          │                           │
   │                          │  Generate token           │
   │                          │  user.token = token       │
   │                          │  save()                   │
   │                          │──────────────────────────►│
   │                          │◄──────────────────────────│
   │  200: {token}            │                           │
   │◄─────────────────────────│                           │
```

### Add Meeting to History

```
Client                     Server                      MongoDB
   │                          │                           │
   │  POST /add_to_activity   │                           │
   │  {token, meeting_code}   │                           │
   │─────────────────────────►│                           │
   │                          │  findOne({token})         │
   │                          │──────────────────────────►│
   │                          │◄──────────────────────────│
   │                          │                           │
   │                          │  new Meeting({            │
   │                          │    user_id,               │
   │                          │    meetingCode            │
   │                          │  }).save()                │
   │                          │──────────────────────────►│
   │                          │◄──────────────────────────│
   │  201: "Added to history" │                           │
   │◄─────────────────────────│                           │
```

---

## Security Considerations

### Password Storage

- **Algorithm**: bcrypt
- **Salt Rounds**: 10
- **Never stored**: Plain text passwords

### Token Management

- **Generation**: `crypto.randomBytes(20)`
- **Length**: 40 hex characters
- **Storage**: Stored in MongoDB, not JWT

### Data Protection

- **Sensitive Fields**: password (hashed), token
- **Exposed Fields**: name, username, meetingCode, date

---

## Backup & Recovery

### MongoDB Atlas (Recommended)

- Automated daily backups
- Point-in-time recovery
- Cross-region replication

### Manual Backup

```bash
# Export database
mongodump --uri="mongodb+srv://..." --out=backup/

# Import database
mongorestore --uri="mongodb+srv://..." backup/
```

---

## Collection Statistics (Example)

```javascript
// Get collection stats
db.users.stats()
db.meetings.stats()

// Expected output structure
{
    "ns": "neomeet.users",
    "count": 1000,
    "size": 512000,
    "avgObjSize": 512,
    "storageSize": 1048576,
    "indexes": 2,
    "indexSizes": {
        "_id_": 36864,
        "username_1": 36864
    }
}
```

---

## Schema Evolution Notes

### Current Version: 1.0

#### Potential Future Enhancements

1. **User Schema**
   - Add `email` field for password recovery
   - Add `avatar` for profile pictures
   - Add `createdAt` and `updatedAt` timestamps

2. **Meeting Schema**
   - Add `duration` to track meeting length
   - Add `participants` array for attendee list
   - Add `title` for meeting naming

---

_Database Structure Document - NeoMeet Video Conferencing Application_
