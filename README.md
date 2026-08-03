# User System

## Project Overview

This project is a user management system based on Spring Boot 3, supporting user registration, login, information management, role-based access control, and data export. The backend uses PostgreSQL as the main database and Redis for caching and distributed scenarios. JWT is used for stateless authentication and role-based authorization.

## Tech Stack

### Backend

* Java 17
* Spring Boot 3.5.4
* MyBatis
* PostgreSQL 17.x / MySQL 8.0.x
* Redis
* Spring Security
* EasyExcel (Alibaba Excel export)
* Lombok
* JJWT (JWT token)
* Maven 3.9.x

### Frontend

* Vue 3
* TypeScript
* Vite
* Arco Design Vue
* Vue Router
* Pinia
* Axios

## Project Structure

```
backend/
  src/
    main/
      java/com/bryan/system/
        config/         # Configuration classes (security, Redis, MyBatis, etc.)
        controller/     # RESTful controllers
        domain/         # Entities, request/response objects, VO
        filter/         # JWT authentication filter
        handler/        # Global exception handler
        mapper/         # MyBatis mapper interfaces
        service/        # Service layer
        util/           # Utility classes (JWT, HTTP, etc.)
      resources/
        application.yaml
        application-dev.yaml
        application-mysql.yaml
        mapper/         # MyBatis mapper xmls
    test/
      java/com/bryan/system/
  pom.xml
  mvnw

frontend/
  src/
    api/               # API request modules
    assets/            # Static assets
    components/        # Shared components
    views/             # Page-level Vue components
    router/            # Vue Router configuration
    store/             # Pinia state management
    utils/             # Utility functions
    App.vue
    main.ts
  package.json
  tsconfig.json
  vite.config.ts
```

## Requirements

* JDK 17+
* Maven 3.9.9+
* Node.js 18+
* PostgreSQL 17.x / MySQL 8.0.x
* Redis 6.x or above

## Configuration

* Update database and Redis settings in `backend/src/main/resources/application-dev.yaml`.
* General settings (logging, MyBatis, etc.) are in `backend/src/main/resources/application.yaml`.
* Database schema scripts are in [`sql/create_table.sql`](sql/create_table.sql).

## Getting Started

### Backend

> Using PostgreSQL as example

1. Initialize the PostgreSQL database by running the schema script:

   ```sh
   psql -U postgres -d postgres -f sql/create_table.sql
   ```

2. Start the Redis service.
3. Build and run the project with Maven:

   ```sh
   cd backend
   ./mvnw spring-boot:run
   ```

   Or run the packaged jar:

   ```sh
   cd backend
   ./mvnw clean package
   java -jar target/user-system-0.0.1-SNAPSHOT.jar
   ```

### Frontend

1. Install dependencies:

   ```sh
   cd frontend
   npm install
   ```

2. Start the development server:

   ```sh
   npm run dev
   ```

3. Build for production:

   ```sh
   npm run build
   ```

The dev server runs on `http://localhost:5173` by default. Make sure the backend is running and CORS is configured to allow requests from the frontend origin.

## Main APIs

### Auth

* User registration: `POST /api/auth/register`
* User login: `POST /api/auth/login`
* Validate token: `GET /api/auth/validate`
* Get current user: `GET /api/auth/me`
* Change password: `PUT /api/auth/password`
* Delete account: `DELETE /api/auth`
* Logout: `GET /api/auth/logout`

### Users

* Create user: `POST /api/users` (admin only)
* Get all users: `GET /api/users` (admin only)
* Get user by ID: `GET /api/users/:userId` (admin only)
* Get user by username: `GET /api/users/username/:username` (admin only)
* Search users: `POST /api/users/search` (admin only)
* Update user: `PUT /api/users/:userId`
* Change user roles: `PUT /api/users/roles/:userId` (admin only)
* Reset password: `PUT /api/users/password/:userId` (admin only)
* Block user: `PUT /api/users/block/:userId` (admin only)
* Unblock user: `PUT /api/users/unblock/:userId` (admin only)
* Delete user: `DELETE /api/users/:userId` (admin only)
* Export users: `GET /api/users/export` (admin only)

### User Profiles

* Upload avatar: `POST /api/user-profiles/avatar`
* Get profile by user ID: `GET /api/user-profiles/:userId`
* Get profile by real name: `GET /api/user-profiles/name/:realName`
* Get current user profile: `GET /api/user-profiles/me`
* Update profile: `PUT /api/user-profiles`

### Roles

* List roles: `GET /api/user-roles` (admin only)

### System Logs

* List latest logs: `GET /api/admin/logs` (admin only)
* List log files: `GET /api/admin/logs/files` (admin only)

## Notes

* For production, inject JWT secret via configuration file instead of hardcoding.
* Global exception handling is in [`GlobalExceptionHandler`](backend/src/main/java/com/bryan/system/handler/GlobalExceptionHandler.java).
* Logical delete field is `deleted`: 0 means active, 1 means deleted.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
