This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).
# Appcult Admin Dashboard
## Getting Started
Web admin panel for Appcult businesses. Manage menu items, loyalty rules, POS integrations, and view customer analytics — all against the Appcult backend API.
First, run the development server:
## Features
| Area | What you can do |
|------|-----------------|
| **Dashboard** | KPI overview: users, sessions, average session duration, QR scans; activity charts; loyalty point summary |
| **Statistics** | Session duration, item engagement, new users / logins, coupon purchases, QR scan participation |
| **CMS** | Create, edit, and delete menu items; apply discounts; manage push notifications |
| **Your Data** | Edit business profile, locations, POS signature, and loyalty rules |
| **Connect** | OAuth connect / disconnect for Square or Clover |
| **Auth** | Admin login and new business signup |
## Tech stack
- **Next.js 15** (App Router) + **React 19**
- **Chart.js** / **react-chartjs-2** (lazy-loaded)
- **Zustand** for client auth state
- **react-toastify** for feedback toasts
- CSS Modules + shared design tokens (`app/globals.css`)
- Poppins via `next/font`
## Prerequisites
- Node.js 18+
- Running Appcult backend (local or remote), e.g. [`appcultBackend`](../appcultBackend) on port `8080`
## Setup
```bash
npm install
```
Create `.env.local` in the project root:
```env
# Local backend
NEXT_PUBLIC_URL=http://localhost:8080/api/v1
# Or production
# NEXT_PUBLIC_URL=https://server.appcult.eu/api/v1
```
### Run
```bash
# Development (Turbopack)
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
# Production build
npm run build
npm start
```
Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.
Open [http://localhost:3000](http://localhost:3000).
You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.
### Create a test account
This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.
With the backend running:
## Learn More
```bash
node scripts/create-test-account.mjs
```
To learn more about Next.js, take a look at the following resources:
Optional overrides:
- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.
```bash
# Windows PowerShell
$env:TEST_EMAIL='you@example.com'; $env:TEST_PASSWORD='Test1234!'; node scripts/create-test-account.mjs
```
You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!
Then sign in at `/login`.
## Deploy on Vercel
## Authentication
The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.
1. Admin logs in via `POST {NEXT_PUBLIC_URL}/loginAdmin` with email + password.
2. Frontend stores the token / business ID in Zustand (localStorage) and sets **httpOnly cookies** via `POST /api/setSession`.
3. [`middleware.ts`](middleware.ts) guards all routes except `/login`, `/new`, and `/api/setSession`. Missing cookies → redirect to `/login`.
Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
## Routes
| Path | Description |
|------|-------------|
| `/login` | Admin login |
| `/new` | Create a new business account |
| `/` | Main dashboard (KPIs + charts) |
| `/Statistics` | Session duration & item engagement |
| `/Statistics/items` | Item popularity & coupon purchases |
| `/Statistics/users` | New users and logins over time |
| `/Statistics/loyalty` | QR code scan / loyalty participation |
| `/CMS` | Menu item list |
| `/CMS/addItem` | Add a menu item |
| `/CMS/updateItem` | Edit an item (item JSON in query string) |
| `/CMS/applyDiscount` | Discounts on items |
| `/CMS/notifications` | Push / in-app notifications |
| `/YourData` | Business profile + loyalty rules |
| `/Connect` | Square / Clover POS OAuth |
| `/LoyaltyRules` | Redirects to `/YourData` |
## Architecture
```
app/                    App Router pages, layouts, loading/error UI
components/
  AdminHeader/          Top nav (Main, Statistics, CMS, Your Data, Connect)
  SectionNav/           Horizontal tabs for CMS & Statistics
  ClientShell/          Client wrapper (header visibility, toasts)
  ui/                   Shared UI: Page, PageHeader, Card, Button, Badge,
                        Skeleton, EmptyState, ErrorState
  */                    Feature components (charts, CMS, forms, …)
lib/
  events.js             Cached event / item fetch helpers (React.cache)
  chartSetup.js         Shared Chart.js registration (Line + Bar only)
  dynamicCharts.js      next/dynamic chart imports
  business.js           Business / locations / POS signature APIs
  connect.js            POS connection check & revoke
  users.js, actions.js, notifications.js, …
middleware.ts           Cookie-based auth gate
scripts/                Utility scripts (e.g. create-test-account)
```
### Layout model
- **Server root layout** loads fonts + `globals.css` and wraps pages in `ClientShell`.
- **Top header** for primary navigation.
- **Section sub-nav** (tabs) only under CMS and Statistics — no nested sidebars.
### Data & performance
- Prefer **Server Components** + cookie auth for initial fetches.
- Charts are **dynamically imported** (`ssr: false`) with skeleton fallbacks.
- Server-prefetched series are passed as props so charts avoid an immediate remount fetch; they re-fetch when the user changes time range.
- Shared fetchers use `React.cache()` for in-request deduplication.
### Design system
Tokens live in `app/globals.css`:
- Brand red: `--brand-red` (`rgb(235, 55, 73)`)
- Surfaces: `--surface`, `--surface-muted`
- Cards, buttons, badges, skeletons via `components/ui/`
## Backend dependency
All data comes from the Appcult API (`NEXT_PUBLIC_URL`). Typical endpoints used by the dashboard:
| Method | Path | Used for |
|--------|------|----------|
| `POST` | `/loginAdmin` | Admin login |
| `POST` | `/business` | Create business |
| `GET`/`PUT` | `/business` | Business profile |
| `GET` | `/user/:businessId` | Users list |
| `GET` | `/item` | Menu items |
| `GET` | `/eventCount`, `/eventDuration`, `/eventMeta` | Analytics |
