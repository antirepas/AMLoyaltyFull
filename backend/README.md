# Appcult Backend
Multi-tenant loyalty platform API for Appcult. Businesses onboard customers, configure earn rules, run a rewards catalog, send push notifications, and (on Enterprise) connect Square/Clover POS so customers earn points from real orders.
**Production host:** `https://server.appcult.eu`  
**Module:** `appcult.com`  
**Stack:** Go 1.23 · Gin · PostgreSQL (pgx) · Redis · JWT · Goose migrations · AWS S3 · Expo push
---
## Table of contents
1. [What this server does](#what-this-server-does)
2. [Architecture](#architecture)
3. [Project layout](#project-layout)
4. [Getting started](#getting-started)
5. [Environment variables](#environment-variables)
6. [Database & migrations](#database--migrations)
7. [Authentication & roles](#authentication--roles)
8. [API reference](#api-reference)
9. [Loyalty engine](#loyalty-engine)
10. [POS integration](#pos-integration)
11. [QR codes](#qr-codes)
12. [Background jobs](#background-jobs)
13. [Deploy](#deploy)
14. [Testing](#testing)
15. [Known limitations](#known-limitations)
---
## What this server does
Appcult is a **per-business loyalty app**. Each business has its own customers, items, discounts, rules, and (optionally) POS connection.
| Capability | Description |
|------------|-------------|
| Customer loyalty | Earn points on purchases, spend on catalog rewards |
| Worker / employee app | Scan customer QR, award points, redeem rewards |
| Admin dashboard | Catalog, discounts, loyalty rules, locations, analytics events, notifications |
| Business plan | Employee enters order total manually → points |
| Enterprise plan | Square webhook + Redis matching → points from POS order total |
| Notifications | Expo push for points, rewards, scheduled campaigns, re-engagement |
| Auth | JWT access + refresh; password reset via email |
---
## Architecture
```
┌─────────────┐  ┌──────────────┐  ┌─────────────┐
│ Customer app│  │ Worker app   │  │ Admin web   │
└──────┬──────┘  └──────┬───────┘  └──────┬──────┘
       │                │                 │
       └────────────────┼─────────────────┘
                        │  HTTPS /api/v1
                        ▼
              ┌─────────────────────┐
              │  Gin HTTP server    │
              │  routes/ + middleware│
              └──────────┬──────────┘
                         │
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
   ┌──────────┐   ┌────────────┐   ┌─────────────┐
   │ loyalty/ │   │  models/   │   │ integration/│
   │  Engine  │   │  domain DB │   │  POS OAuth  │
   └────┬─────┘   └─────┬──────┘   └──────┬──────┘
        │               │                 │
        └───────────────┼─────────────────┘
                        ▼
              ┌─────────────────────┐
              │ PostgreSQL + Redis  │
              └─────────────────────┘
```
**Request flow**
1. `middleware` validates JWT (and admin/employee role when required).
2. `routes` bind JSON, map HTTP status codes.
3. Domain work lives in `loyalty/` (points) or `models/` (CRUD).
4. POS OAuth/order helpers live under `integration/provider/` and `utils/`.
**Multi-tenancy:** almost all data is scoped by `business_id`. Customers belong to one business; scanning the wrong business’s QR returns a conflict.
---
## Project layout
```
appcultBackend/
├── main.go                 # Gin server, CORS, crons, :8080
├── routes/                 # HTTP handlers & route registration
├── middleware/             # JWT, admin, employee, S3 upload
├── models/                 # Domain structs + SQL
├── loyalty/                # Points engine, ledger, POS matcher, expiry
├── integration/provider/   # Square / Clover OAuth + POS order ops
├── utils/                  # JWT, hash, QR, Square HTTP, email, crypto
├── db/
│   ├── db.go               # pgx pool + Redis client
│   └── migrations/         # Goose SQL migrations
├── templates/              # Password-reset HTML
├── static/                 # CSS for reset page
├── docs/loyalty-engine.md  # Deep dive on loyalty
├── k8s/                    # Kubernetes manifests
├── Dockerfile
├── docker-compose.yml
└── Makefile
```
---
## Getting started
### Prerequisites
- Go 1.23+
- PostgreSQL 16+
- Redis 7+
- [Goose](https://github.com/pressly/goose) for migrations
- Optional: [Air](https://github.com/air-verse/air) for hot reload (`make run`)
### Local setup
1. Clone and enter the repo.
2. Create a `.env` (see [Environment variables](#environment-variables)).
3. Start Postgres + Redis (example via Compose):
   ```bash
   docker compose up -d postgres redis
   ```
4. Run migrations:
   ```bash
   make migrate-up
   # or: goose -env .env -dir ./db/migrations up
   ```
5. Run the API:
   ```bash
   go run .
   # or: make run   # Air
   # or: make build && ./myapp
   ```
Server listens on `0.0.0.0:8080`. Base path: `/api/v1`.
### Docker (full stack)
```bash
docker compose up --build
```
Compose wires `app`, `postgres`, and `redis`. Adjust `DATABASE_URL` / Redis address for your environment (see notes under Redis below).
---
## Environment variables
| Variable | Purpose |
|----------|---------|
| `DATABASE_URL` | PostgreSQL connection string (required) |
| `ACCESS_TOKEN_SECRET` | JWT access token signing key |
| `REFRESH_TOKEN_SECRET` | JWT refresh token signing key |
| `ENC_KEY` | Base64 AES key for encrypting POS tokens |
| `AWS_REGION` | S3 region for item images |
| `AWS_ACCESS_KEY_ID` | S3 credentials |
| `AWS_SECRET_ACCESS_KEY` | S3 credentials |
| `EMAIL_USER` | SMTP user for password reset |
| `EMAIL_PASS` | SMTP password |
| `SQUARE_APPLICATION_ID` | Square OAuth app id |
| `SQUARE_APPLICATION_SECRET` | Square OAuth secret |
| `CLOVER_APP_ID` | Clover (and currently Square provider env reuse) app id |
