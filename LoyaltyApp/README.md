# Welcome to your Expo app 👋
# CoffeeApp (AppCult)
This is an [Expo](https://expo.dev) project created with [`create-expo-app`](https://www.npmjs.com/package/create-expo-app).
A React Native loyalty app for cafés, built with Expo. Customers browse the menu, collect points, redeem rewards, and manage their profile — connected to the [AppCult](https://appcult.eu) backend.
## Get started
## Features
1. Install dependencies
- **Authentication** — Sign up, log in, forgot password, and automatic token refresh
- **Menu** — Browse café items with search and favourites
- **Loyalty** — Earn points and show a QR loyalty card in-store
- **Rewards & coupons** — Redeem points for rewards; view purchased coupons
- **Promotions** — See discounted items and favourites
- **Profile & settings** — Edit profile, manage notifications, help, FAQ, and privacy policy
- **Push notifications** — Register device tokens with the backend
   ```bash
   npm install
   ```
## Tech stack
2. Start the app
| Layer | Choice |
| --- | --- |
| Framework | Expo 53 / React Native 0.79 |
| Routing | Expo Router (file-based) |
| State | Zustand + AsyncStorage (persisted) |
| HTTP | Axios (auth interceptors, token refresh) |
| UI | Custom theme, Linear Gradient, Lottie, SVG icons |
| Builds | EAS Build |
   ```bash
    npx expo start
   ```
## Project structure
In the output, you'll find options to open the app in a
```
app/                  # Expo Router screens & layouts
  (tabs)/             # Main tab navigator (home, coupons, promotions, settings)
src/
  screens/            # Screen implementations
  components/         # Reusable UI (cards, headers, animations)
  hooks/              # API helpers (auth, items, loyalty, rewards, …)
  store/              # Zustand stores (user, catalog/cart)
  theme/              # Colors, spacing, typography
axios.js              # Shared Axios client
```
- [development build](https://docs.expo.dev/develop/development-builds/introduction/)
- [Android emulator](https://docs.expo.dev/workflow/android-studio-emulator/)
- [iOS simulator](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), a limited sandbox for trying out app development with Expo
## Getting started
You can start developing by editing the files inside the **app** directory. This project uses [file-based routing](https://docs.expo.dev/router/introduction).
### Prerequisites
## Get a fresh project
- Node.js 18+
- npm
- Expo CLI / EAS CLI (for device builds)
- Android Studio and/or Xcode for emulators
When you're ready, run:
### Install
```bash
npm run reset-project
npm install
```
This command will move the starter code to the **app-example** directory and create a blank **app** directory where you can start developing.
### Environment
## Learn more
Create a `.env` (or configure via EAS) with:
To learn more about developing your project with Expo, look at the following resources:
```bash
EXPO_PUBLIC_API_URL=https://server.appcult.eu/api/v1/
EXPO_PUBLIC_BUSINESS_ID=<your-business-id>
```
- [Expo documentation](https://docs.expo.dev/): Learn fundamentals, or go into advanced topics with our [guides](https://docs.expo.dev/guides).
- [Learn Expo tutorial](https://docs.expo.dev/tutorial/introduction/): Follow a step-by-step tutorial where you'll create a project that runs on Android, iOS, and the web.
Production builds in `eas.json` already set these for the AppCult API.
## Join the community
### Run
Join our community of developers creating universal apps.
```bash
npx expo start
```
- [Expo on GitHub](https://github.com/expo/expo): View our open source platform and contribute.
- [Discord community](https://chat.expo.dev): Chat with Expo users and ask questions.
Then open in a development build, Android emulator, iOS simulator, or Expo Go (limited; prefer a [dev client](https://docs.expo.dev/develop/development-builds/introduction/) for notifications and native modules).
```bash
npm run android   # Android
npm run ios       # iOS
npm run web       # Web
```
### Build with EAS
```bash
eas build --profile development   # Internal dev client
eas build --profile preview       # Internal preview
eas build --profile production    # Store / production
```
## Main tabs
| Tab | Purpose |
| --- | --- |
| Home | Menu, search, item details |
| Coupons | Loyalty card, points, redeemed rewards |
| Promotions | Favourites and discounted items |
| Settings | Profile, help, FAQ, notifications, privacy |
## Scripts
| Command | Description |
| --- | --- |
| `npm start` | Start Expo |
| `npm run android` / `ios` / `web` | Platform-specific start |
| `npm test` | Jest (watch mode) |
| `npm run lint` | Expo lint |
## Notes
- Auth is gated at `app/index.js`: a valid token routes to tabs; otherwise to the auth screen.
- Catalog, rewards, and discounts load via `src/store/store.js` from the AppCult API.
- Android Firebase / push setup uses `google-services.json` (see `app.json`).
