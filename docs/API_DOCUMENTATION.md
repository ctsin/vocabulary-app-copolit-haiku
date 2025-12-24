# API Documentation

## Overview

This document provides comprehensive documentation for all API endpoints available in the Vocabulary App. The API follows REST principles and returns JSON responses.

## Base URL

```
https://api.vocabulary-app.example.com/v1
```

## Authentication

All API endpoints require authentication using a Bearer token in the Authorization header:

```
Authorization: Bearer <your_api_token>
```

## Response Format

All API responses follow a standard format:

### Success Response (2xx)
```json
{
  "success": true,
  "data": {},
  "message": "Operation completed successfully"
}
```

### Error Response (4xx, 5xx)
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message"
  }
}
```

## API Endpoints

### Vocabulary Management

#### 1. Get All Vocabularies

**Endpoint:** `GET /vocabularies`

**Description:** Retrieve a list of all vocabularies

**Query Parameters:**
- `page` (optional): Page number for pagination (default: 1)
- `limit` (optional): Items per page (default: 20, max: 100)
- `search` (optional): Search vocabularies by name
- `sort` (optional): Sort field (name, created_at, updated_at)
- `order` (optional): Sort order (asc, desc)

**Response:**
```json
{
  "success": true,
  "data": {
    "vocabularies": [
      {
        "id": "vocab_123",
        "name": "Business Terms",
        "description": "Common business vocabulary",
        "language": "en",
        "word_count": 150,
        "created_at": "2025-01-15T10:30:00Z",
        "updated_at": "2025-01-20T14:22:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 42,
      "total_pages": 3
    }
  }
}
```

**Status Codes:**
- `200 OK`: Successfully retrieved vocabularies
- `401 Unauthorized`: Invalid or missing authentication token
- `400 Bad Request`: Invalid query parameters

---

#### 2. Get Vocabulary by ID

**Endpoint:** `GET /vocabularies/{id}`

**Description:** Retrieve a specific vocabulary and its metadata

**Path Parameters:**
- `id` (required): Vocabulary ID

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "vocab_123",
    "name": "Business Terms",
    "description": "Common business vocabulary",
    "language": "en",
    "word_count": 150,
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-01-20T14:22:00Z",
    "tags": ["business", "professional"]
  }
}
```

**Status Codes:**
- `200 OK`: Successfully retrieved vocabulary
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Vocabulary not found

---

#### 3. Create Vocabulary

**Endpoint:** `POST /vocabularies`

**Description:** Create a new vocabulary

**Request Body:**
```json
{
  "name": "Business Terms",
  "description": "Common business vocabulary",
  "language": "en",
  "tags": ["business", "professional"]
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "vocab_123",
    "name": "Business Terms",
    "description": "Common business vocabulary",
    "language": "en",
    "word_count": 0,
    "created_at": "2025-01-20T14:22:00Z",
    "updated_at": "2025-01-20T14:22:00Z"
  },
  "message": "Vocabulary created successfully"
}
```

**Status Codes:**
- `201 Created`: Vocabulary created successfully
- `400 Bad Request`: Invalid request body
- `401 Unauthorized`: Invalid or missing authentication token
- `409 Conflict`: Vocabulary name already exists

---

#### 4. Update Vocabulary

**Endpoint:** `PUT /vocabularies/{id}`

**Description:** Update an existing vocabulary

**Path Parameters:**
- `id` (required): Vocabulary ID

**Request Body:**
```json
{
  "name": "Updated Business Terms",
  "description": "Updated description",
  "tags": ["business", "professional", "new"]
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "vocab_123",
    "name": "Updated Business Terms",
    "description": "Updated description",
    "language": "en",
    "word_count": 150,
    "updated_at": "2025-01-21T09:15:00Z"
  },
  "message": "Vocabulary updated successfully"
}
```

**Status Codes:**
- `200 OK`: Vocabulary updated successfully
- `400 Bad Request`: Invalid request body
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Vocabulary not found
- `409 Conflict`: Vocabulary name already exists

---

#### 5. Delete Vocabulary

**Endpoint:** `DELETE /vocabularies/{id}`

**Description:** Delete a vocabulary and all its associated words

**Path Parameters:**
- `id` (required): Vocabulary ID

**Response:**
```json
{
  "success": true,
  "message": "Vocabulary deleted successfully"
}
```

**Status Codes:**
- `200 OK`: Vocabulary deleted successfully
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Vocabulary not found

---

### Word Management

#### 6. Get Words in Vocabulary

**Endpoint:** `GET /vocabularies/{id}/words`

**Description:** Retrieve all words in a vocabulary

**Path Parameters:**
- `id` (required): Vocabulary ID

**Query Parameters:**
- `page` (optional): Page number for pagination (default: 1)
- `limit` (optional): Items per page (default: 50, max: 500)
- `search` (optional): Search words by term or definition
- `sort` (optional): Sort field (term, difficulty, created_at)
- `order` (optional): Sort order (asc, desc)

**Response:**
```json
{
  "success": true,
  "data": {
    "words": [
      {
        "id": "word_456",
        "term": "Synergy",
        "definition": "The interaction of elements that produces a combined effect greater than the sum of individual elements",
        "example": "The team's synergy resulted in outstanding project completion",
        "difficulty": "intermediate",
        "part_of_speech": "noun",
        "created_at": "2025-01-16T11:20:00Z",
        "updated_at": "2025-01-16T11:20:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 50,
      "total": 150,
      "total_pages": 3
    }
  }
}
```

**Status Codes:**
- `200 OK`: Successfully retrieved words
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Vocabulary not found

---

#### 7. Get Word Details

**Endpoint:** `GET /vocabularies/{vocab_id}/words/{word_id}`

**Description:** Retrieve details of a specific word

**Path Parameters:**
- `vocab_id` (required): Vocabulary ID
- `word_id` (required): Word ID

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "word_456",
    "term": "Synergy",
    "definition": "The interaction of elements that produces a combined effect greater than the sum of individual elements",
    "example": "The team's synergy resulted in outstanding project completion",
    "difficulty": "intermediate",
    "part_of_speech": "noun",
    "synonyms": ["cooperation", "collaboration"],
    "antonyms": ["conflict", "discord"],
    "etymology": "From Greek syn- (together) + ergos (work)",
    "pronunciation": "SIN-er-jee",
    "created_at": "2025-01-16T11:20:00Z",
    "updated_at": "2025-01-16T11:20:00Z"
  }
}
```

**Status Codes:**
- `200 OK`: Successfully retrieved word
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Word or vocabulary not found

---

#### 8. Add Word to Vocabulary

**Endpoint:** `POST /vocabularies/{id}/words`

**Description:** Add a new word to a vocabulary

**Path Parameters:**
- `id` (required): Vocabulary ID

**Request Body:**
```json
{
  "term": "Synergy",
  "definition": "The interaction of elements that produces a combined effect greater than the sum of individual elements",
  "example": "The team's synergy resulted in outstanding project completion",
  "difficulty": "intermediate",
  "part_of_speech": "noun",
  "synonyms": ["cooperation", "collaboration"],
  "antonyms": ["conflict", "discord"],
  "etymology": "From Greek syn- (together) + ergos (work)",
  "pronunciation": "SIN-er-jee"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "word_456",
    "term": "Synergy",
    "definition": "The interaction of elements that produces a combined effect greater than the sum of individual elements",
    "example": "The team's synergy resulted in outstanding project completion",
    "difficulty": "intermediate",
    "part_of_speech": "noun",
    "created_at": "2025-01-20T14:30:00Z",
    "updated_at": "2025-01-20T14:30:00Z"
  },
  "message": "Word added successfully"
}
```

**Status Codes:**
- `201 Created`: Word created successfully
- `400 Bad Request`: Invalid request body
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Vocabulary not found
- `409 Conflict`: Word already exists in vocabulary

---

#### 9. Update Word

**Endpoint:** `PUT /vocabularies/{vocab_id}/words/{word_id}`

**Description:** Update an existing word

**Path Parameters:**
- `vocab_id` (required): Vocabulary ID
- `word_id` (required): Word ID

**Request Body:**
```json
{
  "definition": "Updated definition",
  "example": "Updated example",
  "difficulty": "advanced"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "word_456",
    "term": "Synergy",
    "definition": "Updated definition",
    "example": "Updated example",
    "difficulty": "advanced",
    "updated_at": "2025-01-21T10:45:00Z"
  },
  "message": "Word updated successfully"
}
```

**Status Codes:**
- `200 OK`: Word updated successfully
- `400 Bad Request`: Invalid request body
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Word or vocabulary not found

---

#### 10. Delete Word

**Endpoint:** `DELETE /vocabularies/{vocab_id}/words/{word_id}`

**Description:** Delete a word from a vocabulary

**Path Parameters:**
- `vocab_id` (required): Vocabulary ID
- `word_id` (required): Word ID

**Response:**
```json
{
  "success": true,
  "message": "Word deleted successfully"
}
```

**Status Codes:**
- `200 OK`: Word deleted successfully
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Word or vocabulary not found

---

### Learning Progress

#### 11. Get User Progress

**Endpoint:** `GET /users/progress`

**Description:** Retrieve user's learning progress

**Query Parameters:**
- `vocabulary_id` (optional): Filter by specific vocabulary

**Response:**
```json
{
  "success": true,
  "data": {
    "total_vocabularies": 5,
    "total_words_learned": 342,
    "average_difficulty": "intermediate",
    "vocabularies": [
      {
        "vocabulary_id": "vocab_123",
        "vocabulary_name": "Business Terms",
        "words_learned": 45,
        "words_total": 150,
        "progress_percentage": 30,
        "last_studied": "2025-01-20T14:22:00Z"
      }
    ]
  }
}
```

**Status Codes:**
- `200 OK`: Successfully retrieved progress
- `401 Unauthorized`: Invalid or missing authentication token

---

#### 12. Record Word Study

**Endpoint:** `POST /users/progress/words/{word_id}`

**Description:** Record that a user studied a word

**Path Parameters:**
- `word_id` (required): Word ID

**Request Body:**
```json
{
  "correct": true,
  "time_spent_seconds": 30,
  "difficulty_rating": 3
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "word_id": "word_456",
    "learned": true,
    "confidence_level": "high",
    "timestamp": "2025-01-21T15:30:00Z"
  },
  "message": "Study record saved successfully"
}
```

**Status Codes:**
- `200 OK`: Study record saved successfully
- `400 Bad Request`: Invalid request body
- `401 Unauthorized`: Invalid or missing authentication token
- `404 Not Found`: Word not found

---

## Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_TOKEN` | 401 | Authentication token is invalid or expired |
| `MISSING_TOKEN` | 401 | No authentication token provided |
| `NOT_FOUND` | 404 | Resource not found |
| `INVALID_REQUEST` | 400 | Request body or parameters are invalid |
| `DUPLICATE_RESOURCE` | 409 | Resource already exists |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests from this IP |
| `INTERNAL_SERVER_ERROR` | 500 | An unexpected server error occurred |

---

## Rate Limiting

API requests are rate-limited to prevent abuse:

- **Standard users**: 1,000 requests per hour
- **Premium users**: 10,000 requests per hour

Rate limit information is included in response headers:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 950
X-RateLimit-Reset: 1642773000
```

---

## Pagination

List endpoints support pagination using the following parameters:

- `page`: Page number (starts from 1)
- `limit`: Number of items per page

Example:
```
GET /vocabularies?page=2&limit=25
```

---

## Sorting

List endpoints support sorting using:

- `sort`: Field to sort by
- `order`: Sort direction (asc or desc)

Example:
```
GET /vocabularies?sort=name&order=asc
```

---

## Changelog

### Version 1.0.0 (2025-01-20)
- Initial API release
- Vocabulary management endpoints
- Word management endpoints
- User progress tracking endpoints
