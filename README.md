# irctc-backend-
# IRCTC Clone

> A full-stack clone of the Indian Railways ticketing experience (IRCTC) built for learning and demonstration purposes.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Tech Stack](#tech-stack)
4. [Architecture & Folder Structure](#architecture--folder-structure)
5. [Getting Started (Local Development)](#getting-started-local-development)

   * Prerequisites
   * Environment variables
   * Install & Run
6. [Database & Seeding](#database--seeding)
7. [API Endpoints (summary)](#api-endpoints-summary)
8. [Authentication & Security](#authentication--security)
9. [Testing](#testing)
10. [Deployment](#deployment)
11. [Roadmap / Next Steps](#roadmap--next-steps)
12. [Contributing](#contributing)
13. [License](#license)

---

## Project Overview

This repository contains an IRCTC-like application that simulates searching trains, checking seat availability, booking tickets, viewing PNR, and administrative actions. The goal is educational: to practice building a production-like booking flow with a full-stack codebase, including UI, RESTful APIs, database modeling, authentication, and background jobs.

> **Note:** This is not meant to be a production replacement for IRCTC. Use responsibly and only for learning or demo purposes.

---

## Features

* User registration, login (email/password) and profile management
* Train search by source/destination/date and class
* Seat availability & quota simulation
* Booking flow: select train → select coach/class → passenger details → payment simulation → confirmation
* PNR generation & view
* Ticket cancellation & refund simulation
* Admin dashboard: add/edit trains, schedules, coaches and view bookings
* Notifications (email / SMS simulation) for booking status
* Booking history and invoice generation (PDF export)
* Rate limiting and basic input validation

---

## Tech Stack

**Frontend**

* React (Create React App / Next.js) + Tailwind CSS
* React Query or SWR for data fetching
* React Hook Form + Yup for validation

**Backend**

* Node.js + Express
* TypeScript (optional) or JavaScript
* Database: PostgreSQL (recommended) or MongoDB
* ORM: Prisma (for Postgres) or Mongoose (for MongoDB)
* Authentication: JWT + secure refresh tokens
* Email: Nodemailer (SMTP or Mailtrap for dev)
* Payments: Mock gateway or Stripe (test mode)
* Background jobs: BullMQ + Redis (for email/notifications)

**Dev & Infra**

* Docker & Docker Compose for local dev
* GitHub Actions for CI (tests & lint)
* PM2 or Docker for production

---

## Architecture & Folder Structure

```
/ (repo root)
├─ /client             # React frontend
│  ├─ public
│  └─ src
│     ├─ pages/components
│     └─ services (api client)
├─ /server             # Express backend
│  ├─ src
│  │  ├─ controllers
│  │  ├─ models (Prisma schema / Mongoose models)
│  │  ├─ routes
│  │  ├─ services (business logic)
│  │  ├─ jobs (background jobs)
│  │  ├─ utils (email, pdf, helpers)
│  │  └─ index.js
├─ /infra              # Docker, scripts, migrations
├─ docker-compose.yml
├─ .env.example
└─ README.md
```

---

## Getting Started (Local Development)

### Prerequisites

* Node.js (v16+ recommended)
* npm or yarn
* PostgreSQL (or MongoDB)
* Redis (optional for background jobs)
* Docker (optional but recommended)

### Environment variables

Create a `.env` file in `/server` and `/client` (as needed). Example `.env` for the server:

```
PORT=4000
NODE_ENV=development
DATABASE_URL=postgresql://user:password@localhost:5432/irctc_clone
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRY=1d
REFRESH_TOKEN_SECRET=your_refresh_secret
SMTP_HOST=smtp.mailtrap.io
SMTP_PORT=2525
SMTP_USER=mailtrap_user
SMTP_PASS=mailtrap_pass
REDIS_URL=redis://localhost:6379
STRIPE_SECRET_KEY=sk_test_...
```

> Rename `.env.example` to `.env` and fill values.

### Install & Run

**Server**

```bash
cd server
npm install
# Run DB migrations (if using Prisma)
npx prisma migrate dev --name init
# or run seed script
npm run seed
# Start dev server
npm run dev
```

**Client**

```bash
cd client
npm install
npm run dev
```

**With Docker Compose** (if `docker-compose.yml` provided)

```bash
docker-compose up --build
```

---

## Database & Seeding

* Use Prisma to model the following main entities: `User`, `Train`, `Coach`, `Seat`, `Schedule`, `Booking`, `Passenger`, `PNR`, `Transaction`.
* Provide seed scripts to populate: sample trains, routes, classes (1A, 2A, 3A, SL), sample users, and a few bookings.

---

## API Endpoints (summary)

> Base: `POST http://localhost:4000/api`

**Auth**

* `POST /auth/register` — register new user
* `POST /auth/login` — login and return access + refresh tokens
* `POST /auth/refresh` — refresh access token
* `POST /auth/logout` — revoke refresh token

**Trains & Search**

* `GET /trains/search?from=ABC&to=XYZ&date=YYYY-MM-DD` — search trains
* `GET /trains/:id` — train details & schedule
* `POST /trains/availability` — check seat availability (body: trainId, date, class)

**Booking**

* `POST /bookings/create` — create booking (returns temporary booking + payment intent)
* `POST /bookings/confirm` — confirm booking after payment (generates PNR)
* `GET /bookings/:pnr` — get booking by PNR
* `POST /bookings/cancel` — cancel a booking (refund simulated)

**Admin** (protected routes)

* `POST /admin/trains` — create train
* `PUT /admin/trains/:id` — update train
* `GET /admin/bookings` — list all bookings

> All protected routes require an `Authorization: Bearer <token>` header.

---

## Authentication & Security

* Passwords hashed with bcrypt (or argon2)
* JWT for stateless access tokens + refresh tokens stored server-side or in DB
* Rate limiting (express-rate-limit) on sensitive endpoints
* Input validation with Joi / Yup
* Use HTTPS in production; store secrets with environment-specific secret manager

---

## Testing

* Unit tests: Jest (backend), React Testing Library (frontend)
* Integration tests: Supertest for API routes

Example test command:

```bash
cd server
npm run test
```

---

## Deployment

* Build client for production (`npm run build`), serve with static server or Vercel/Netlify
* Backend can be deployed to: Heroku, Render, Railway, DigitalOcean App Platform, or any container host
* Use Dockerfile for both client and server and orchestrate with Docker Compose in prod
* Use managed DB (e.g., AWS RDS / Neon / Railway Postgres) and managed Redis for queues

---

## Roadmap / Next Steps

* Add real-time seat allocation with optimistic locking
* Integrate real payment gateway (Stripe/PayU) in sandbox/test mode
* Add seat map UI to pick exact berth
* Add multi-lingual support
* Improve accessibility & mobile responsiveness

---

---

## Acknowledgements

This project uses ideas and patterns commonly used in ticketing/booking systems. Thanks to open-source libraries and communities.

---

*Generated README — customize to match your actual repo structure and implementation details.*
