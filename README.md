# LinkBin

LinkBin is a full-stack MERN URL shortener and paste sharing platform. It is built as an interview-ready backend-focused project that demonstrates REST API design, authentication, Redis caching, rate limiting, expiry handling, analytics, API documentation, testing, Docker, and CI.

## Features

### URL Shortener

- Create short URLs from long URLs
- Generate random short codes
- Create custom aliases
- Redirect users to the original URL
- Disable or delete short URLs
- Set expiry time for short links
- Generate QR codes for short URLs
- Track detailed click analytics

### Paste Sharing

- Create and share text, code, logs, JSON, or notes
- Public, unlisted, and private paste visibility
- Syntax highlighting and readable paste view
- Copy paste content
- Set paste expiry time
- Disable or delete pastes
- Track lightweight paste view analytics

### Authentication And Dashboard

- Register and login with JWT authentication
- Secure password hashing with bcrypt
- Protected dashboard routes
- Manage URLs and pastes from a user dashboard
- View URL analytics and paste analytics

## Tech Stack

| Layer | Technology |
| --- | --- |
| Frontend | React, React Router, Axios, Vite |
| Backend | Node.js, Express.js |
| Database | MongoDB, Mongoose |
| Cache / Rate Limiting | Redis |
| Auth | JWT, bcrypt |
| Validation | Zod |
| API Docs | Swagger / OpenAPI |
| Testing | Jest, Supertest, mongodb-memory-server |
| DevOps | Docker, Docker Compose, GitHub Actions |

## Architecture

```text
React Frontend
    |
    v
Express API
    |
    |-- MongoDB: users, URLs, pastes, analytics
    |-- Redis: short URL lookup cache
    |-- Redis: rate limiting
```

## Redis Usage

LinkBin uses Redis in two important places:

1. **Cache-aside redirects**
   - The backend checks Redis before MongoDB for short URL redirects.
   - Frequently accessed short URLs avoid repeated MongoDB lookups.
   - On cache miss, MongoDB is queried and Redis is updated.

2. **Rate limiting**
   - Redis-backed rate limits protect auth, URL creation, paste creation, and redirect endpoints.
   - This helps prevent spam, brute-force login attempts, and API abuse.

## Analytics

URL analytics include:

- Total clicks
- Unique visitors
- Clicks by date
- Referrer
- Browser and device
- Approximate country/city
- Last clicked time

Paste analytics include:

- Total views
- Unique views
- Views by date
- Last viewed time

## API Overview

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | `/api/auth/register` | Register user |
| POST | `/api/auth/login` | Login user |
| GET | `/api/auth/me` | Get current user |
| POST | `/api/urls` | Create short URL |
| GET | `/api/urls` | List user's short URLs |
| GET | `/:shortCode` | Redirect short URL |
| POST | `/api/pastes` | Create paste |
| GET | `/api/pastes` | List user's pastes |
| GET | `/p/:slug` | View public/unlisted paste |
| GET | `/api/dashboard/summary` | Dashboard summary |
| GET | `/api/docs` | Swagger API docs |

## Local Setup

### Run With Docker Compose

Create backend environment file:

```bash
cd backend
cp .env.example .env
cd ..
```

Start the full stack:

```bash
docker compose up --build
```

Services:

```text
Frontend: http://localhost:5173
Backend:  http://localhost:5000
MongoDB:  localhost:27018
Redis:    localhost:6379
```

### Run Backend Manually

MongoDB and Redis should already be running.

```bash
cd backend
npm install
cp .env.example .env
npm run dev
```

### Run Frontend Manually

```bash
cd frontend
npm install
cp .env.example .env
npm run dev
```

## Environment Variables

Backend:

```env
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/linkbin
JWT_SECRET=replace_with_strong_secret
JWT_EXPIRES_IN=7d
CLIENT_URL=http://localhost:5173
REDIS_URL=redis://localhost:6379
BASE_URL=http://localhost:5000
```

Frontend:

```env
VITE_API_URL=http://localhost:5000
```

## Testing

Run backend API integration tests:

```bash
cd backend
npm test
```

The backend tests use Jest and Supertest to validate Express API behavior, authentication flows, validation, and database-backed routes.

## CI

GitHub Actions is configured to validate the project on push/PR. The pipeline checks backend tests, frontend builds, and Docker image builds.


## This project demonstrates:

- REST API design with Express.js
- MongoDB schema design and indexing
- JWT authentication and protected routes
- Redis cache-aside pattern
- Redis-backed rate limiting
- Expiry handling for URLs and pastes
- Analytics/event tracking
- Swagger API documentation
- Docker Compose orchestration
- Backend API integration testing
- GitHub Actions CI
