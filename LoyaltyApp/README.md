# CoffeeApp — Café Loyalty Mobile App

A cross-platform mobile loyalty application for cafés, built as a client for the AppCult platform. The app lets customers browse the menu, collect loyalty points, redeem rewards, and manage their account — replacing traditional stamp cards with a digital experience.

## Overview

CoffeeApp connects café customers to a business backend so they can earn and spend loyalty points in-store. Users authenticate, browse products, favourite items, view promotions, redeem coupons with points, and present a QR loyalty card at checkout. The app is tailored per business via configuration and talks to a shared REST API.

## What I built

- **Auth flow** — Sign up, log in, password reset, session persistence, and automatic token refresh
- **Product catalogue** — Searchable menu with item details, favourites, and pull-to-refresh sync from the API
- **Loyalty system** — Points balance, QR loyalty card for in-store scanning, and reward redemption
- **Coupons & promotions** — Purchased rewards, discounted items, and promotional listings
- **User settings** — Profile editing (including photo), notifications, help, FAQ, and privacy policy
- **Push notifications** — Device token registration with the backend for café messaging
- **State & networking** — Persistent client state (Zustand + AsyncStorage) and authenticated HTTP with interceptors

## Tech stack

React Native · Expo · Expo Router · TypeScript / JavaScript · Zustand · Axios · EAS Build · Firebase (Android push) · Lottie / SVG UI

## Highlights

- File-based navigation with a custom tab bar and modal flows for coupons and discounts
- Business-scoped multi-tenant design (`businessId` + remote API)
- Production-ready build profiles with EAS for development, preview, and store releases
- Themed UI with gradients, animations, and branded café styling
