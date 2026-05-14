# Project Structure

```
project/
├── server/                     # Backend (Node.js + Express)
│   ├── index.js                # Entry; Express app, Socket.io, MongoDB connect
│   ├── models/                 # Mongoose schemas
│   │   ├── User.js
│   │   ├── Booking.js
│   │   ├── Hospital.js
│   │   └── Ambulance.js
│   ├── routes/
│   │   ├── auth.js             # Register, login, me
│   │   ├── bookings.js         # Create, list, accept, status, cancel
│   │   ├── hospitals.js        # List, nearby, my/bookings, acknowledge
│   │   ├── traffic.js          # Clearance requests, approve, route-cleared
│   │   ├── ambulances.js       # My ambulance, location, available
│   │   └── routing.js          # Calculate route, nearest hospital
│   ├── services/
│   │   ├── routingService.js   # Distance, route, nearest hospital
│   │   ├── notificationService.js  # Notify hospital, traffic, patient; requestTrafficClearance
│   │   └── emailService.js     # Send emails (if configured)
│   ├── middleware/
│   │   └── auth.js             # JWT verify, authorize(roles)
│   └── scripts/
│       └── seed.js             # Create test users, hospitals, ambulances
├── mobile-app/                 # React Native (Expo)
│   ├── App.js                  # Nav container; role-based screens
│   ├── config.js               # API_BASE_URL, SOCKET_URL
│   ├── screens/                # Login, Register, Home, Booking, Tracking, Driver/Hospital/Traffic dashboards, Profile
│   └── services/
│       └── api.js              # Axios instance + auth header
├── .env                        # PORT, MONGODB_URI, JWT_SECRET, optional EMAIL_*
├── package.json                # Backend deps and scripts (start, dev, seed)
└── README.md                   # Full setup and usage guide
```

## Data Flow
- **Auth:** Register/Login → token stored in app → sent as Bearer for API calls.
- **Bookings:** Patient creates booking → Driver accepts → Driver updates status → Hospital and Traffic see updates (API + Socket.io).
- **Traffic:** Driver "En route to hospital" → server sets trafficClearance and emits → Traffic dashboard shows request.

See **README.md** for setup and **WHERE_TO_SEE_FEATURES.md** for where features appear in the app.
