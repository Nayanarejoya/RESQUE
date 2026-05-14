# Ambulance Setup

## Overview
Drivers use the app with role **driver**. Each driver is linked to an **ambulance** (vehicle). The system can auto-create an ambulance when a driver first accesses their dashboard.

## For Drivers

1. **Register** with role **Driver** (or use seed account `driver@test.com` / `password123`).
2. **Log in** and open the Driver Dashboard.
3. The app calls `GET /api/ambulances/my-ambulance`. If no ambulance exists for this user, the backend can auto-create one with a generated vehicle number (e.g. `AMB-xxxxxx`).
4. **Allow location** so the app can send ambulance position for tracking.
5. The driver can then see **available bookings** and **accept** them; status updates (en route, picked up, en route to hospital, arrived) are sent via the app.

## Backend

- **Models:** `server/models/Ambulance.js` – vehicle number, driverId, currentLocation, status, equipment, etc.
- **Routes:** `server/routes/ambulances.js` – my-ambulance, location update, status, available ambulances.
- **Auto-create:** When a driver has no ambulance, the my-ambulance endpoint can create one and associate it with the driver's user ID.

## Status Values

- `available` – can accept new bookings
- `on_way_to_patient` – driver en route to pickup
- `picked_up` – patient in ambulance
- `on_way_to_hospital` – en route to hospital
- After completion, status is set back to `available`.

See **README.md** Section 8.2 for how to use the app as a driver.
