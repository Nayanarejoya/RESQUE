# Standalone Project

This project is **standalone and independent**. It does not depend on other repositories or external systems beyond:

- **Node.js** and **npm** for the backend
- **MongoDB** (local or Atlas) for the database
- **Expo/React Native** for the mobile app

All application code (backend API, mobile app, models, routes, services) is self-contained in this repository. You can:

- Share it as a zip (e.g. via Google Drive) without using Git
- Run it on a single machine with local MongoDB, or use MongoDB Atlas for the database
- Deploy backend and app separately; configure `mobile-app/config.js` to point to your backend URL

There are no required external APIs or services except optional email (see EMAIL_SETUP.md) and optional maps/routing keys if you extend the app.

See **README.md** for full setup and usage.
