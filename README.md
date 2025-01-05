# Event Management REST API

This project is a Go-based REST API built using the Gin framework. It provides functionalities for managing events, users, and registrations, interacting with a SQLite database. Users can create events, update or delete them, and register for events.

---

## Features

- **User Management**:
  - Sign up new users.
  - Authenticate users with login.

- **Event Management**:
  - Create, update, and delete events.
  - Retrieve all events or fetch details of a specific event.

- **Registrations**:
  - Register for events.
  - Cancel registrations.

- **Middleware**:
  - Authentication middleware using JWT tokens.

---

## Installation

1. **Clone the Repository:**
```bash
git clone https://github.com/wu7115/go-rest-api.git
cd repo
```

2. **Install Dependencies:**
```bash
go mod tidy
```

3. **Run the Server:**
```bash
go run main.go
```

The server will run at `http://localhost:8080`.

---

## Endpoints

### Authentication
- **POST /signup** - Register a new user.
- **POST /login** - Authenticate a user and receive a JWT token.

### Events
- **GET /events** - Fetch all events.
- **GET /events/:id** - Fetch a specific event by ID.
- **POST /events** *(Authenticated)* - Create a new event.
- **PUT /events/:id** *(Authenticated)* - Update an existing event.
- **DELETE /events/:id** *(Authenticated)* - Delete an event.

### Registrations
- **POST /events/:id/register** *(Authenticated)* - Register for an event.
- **DELETE /events/:id/register** *(Authenticated)* - Cancel event registration.

---

## Database Schema

### Users Table
```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  email TEXT NOT NULL UNIQUE,
  password TEXT NOT NULL
);
```

### Events Table
```sql
CREATE TABLE events (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  description TEXT NOT NULL,
  location TEXT NOT NULL,
  dateTime DATETIME NOT NULL,
  user_id INTEGER,
  FOREIGN KEY(user_id) REFERENCES users(id)
);
```

### Registrations Table
```sql
CREATE TABLE registrations (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  event_id INTEGER,
  user_id INTEGER,
  FOREIGN KEY(event_id) REFERENCES events(id),
  FOREIGN KEY(user_id) REFERENCES users(id)
);
```

---

## Technologies

- **Language:** Go
- **Framework:** Gin
- **Database:** SQLite

---

## Setup Instructions

1. **Initialize Database:**
   - Database is automatically created as `api.db` when the server starts.

2. **Environment Variables:**
   - Configure JWT secret keys in `utils` package for secure token handling.

---

## Authentication

- Uses JWT tokens for securing endpoints.
- Tokens must be passed in the `Authorization` header with the prefix `Bearer`.

Example Header:
```
Authorization: Bearer <token>
```

---

## Example Requests
Uses Live Preview on VS Code for testing requests.

### Create User
```bash
POST http://localhost:8080/signup
Content-Type: application/json

{
    "email": "email@gmail.com",
    "password": "password"
}
```

### Login
```bash
POST http://localhost:8080/login
Content-Type: application/json

{
    "email": "email@gmail.com",
    "password": "password"
}
```

### Create Event
```bash
curl -X POST http://localhost:8080/events \
-H "Authorization: Bearer <token>" \
-H "Content-Type: application/json" \
-d '{"name": "New Event", "description": "Event Details", "location": "Online", "dateTime": "2024-01-01T10:00:00Z"}'
```

### Register for Event
```bash
POST http://localhost:8080/events
content-type: application/json
authorization: someTokenGeneratedWhenCreatingUser

{
    "name": "Name",
    "description": "Descript",
    "location": "Location",
    "dateTime": "2025-01-01T15:30:00.000Z"
}
```

---

## Error Handling

- Returns appropriate HTTP status codes for errors:
  - 400: Bad Request.
  - 401: Unauthorized.
  - 500: Internal Server Error.

---

## Contributing

1. Fork the repository.
2. Create a new feature branch.
3. Commit your changes.
4. Submit a pull request.

---

## License

This project is licensed under the MIT License.

