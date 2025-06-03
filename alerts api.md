# Alert API Documentation

This document describes the Alert API endpoints for the EdgeTrack backend.

---

## Base URL

```
/api/alerts
```

---

## Authentication

All endpoints require authentication via a JWT token in the `Authorization` header:

```
Authorization: Bearer <token>
```

---

## Endpoints

### 1. Get All Alerts

- **GET** `/api/alerts`
- **Description:** Retrieve all alerts visible to the authenticated user.
- **Access:** All authenticated users. Parents only see alerts for their children.
- **Response Example:**
    ```json
    {
      "status": "success",
      "data": [
        {
          "_id": "665d1c...",
          "athlete": {
            "_id": "665d1b...",
            "name": "Jane Doe",
            "role": "athlete",
            "team": "665d1a..."
          },
          "severity": "High",
          "injuryName": "Sprained Ankle",
          "startDate": "2025-06-01T00:00:00.000Z",
          "estimatedRecovery": "2025-06-15T00:00:00.000Z",
          "recoveryProgress": 40,
          "notes": [],
          "createdAt": "2025-06-03T12:00:00.000Z"
        }
      ]
    }
    ```

---

### 2. Create an Alert

- **POST** `/api/alerts`
- **Description:** Create a new alert for an athlete.
- **Access:** Only users with role `coach` or `manager`.
- **Request Body Example:**
    ```json
    {
      "athlete": "665d1b...",
      "severity": "High",
      "injuryName": "Sprained Ankle",
      "startDate": "2025-06-01",
      "estimatedRecovery": "2025-06-15",
      "recoveryProgress": 0,
      "notes": [
        {
          "type": "Coach",
          "author": "665d1c...",
          "content": "Initial assessment.",
          "date": "2025-06-01"
        }
      ]
    }
    ```
- **Response Example:**
    ```json
    {
      "status": "success",
      "data": {
        "_id": "665d1c...",
        "athlete": "665d1b...",
        "severity": "High",
        "injuryName": "Sprained Ankle",
        "startDate": "2025-06-01T00:00:00.000Z",
        "estimatedRecovery": "2025-06-15T00:00:00.000Z",
        "recoveryProgress": 0,
        "notes": [
          {
            "type": "Coach",
            "author": "665d1c...",
            "content": "Initial assessment.",
            "date": "2025-06-01T00:00:00.000Z"
          }
        ],
        "createdAt": "2025-06-03T12:00:00.000Z"
      }
    }
    ```
- **Error Response (Unauthorized):**
    ```json
    {
      "message": "Not authorized to create alerts"
    }
    ```

---

### 3. Get Alert by ID

- **GET** `/api/alerts/:id`
- **Description:** Retrieve a specific alert by its ID.
- **Access:** All authenticated users. Parents can only access alerts for their children.
- **Response Example:**
    ```json
    {
      "status": "success",
      "data": {
        "_id": "665d1c...",
        "athlete": {
          "_id": "665d1b...",
          "name": "Jane Doe",
          "role": "athlete",
          "team": "665d1a..."
        },
        "severity": "High",
        "injuryName": "Sprained Ankle",
        "startDate": "2025-06-01T00:00:00.000Z",
        "estimatedRecovery": "2025-06-15T00:00:00.000Z",
        "recoveryProgress": 40,
        "notes": [],
        "createdAt": "2025-06-03T12:00:00.000Z"
      }
    }
    ```
- **Error Response (Not Found):**
    ```json
    {
      "message": "Alert not found"
    }
    ```
- **Error Response (Unauthorized):**
    ```json
    {
      "message": "Not authorized to view this alert"
    }
    ```

---

### 4. Update an Alert

- **PATCH** `/api/alerts/:id`
- **Description:** Update an existing alert.
- **Access:** Only users with role `coach` or `manager`.
- **Request Body Example:**
    ```json
    {
      "recoveryProgress": 60,
      "notes": [
        {
          "type": "Physician",
          "author": "665d1d...",
          "content": "Cleared for light activity.",
          "date": "2025-06-10"
        }
      ]
    }
    ```
- **Response Example:**
    ```json
    {
      "status": "success",
      "data": {
        "_id": "665d1c...",
        "athlete": "665d1b...",
        "severity": "High",
        "injuryName": "Sprained Ankle",
        "startDate": "2025-06-01T00:00:00.000Z",
        "estimatedRecovery": "2025-06-15T00:00:00.000Z",
        "recoveryProgress": 60,
        "notes": [
          {
            "type": "Physician",
            "author": "665d1d...",
            "content": "Cleared for light activity.",
            "date": "2025-06-10T00:00:00.000Z"
          }
        ],
        "createdAt": "2025-06-03T12:00:00.000Z"
      }
    }
    ```
- **Error Response (Unauthorized):**
    ```json
    {
      "message": "Not authorized to update alerts"
    }
    ```
- **Error Response (Not Found):**
    ```json
    {
      "message": "Alert not found"
    }
    ```

---

### 5. Delete an Alert

- **DELETE** `/api/alerts/:id`
- **Description:** Delete an alert by its ID.
- **Access:** Only users with role `coach` or `manager`.
- **Response Example:**
    ```json
    {
      "status": "success",
      "data": null
    }
    ```
- **Error Response (Unauthorized):**
    ```json
    {
      "message": "Not authorized to delete alerts"
    }
    ```

---

## Alert Object Schema

```json
{
  "_id": "string",
  "athlete": "UserId or User object",
  "severity": "Low | Medium | High",
  "injuryName": "string",
  "startDate": "ISODate",
  "estimatedRecovery": "ISODate",
  "recoveryProgress": "number (0-100)",
  "notes": [
    {
      "type": "Physician | Coach | Parent",
      "author": "UserId",
      "content": "string",
      "date": "ISODate"
    }
  ],
  "createdAt": "ISODate"
}
```

---

## Notes

- All endpoints require authentication.
- Only `coach` and `manager` roles can create, update, or delete alerts.
- Parents can only view alerts for their children.
- Error responses are returned with appropriate HTTP status codes and a `message` field.

---
