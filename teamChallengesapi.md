# 🏆 Team Challenges API

RESTful API for managing team and individual challenges in a sports management platform.

## 🔐 Authentication

All routes are protected using JWT. Include this in your request headers:

```http
Authorization: Bearer <your_token>
```

### Role Permissions
- **`coach` / `manager`**: Full access (create, update, delete)
- **`athlete` / `parent`**: Read-only access to assigned/viewable challenges

## 📘 API Endpoints

### Get All Team Challenges
```http
GET /api/teamChallenges
```

Retrieve all team challenges accessible to the authenticated user.

**Response:**
```json
[
  {
    "_id": "666000d529dcab001e9d50b1",
    "title": "Sprint Challenge",
    "team": "665f3b17c6c3e8e5dbb45a20",
    "description": "Run 3km in 15 mins",
    "points": 100,
    "category": "Fitness",
    "status": "active",
    "startDate": "2025-06-06T16:39:00.000Z",
    "endDate": "2025-06-21T16:39:00.000Z"
  }
]
```

### Get My Challenges
```http
GET /api/teamChallenges/my
```

Get all challenges assigned to the logged-in user (athlete/parent).

### Create Challenge
```http
POST /api/teamChallenges
```
**Roles:** `coach`, `manager`

Create a new team or individual challenge.

**Request Body:**
```json
{
  "title": "Sprint Challenge",
  "type": "team",
  "team": "665f3b17c6c3e8e5dbb45a20",
  "users": [],
  "description": "Run 3km in 15 mins",
  "points": 100,
  "category": "Fitness",
  "status": "active",
  "startDate": "2025-06-06T16:39:00.000Z",
  "endDate": "2025-06-21T16:39:00.000Z"
}
```

**For Individual Challenges:**
```json
{
  "title": "Personal Goal Challenge",
  "type": "individual",
  "users": ["665f3b17c6c3e8e5dbb45a21", "665f3b17c6c3e8e5dbb45a22"],
  "description": "Improve personal best by 10%",
  "points": 150,
  "category": "Performance",
  "status": "active",
  "startDate": "2025-06-06T16:39:00.000Z",
  "endDate": "2025-06-21T16:39:00.000Z"
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "_id": "666000d529dcab001e9d50b1",
    "title": "Sprint Challenge",
    "type": "team",
    "team": "665f3b17c6c3e8e5dbb45a20",
    "description": "Run 3km in 15 mins",
    "points": 100,
    "category": "Fitness",
    "status": "active",
    "startDate": "2025-06-06T16:39:00.000Z",
    "endDate": "2025-06-21T16:39:00.000Z"
  }
}
```

### Get Challenge by ID
```http
GET /api/teamChallenges/:id
```

Fetch a specific challenge by its ID.

### Get Team Challenges
```http
GET /api/teamChallenges/team/:teamId
```

Get all challenges for a specific team.

### Get User Challenges
```http
GET /api/teamChallenges/user/:userId
```

Get all challenges assigned to a specific user.

### Update Challenge
```http
PUT /api/teamChallenges/:id
```
**Roles:** `coach`, `manager`

Update an existing challenge.

**Request Body (partial update):**
```json
{
  "status": "completed",
  "points": 120,
  "description": "Updated challenge description"
}
```

### Delete Challenge
```http
DELETE /api/teamChallenges/:id
```
**Roles:** `coach`, `manager`

Delete a challenge by ID.

## 🗂️ Challenge Schema

```json
{
  "_id": "ObjectId",
  "title": "String",
  "type": "team | individual",
  "team": "ObjectId (if type is team)",
  "users": ["ObjectId"],
  "description": "String",
  "points": "Number",
  "category": "String",
  "status": "active | in-progress | completed",
  "startDate": "Date",
  "endDate": "Date",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

## 📌 Usage Notes

- Use `type: "individual"` with `users` array for personalized challenges
- Use `type: "team"` with `team` field for group challenges
- Coaches/Managers can only operate on teams they belong to
- Parents can only view challenges assigned to their children
- All dates should be in ISO 8601 format
- Points field defaults to 0 if not specified
- Status defaults to "active" if not specified

## 🚨 Error Responses

The API returns standard HTTP status codes:

- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

**Error Response Format:**
```json
{
  "status": "error",
  "message": "Error description",
  "code": "ERROR_CODE"
}
```