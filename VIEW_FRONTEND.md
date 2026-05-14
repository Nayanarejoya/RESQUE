# Frontend (Mobile App) Overview

## Stack
- **React Native** with **Expo** – one codebase for iOS and Android.
- **React Navigation** – Stack and Tab navigators; role-based screens after login.
- **Screens:** Login, Register, Home, Booking, Tracking, Driver Dashboard, Hospital Dashboard, Traffic Dashboard, Profile.

## Entry and Role Routing
- **App.js** – Reads stored user/token; if logged in, shows the dashboard for the user's role (patient tabs, driver, hospital, traffic_police). Otherwise shows Login.
- **Patient:** Tab navigator with Home, Bookings, Profile; Stack includes Tracking.
- **Driver / Hospital / Traffic:** Single main screen (dashboard) and logout.

## Key Screens
- **LoginScreen, RegisterScreen** – Auth; role selection on register.
- **HomeScreen** – Patient home: location, nearby hospitals, book ambulance.
- **BookingScreen** – Patient’s booking list.
- **TrackingScreen** – Map and status for a booking.
- **DriverDashboardScreen** – Available bookings, current booking, status buttons.
- **HospitalDashboardScreen** – Bookings for hospital, acknowledge actions.
- **TrafficDashboardScreen** – Clearance requests, booking/route/time details, approve, route cleared.

## Config
- **mobile-app/config.js** – `API_BASE_URL` and `SOCKET_URL`; set to your backend (e.g. `http://YOUR_IP:5000/api` and `http://YOUR_IP:5000`).

## Running
From `mobile-app`: `npm install`, then `npm start`. Use Expo Go on device or emulator. See **README.md** Section 5 and 6.
