# AppCult Employee

Cross-platform mobile app for business staff to operate a customer loyalty program in-store. Built with **React Native** and **Expo**.

Employees scan customer QR codes to award loyalty points, redeem rewards, and apply discounts — replacing manual point tracking at the counter.

---

## Role in the product

AppCult Employee is the staff-facing companion to a customer loyalty platform. Customers present a QR code at checkout; employees use this app to:

- Look up the customer and award points based on order total
- Redeem loyalty rewards against available points
- View and apply customer discounts

Staff authenticate with email/password, are tied to a business location, and can update their profile from the app.

---

## Features

- **QR code scanning** — Camera-based barcode scanning routes to order, reward, or discount flows
- **Loyalty points** — Record purchases and credit points to customer accounts via REST API
- **Reward redemption** — Fetch reward details and confirm redemptions in-app
- **Discount handling** — Display discount percentage and related item info from a scanned code
- **Auth & session** — Login/signup with JWT auth, automatic token refresh, and persisted session (Zustand + AsyncStorage)
- **Staff profile** — Update name, email, and assigned business location

---

## Tech stack

| Area | Technology |
|------|------------|
| Framework | React Native, Expo (SDK 53) |
| Navigation | Expo Router (file-based routing) |
| State | Zustand (persisted with AsyncStorage) |
| Networking | Axios (auth interceptors, token refresh) |
| Device APIs | Expo Camera (QR/barcode scanning) |
| Build & deploy | EAS Build |

---

## CV / portfolio blurb

> Built a React Native (Expo) employee mobile app for a loyalty platform. The app lets store staff scan customer QR codes to award points, redeem rewards, and apply discounts, with JWT authentication, persisted sessions, and REST API integration.

---

## Getting started

```bash
npm install
npx expo start
```

Requires `EXPO_PUBLIC_API_URL` for the backend API.

