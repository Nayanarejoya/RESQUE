# Quick Start

1. **Prerequisites:** Node.js v14+, MongoDB (local or Atlas), Expo Go on phone (optional).
2. **Backend:** In project root run `npm install`, create `.env` with `PORT=5000`, `MONGODB_URI=...`, `JWT_SECRET=...`, then `npm run dev`. Wait for "MongoDB connected" and "Server running on port 5000".
3. **Mobile app:** `cd mobile-app`, `npm install`, set your computer's IP in `config.js` (API_BASE_URL and SOCKET_URL), then `npm start`. Open with Expo Go or emulator.
4. **Seed (optional):** From project root run `npm run seed`. Use `patient@test.com`, `driver@test.com`, `hospital@test.com`, or `traffic@test.com` with password `password123`.
5. **Use:** Log in with one of the seed accounts (or register), then follow README Section 8 for your role.

For full steps, troubleshooting, and sharing the project, see **README.md**.
