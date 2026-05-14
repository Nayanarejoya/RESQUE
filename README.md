# Rescue – Smart Ambulance Booking & Traffic Clearance System

A full-stack system for emergency ambulance booking, real-time tracking, hospital notifications, and traffic clearance. It includes a **Node.js/Express backend**, **MongoDB** database, and a **React Native (Expo)** mobile app with separate interfaces for **patients**, **drivers**, **hospitals**, and **traffic police**.

---

## Table of Contents

1. [Prerequisites and installation](#1-prerequisites-and-installation)
2. [Project Structure](#2-project-structure)
3. [Backend Setup](#3-backend-setup)
4. [Database Setup (MongoDB)](#4-database-setup-mongodb)
5. [Mobile App Setup](#5-mobile-app-setup)
6. [Running the System](#6-running-the-system)
7. [Seed Data (Test Accounts)](#7-seed-data-test-accounts)
8. [How to Use the App](#8-how-to-use-the-app)
9. [End-to-End Flow](#9-end-to-end-flow)
10. [Configuration Reference](#10-configuration-reference)
11. [Troubleshooting](#11-troubleshooting)
12. [Optional: Email & Other Services](#12-optional-email--other-services)
13. [Sharing the project (e.g. Google Drive) – no Git needed](#13-sharing-the-project-eg-google-drive--no-git-needed)

**→ For a short list of steps and exact commands to run, see [SETUP_STEPS_AND_COMMANDS.md](SETUP_STEPS_AND_COMMANDS.md).**

---

## 1. Prerequisites and installation

You need the following to run the project. Install them **before** starting backend or mobile app setup.

| Requirement | Version / Notes |
|-------------|-----------------|
| **Node.js** | v14 or higher (includes **npm**) |
| **MongoDB** | Local installation **or** MongoDB Atlas (cloud) – see below |
| **Expo Go** | On your phone – for testing the mobile app (optional if using emulator) |
| **Git** | Not required. You can share or receive the project as a zip (e.g. via Google Drive). |

---

### 1.1 Install Node.js and npm

Node.js is required to run the backend and the mobile app tooling. **npm** (package manager) is included with Node.js.

**Windows:**

1. Go to [https://nodejs.org](https://nodejs.org).
2. Download the **LTS** (Long Term Support) version.
3. Run the installer. Accept the default options (including “Add to PATH”).
4. Restart your terminal or command prompt.
5. Verify:
   ```bash
   node --version
   npm --version
   ```
   You should see version numbers (e.g. `v20.x.x` and `10.x.x`).

**Mac:**

- **Option A (recommended):** Install [nvm](https://github.com/nvm-sh/nvm) (Node Version Manager), then run:
  ```bash
  nvm install --lts
  nvm use --lts
  ```
- **Option B:** Download the LTS installer from [https://nodejs.org](https://nodejs.org) and run it.
- Verify: `node --version` and `npm --version`.

**Linux (Ubuntu/Debian):**

```bash
# Using NodeSource (LTS)
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
node --version
npm --version
```

---

### 1.2 Install MongoDB (optional – or use MongoDB Atlas)

You need a MongoDB database. You can either run MongoDB **locally** or use **MongoDB Atlas** (cloud – no local install).

**Option A: MongoDB Atlas (no local install)**

1. Go to [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas) and create a free account.
2. Create a free cluster and a database user (username + password).
3. In **Network Access**, add your IP or “Allow Access from Anywhere” (`0.0.0.0/0`) for development.
4. Get your connection string (Connect → Connect your application) and use it in `.env` as `MONGODB_URI` (see Section 4).

No MongoDB software is installed on your computer; the backend connects to Atlas over the internet.

**Option B: Local MongoDB (Windows)**

1. Go to [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community).
2. Select Windows, download the MSI installer, and run it.
3. Choose “Complete” installation. Optionally install MongoDB as a service so it starts with Windows.
4. Ensure the MongoDB service is running (Windows: Services → MongoDB; or run `mongod` from a terminal).
5. In `.env` use: `MONGODB_URI=mongodb://localhost:27017/smart-ambulance`.

**Option B: Local MongoDB (Mac)**

```bash
# With Homebrew
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

Then use `MONGODB_URI=mongodb://localhost:27017/smart-ambulance` in `.env`.

---

### 1.3 Install Expo Go on your phone (for testing the app)

If you want to run the mobile app on your **physical phone** (instead of an emulator):

1. **Android:** Open Play Store, search for **Expo Go**, install it.
2. **iPhone:** Open App Store, search for **Expo Go**, install it.
3. Ensure your phone and computer are on the **same Wi‑Fi network**.
4. When you run `npm start` in the `mobile-app` folder, a QR code will appear; scan it with Expo Go (Android) or the Camera app (iPhone) to open the app.

You do **not** need Expo Go if you only use an Android/iOS emulator (e.g. from Android Studio or Xcode).

---

### 1.4 Summary

After installation you should have:

- **Node.js** and **npm** working (`node --version`, `npm --version`).
- **MongoDB** either: (a) running locally, or (b) an Atlas connection string ready for `.env`.
- **Expo Go** on your phone (optional), or an emulator ready.

Then proceed to **Section 3 (Backend setup)** and **Section 5 (Mobile app setup)**.

---

## 2. Project Structure

```
project/
├── server/                 # Backend API
│   ├── index.js            # Entry point, Express + Socket.io
│   ├── models/             # Mongoose models (User, Booking, Hospital, Ambulance)
│   ├── routes/              # API routes (auth, bookings, hospitals, traffic, etc.)
│   ├── services/            # Routing, notifications, email
│   └── scripts/
│       └── seed.js         # Creates test users and data
├── mobile-app/             # React Native (Expo) app
│   ├── App.js              # Navigation and role-based screens
│   ├── config.js           # API and Socket URLs (you must edit this)
│   ├── screens/             # Patient, Driver, Hospital, Traffic, Login, etc.
│   └── services/
│       └── api.js          # Axios client with auth token
├── .env                    # Backend config (create from .env.example if available)
├── package.json            # Backend dependencies and scripts
└── README.md               # This file
```

---

## 3. Backend Setup

### Step 1: Open the project folder

```bash
cd path/to/project
```

(Use the folder where the project files are, e.g. `C:\Users\YourName\Videos\project` or `~/project`.)

### Step 2: Install backend dependencies

```bash
npm install
```

### Step 3: Create the environment file

Create a file named **`.env`** in the **project root** (same folder as `package.json`), with at least:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/smart-ambulance
JWT_SECRET=your-secret-key-change-in-production
```

- **Local MongoDB:** Use `mongodb://localhost:27017/smart-ambulance` (see [Section 4](#4-database-setup-mongodb)).
- **MongoDB Atlas:** Use your Atlas connection string (see [Section 4](#4-database-setup-mongodb)).

Optional (for emails and production):

```env
# Email (optional – see EMAIL_SETUP.md)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_USER=your-sender@gmail.com
EMAIL_PASS=your-app-password
NOTIFICATION_RECIPIENT_EMAIL=your-notifications@example.com
```

### Step 4: Start the backend

```bash
npm run dev
```

Or, without auto-restart:

```bash
npm start
```

You should see:

- `Server running on port 5000`
- `✅ MongoDB connected successfully` (if MongoDB is running and reachable)

If MongoDB fails to connect, see [Section 4](#4-database-setup-mongodb) and [Section 11](#11-troubleshooting).

---

## 4. Database Setup (MongoDB)

### Option A: Local MongoDB

1. Install [MongoDB Community Server](https://www.mongodb.com/try/download/community) and start the MongoDB service.
2. In `.env` use:
   ```env
   MONGODB_URI=mongodb://localhost:27017/smart-ambulance
   ```
3. Restart the backend; you should see `✅ MongoDB connected successfully`.

### Option B: MongoDB Atlas (cloud)

1. Create an account at [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas).
2. Create a **cluster** (free tier is enough).
3. **Database Access:** Add a database user and note username/password.
4. **Network Access:** Add IP address:
   - For development: **“Allow Access from Anywhere”** (`0.0.0.0/0`).
   - Wait 1–2 minutes for it to apply.
5. **Connect:** In Atlas, click “Connect” → “Connect your application” → copy the connection string.
6. In `.env` set:
   ```env
   MONGODB_URI=mongodb+srv://USERNAME:PASSWORD@cluster0.xxxxx.mongodb.net/smart-ambulance?retryWrites=true&w=majority
   ```
   Replace `USERNAME`, `PASSWORD`, and the cluster hostname with your values.
7. Restart the backend.

For more detail, see **MONGODB_ATLAS_SETUP.md** and **MONGODB_TROUBLESHOOTING.md** in the project.

---

## 5. Mobile App Setup

### Step 1: Go to the mobile app folder

```bash
cd mobile-app
```

### Step 2: Install app dependencies

```bash
npm install
```

### Step 3: Set API and Socket URLs

Open **`mobile-app/config.js`** and set your **computer’s IP address** and port so the phone can reach the backend:

```javascript
// Use your computer's IP (not localhost) so the phone can connect
export const API_BASE_URL = 'http://YOUR_IP:5000/api';
export const SOCKET_URL = 'http://YOUR_IP:5000';
```

**How to find your IP:**

- **Windows:** `ipconfig` → look for “IPv4 Address” (e.g. `192.168.1.105`).
- **Mac/Linux:** `ifconfig` or `ip addr` → look for your Wi‑Fi IP (e.g. `192.168.1.105`).

Use the IP of the machine where the backend is running. Example:

```javascript
export const API_BASE_URL = 'http://192.168.1.105:5000/api';
export const SOCKET_URL = 'http://192.168.1.105:5000';
```

### Step 4: Start the Expo app

From the **`mobile-app`** folder:

```bash
npm start
```

- A QR code and Metro bundler will open in the terminal/browser.
- **On phone:** Install **Expo Go**, then scan the QR code (same Wi‑Fi as the computer).
- **On emulator:** Press `a` for Android or `i` for iOS in the terminal.

---

## 6. Running the System

1. **Start MongoDB** (if local) and ensure it is running.
2. **Start the backend** (from project root):
   ```bash
   npm run dev
   ```
   Wait for “MongoDB connected” and “Server running on port 5000”.
3. **Start the mobile app** (from `mobile-app`):
   ```bash
   npm start
   ```
   Open the app on your device or emulator via Expo Go or the Expo CLI.
4. **Optional:** Load test data (see [Section 7](#7-seed-data-test-accounts)).

The app will show the **Login** screen. Use seed accounts or register new users by role.

---

## 7. Seed Data (Test Accounts)

To create sample users and hospitals (and optionally ambulances), run the seed script **from the project root**:

```bash
npm run seed
```

This will:

- Clear existing users, ambulances, and hospitals (bookings may need to be cleared separately if your schema requires it).
- Create one user per role and link hospitals to the hospital user.

**Default test accounts (all use password `password123`):**

| Role            | Email             | Purpose                    |
|-----------------|-------------------|----------------------------|
| Patient         | patient@test.com  | Book ambulances, track    |
| Driver          | driver@test.com  | Accept trips, update status |
| Hospital        | hospital@test.com | See and acknowledge bookings |
| Traffic Police  | traffic@test.com  | See and approve clearance  |

After seeding, log in with any of these emails and `password123` to test each role.

---

## 8. How to Use the App

### 8.1 Patient

1. **Register or log in** as Patient (or use `patient@test.com` / `password123`).
2. **Allow location** when the app asks (needed for nearby hospitals and booking).
3. **Home screen:** Set or confirm pickup location; see “Hospital (nearest by your location)” and select a hospital.
4. **Book ambulance:** Tap “Book Ambulance”, add optional medical info/priority, then confirm. You’ll see “Requested” or similar status.
5. **My Bookings:** Open the “Bookings” tab to see your active booking.
6. **Tracking:** When a driver has accepted, you can open the booking to see status and (if implemented) map/tracking until the ambulance reaches the hospital.

### 8.2 Driver

1. **Register or log in** as Driver (or use `driver@test.com` / `password123`).
2. **Allow location** so the app can send your position (required for “my ambulance” and availability).
3. **Dashboard:** You’ll see “Available bookings” (pending requests). Tap **Accept** on a booking to assign it to you.
4. **After accepting:** The booking moves to “Current booking”. Update status in order:
   - **Driver en route** → you’re going to the patient.
   - **Patient picked up** → patient is in the ambulance.
   - **En route to hospital** → heading to the hospital (this triggers traffic clearance and hospital notification).
   - **Arrived at hospital** → trip complete.
5. Keep the app open so location updates are sent if your app uses them for tracking.

### 8.3 Hospital

1. **Register or log in** as Hospital (or use `hospital@test.com` / `password123`).
2. **Dashboard:** Lists bookings for your hospital (and any other hospitals linked to your user). New bookings appear when a patient books to your hospital; “En route to hospital” updates appear when the driver selects that status.
3. **Acknowledge:** For each booking you can tap **Acknowledge** to mark that the hospital has been notified. You can also use **Acknowledge all** and **Clear acknowledged** to manage the list.
4. Bookings show patient, ambulance, status, and (if sent) route/time info.

### 8.4 Traffic Police

1. **Register or log in** as Traffic Police (or use `traffic@test.com` / `password123`).
2. **Traffic dashboard:** Shows **Traffic Clearance Requests** when:
   - A driver has accepted a booking (clearance requested), or
   - A driver has tapped **En route to hospital**.
3. Each card shows **booking details** (patient, ambulance, priority, status), **route details** (distance, ETA, traffic conditions), **time** (requested, assigned, en route to hospital, elapsed), and a **route map**.
4. **Approve** → mark clearance approved.
5. **Route cleared** → mark the route as cleared for the ambulance.
6. If you see “Could not load” or no requests, ensure you’re logged in as **Traffic Police** and that the backend and socket URL in `config.js` are correct.

---

## 9. End-to-End Flow

Typical sequence:

1. **Patient** logs in → allows location → selects hospital → **Books ambulance**.
2. **Driver** logs in → sees the new booking in “Available bookings” → **Accepts** the booking.
3. **Hospital** dashboard shows the new booking (and can acknowledge when ready).
4. **Traffic** dashboard can show the request (once driver has accepted or gone “En route to hospital”).
5. **Driver** updates status: **Driver en route** → **Patient picked up** → **En route to hospital**.
6. **Traffic** sees the request with full details → **Approve** → (optionally) **Route cleared**.
7. **Hospital** sees “En route to hospital” and can **Acknowledge**.
8. **Driver** taps **Arrived at hospital** → booking completes; it disappears from active traffic list and driver can take new bookings.
9. **Patient** sees the trip as completed in My Bookings.

---

## 10. Configuration Reference

### Backend (`.env` in project root)

| Variable | Required | Description |
|----------|----------|-------------|
| `PORT` | No (default 5000) | Server port. |
| `MONGODB_URI` | Yes | MongoDB connection string (local or Atlas). |
| `JWT_SECRET` | Yes | Secret for JWT tokens; use a long random string in production. |
| `EMAIL_*` | No | See EMAIL_SETUP.md for email notifications. |
| `NOTIFICATION_RECIPIENT_EMAIL` | No | If set, all notification emails go to this address. |

### Mobile app (`mobile-app/config.js`)

| Export | Description |
|--------|-------------|
| `API_BASE_URL` | Full URL to the backend API, e.g. `http://YOUR_IP:5000/api`. |
| `SOCKET_URL` | Full URL to the backend (no `/api`), e.g. `http://YOUR_IP:5000`. |

Use the same host and port as the running backend. For a physical device, use the computer’s LAN IP, not `localhost`.

### For new user: what to change when using your own accounts

If someone gave you this code and you use **your own** MongoDB, your own email, and your own machine, only these places use “your” details. Change them; nothing else in the code uses your personal config.

| Where | What to change | Your details go here |
|-------|----------------|----------------------|
| **`.env`** (project root; create the file if you don’t have it) | Database and optional email | **MONGODB_URI** = your MongoDB connection string (your Atlas URI or `mongodb://localhost:27017/smart-ambulance`). **JWT_SECRET** = any long random string you choose. Optional: **EMAIL_USER**, **EMAIL_PASS**, **NOTIFICATION_RECIPIENT_EMAIL** if you want email (see EMAIL_SETUP.md). |
| **`mobile-app/config.js`** | Backend URL so the app reaches your server | Replace **YOUR_IP** with **your computer’s IP address** (e.g. `192.168.1.105`). Find it: Windows → `ipconfig`; Mac/Linux → `ifconfig` or `ip addr`. Use the same port (5000) if your server runs on 5000. |

No other files store your database URL, email, or IP. The app does not read your details from anywhere else.

---

## 11. Troubleshooting

### Backend won’t start or “MongoDB connection” errors

- **Local MongoDB:** Ensure the MongoDB service is running (e.g. Windows Services, or `brew services start mongodb-community` on Mac).
- **Atlas:** Check Network Access (e.g. `0.0.0.0/0` for testing), correct username/password in `MONGODB_URI`, and that the database name is `smart-ambulance` (or adjust in the URI). See **MONGODB_ATLAS_SETUP.md** and **MONGODB_TROUBLESHOOTING.md**.

### App shows “Could not load” or “Network Error”

- **Same Wi‑Fi:** Phone and computer must be on the same network.
- **Correct IP:** In `mobile-app/config.js`, use the computer’s current IP (from `ipconfig` / `ifconfig`). Restart the Expo app after changing `config.js`.
- **Firewall:** Allow inbound connections on port **5000** for the Node process (Windows Firewall or equivalent).
- **Backend running:** Confirm in the terminal that the server is listening on port 5000.

### Traffic dashboard is empty

- Log in with a **Traffic Police** account (e.g. `traffic@test.com`).
- Ensure at least one driver has **accepted** a booking and (for full flow) tapped **En route to hospital**.
- Pull to refresh or reopen the Traffic screen; check backend logs for `🚦 Emitted traffic-clearance-request`.

### Hospital dashboard shows no bookings

- Log in with a **Hospital** account (e.g. `hospital@test.com`).
- If you added hospitals via API/admin, ensure the hospital record has `userId` set to the logged-in hospital user (or use the in-app flow that links hospitals to the user).
- Have a patient book an ambulance and select that hospital (or nearest), then refresh.

### “User already exists” or login fails after seed

- Seed uses fixed emails. If you previously registered with the same email, either use that password or run `npm run seed` again (this **clears** and recreates users/hospitals/ambulances; any other data may be lost).

---

## 12. Optional: Email & Other Services

- **Email notifications:** Configure `EMAIL_*` and optionally `NOTIFICATION_RECIPIENT_EMAIL` in `.env`. Full steps and templates are in **EMAIL_SETUP.md**.
- **Google Maps / routing:** The app may use placeholder or simple routing; for production you would add a maps/routing API key and use it in the backend and/or app according to the code.
- **SMS (e.g. Twilio):** Not required for basic operation; add and configure only if you extend the app to send SMS.

---

## API Endpoints (Summary)

| Area | Method | Endpoint | Description |
|------|--------|----------|-------------|
| Auth | POST | `/api/auth/register` | Register (body: name, email, phone, password, role) |
| Auth | POST | `/api/auth/login` | Login (body: email, password) |
| Auth | GET | `/api/auth/me` | Current user (Bearer token) |
| Bookings | POST | `/api/bookings` | Create booking (patient) |
| Bookings | GET | `/api/bookings/my-bookings` | My bookings |
| Bookings | PATCH | `/api/bookings/:id/status` | Update status (driver) |
| Bookings | POST | `/api/bookings/:id/accept` | Accept booking (driver) |
| Hospitals | GET | `/api/hospitals/nearby` | Nearby hospitals (query: latitude, longitude) |
| Hospitals | GET | `/api/hospitals/my/bookings` | Bookings for my hospital(s) |
| Traffic | GET | `/api/traffic/clearance-requests` | List clearance requests (traffic_police/admin) |
| Traffic | POST | `/api/traffic/clearance-requests/:id/approve` | Approve clearance |
| Traffic | POST | `/api/traffic/clearance-requests/:id/route-cleared` | Mark route cleared |

All endpoints except register/login require the `Authorization: Bearer <token>` header.

---

## 13. Sharing the project (e.g. Google Drive) – no Git needed

You **do not need Git** to share or use this project. You can share the whole codebase via **Google Drive**, **OneDrive**, **Dropbox**, or any file sharing (zip + link).

### If you are sharing the project (uploader)

1. **Create a zip of the project folder** – but **exclude** these so the zip stays small and you don’t share secrets or cache:
   - **`node_modules`** (both in project root and in `mobile-app/`) – the recipient will run `npm install` themselves.
   - **`.env`** – contains your MongoDB password and secrets; the recipient should create their own.
   - **`mobile-app/node_modules`** – same as above.
   - Optional: **`.expo`**, **`mobile-app/.expo`** – cache; can be excluded.

2. **What to include:** All source code: `server/`, `mobile-app/` (except `mobile-app/node_modules`), `package.json`, `mobile-app/package.json`, `README.md`, and any other `.md`, `.js`, `.json` files at the root.

3. **Upload the zip** to Google Drive (or similar) and share the link (view or download).

4. **Tell the recipient:** “Download the zip, unzip it, then follow the README from **Section 3 (Backend setup)** and **Section 5 (Mobile app setup)**. Create your own `.env` file; do not expect a `.env` in the zip.”

**Quick way to zip (Windows):**  
- Copy the whole project folder to a new folder (e.g. `rescue-project-share`).  
- Delete `node_modules` and `mobile-app/node_modules` and the `.env` file from the copy.  
- Right‑click the folder → **Send to** → **Compressed (zipped) folder**.  
- Upload that zip to Drive.

**Quick way to zip (Mac/Linux):**  
From the parent of the project folder (and after removing `node_modules` and `.env` from the project if desired):

```bash
zip -r rescue-project.zip project -x "project/node_modules/*" "project/mobile-app/node_modules/*" "project/.env"
```

### If you received the project (downloader)

1. **Download the zip** from the shared link and **unzip** it to a folder (e.g. `C:\Users\YourName\Projects\rescue` or `~/Projects/rescue`).

2. **You do not need Git.** Just use the unzipped folder as your project.

3. **Follow this README from the start:**
   - **Section 3** – Backend setup: `npm install`, create `.env` with your own `MONGODB_URI` and `JWT_SECRET`, then `npm run dev`.
   - **Section 4** – Use local MongoDB or MongoDB Atlas (your own URI in `.env`).
   - **Section 5** – Mobile app: `cd mobile-app`, `npm install`, edit `config.js` with your computer’s IP, then `npm start`.
   - **Section 7** – Optional: run `npm run seed` for test accounts.

4. **No `.env` in the zip?** That’s intentional. Create your own `.env` in the project root with at least `PORT=5000`, `MONGODB_URI=...`, and `JWT_SECRET=...`.

---

## License

MIT.

---

**Quick start checklist for a new user:**

1. Install Node.js and (optionally) MongoDB locally or use Atlas.  
2. Get the project (download the zip from Drive and unzip, or clone with Git) → `npm install` in project root.  
3. Create `.env` with `PORT`, `MONGODB_URI`, `JWT_SECRET`.  
4. Run `npm run dev` and confirm MongoDB connects.  
5. In `mobile-app`: `npm install`, set `config.js` to your machine’s IP and port 5000, then `npm start`.  
6. (Optional) Run `npm run seed` and use `patient@test.com` / `driver@test.com` / `hospital@test.com` / `traffic@test.com` with password `password123`.  
7. Open the app on device/emulator and follow [Section 8](#8-how-to-use-the-app) and [Section 9](#9-end-to-end-flow).

**Exact commands for each step:** see **[SETUP_STEPS_AND_COMMANDS.md](SETUP_STEPS_AND_COMMANDS.md)**.
