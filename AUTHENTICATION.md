# Authentication

## Overview
The app uses **JWT (JSON Web Token)** authentication. After login or register, the server returns a token; the mobile app stores it and sends it in the `Authorization: Bearer <token>` header for protected API calls.

## Roles
- **patient** – Book and track ambulances.
- **driver** – Accept bookings and update status; linked to an ambulance.
- **hospital** – View and acknowledge hospital bookings.
- **traffic_police** – View and approve traffic clearance requests.
- **admin** – Can manage system (e.g. ambulances).

## Endpoints

- **POST /api/auth/register** – Body: name, email, phone, password, role (and optionally location). Returns token and user.
- **POST /api/auth/login** – Body: email, password. Returns token and user.
- **GET /api/auth/me** – Returns current user (requires valid Bearer token).

## Backend

- **Middleware:** `server/middleware/auth.js` – verifies JWT, loads user into `req.user`. Some routes use `authorize('role1', 'role2')` to restrict by role.
- **Token:** Signed with `JWT_SECRET` from `.env`; set a strong secret in production.

## Mobile App

- Token is stored (e.g. AsyncStorage) and attached by `mobile-app/services/api.js` request interceptor.
- On logout, token is cleared and the user is shown the login screen.
- Role determines which dashboard is shown (patient tabs, driver, hospital, traffic).

See **README.md** for test accounts (e.g. patient@test.com, driver@test.com) after running `npm run seed`.
