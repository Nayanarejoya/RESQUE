# Setup Steps and Commands

This document lists the **exact steps and commands** to run on your system to get the Rescue app running. Run them in order. Use **Command Prompt**, **PowerShell**, or **Terminal** (Mac/Linux).

---

## Step 1: Open the project folder

Go to the folder where the project files are (e.g. where you unzipped the code).

**Windows (PowerShell or CMD):**
```bash
cd C:\Users\YourName\Videos\project
```
*(Replace `YourName` and the path with your actual folder.)*

**Mac/Linux:**
```bash
cd ~/Projects/project
```
*(Replace with your actual path.)*

---

## Step 2: Install backend dependencies

Run this **in the project root** (same folder where `package.json` and `server` folder are):

```bash
npm install
```

**Expected:** A list of packages installing; no errors. May take 1–2 minutes.

---

## Step 3: Create the `.env` file

Create a file named **`.env`** in the **project root** (same folder as `package.json`).

**Option A – Using a text editor**  
Create a new file, name it `.env`, and paste the following. Then change the values as needed.

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/smart-ambulance
JWT_SECRET=your-secret-key-change-in-production
```

- If you use **MongoDB Atlas**, replace `MONGODB_URI` with your Atlas connection string.
- You can change `JWT_SECRET` to any long random string.

**Option B – Using Command Prompt / PowerShell (Windows):**  
*(Run from project root.)*

```bash
echo PORT=5000 > .env
echo MONGODB_URI=mongodb://localhost:27017/smart-ambulance >> .env
echo JWT_SECRET=your-secret-key-change-in-production >> .env
```

Then open `.env` in Notepad or any editor to change values if needed.

**Option B – Using Terminal (Mac/Linux):**
```bash
cat > .env << 'EOF'
PORT=5000
MONGODB_URI=mongodb://localhost:27017/smart-ambulance
JWT_SECRET=your-secret-key-change-in-production
EOF
```

---

## Step 4: Start the backend server

Run this **in the project root**:

```bash
npm run dev
```

**Expected output:**
- `Server running on port 5000`
- `✅ MongoDB connected successfully`

If you see **MongoDB connection error**, ensure MongoDB is running (local) or your Atlas URI in `.env` is correct. Then run the command again.

**Leave this terminal open.** The server must keep running while you use the app.

---

## Step 5: Find your computer’s IP address (for the mobile app)

You need this to edit `mobile-app/config.js` so the app can reach the backend.

**Windows (PowerShell or CMD):**
```bash
ipconfig
```
Look for **IPv4 Address** under your Wi‑Fi or Ethernet adapter (e.g. `192.168.1.105`).

**Mac / Linux:**
```bash
ifconfig
```
Or:
```bash
ip addr
```
Look for your Wi‑Fi IP (e.g. `192.168.1.105`). Note this IP; you will use it in the next step.

---

## Step 6: Open the mobile app folder and install dependencies

Open a **new** terminal (keep the backend running in the first one). Then:

```bash
cd mobile-app
```

*(If your project folder is `C:\Users\YourName\Videos\project`, then `cd mobile-app` means `C:\Users\YourName\Videos\project\mobile-app`.)*

Install app dependencies:

```bash
npm install
```

**Expected:** Packages installing; may take a few minutes.

---

## Step 7: Set your IP in the mobile app config

Open the file **`mobile-app/config.js`** in an editor.

Replace **`YOUR_IP`** with the IP address you found in Step 5.

**Example:** If your IP is `192.168.1.105`, change:

```javascript
const BACKEND_HOST = 'http://YOUR_IP:5000';
```

to:

```javascript
const BACKEND_HOST = 'http://192.168.1.105:5000';
```

Save the file.

---

## Step 8: Start the mobile app

Still in the **`mobile-app`** folder, run:

```bash
npm start
```

**Expected:** A QR code and Metro bundler in the terminal or browser.

- **On phone:** Install **Expo Go** from the app store, then scan the QR code (phone and computer must be on the same Wi‑Fi).
- **On Android emulator:** Press **`a`** in the terminal.
- **On iOS simulator (Mac):** Press **`i`** in the terminal.

The app will load and show the Login screen.

---

## Step 9 (Optional): Load test accounts (seed data)

If you want ready-made test users (patient, driver, hospital, traffic police), run the seed script **from the project root** in a **new** terminal (backend can keep running).

**From project root:**
```bash
cd C:\Users\YourName\Videos\project
```
*(Or your actual project path.)*

Then:

```bash
npm run seed
```

**Expected:** Messages like “Created patient user”, “Created driver user”, etc. Test accounts (all use password **`password123`**):

| Role   | Email              |
|--------|--------------------|
| Patient | patient@test.com  |
| Driver  | driver@test.com   |
| Hospital | hospital@test.com |
| Traffic | traffic@test.com  |

You can now log in with any of these emails and `password123`.

---

## Quick reference: commands in order

| Step | Where to run        | Command |
|------|---------------------|--------|
| 1    | Any                 | `cd path/to/project` |
| 2    | Project root        | `npm install` |
| 3    | —                   | Create `.env` file (see above) |
| 4    | Project root        | `npm run dev` |
| 5    | Any                 | `ipconfig` (Windows) or `ifconfig` (Mac/Linux) |
| 6    | Project root then   | `cd mobile-app` then `npm install` |
| 7    | —                   | Edit `mobile-app/config.js` (replace YOUR_IP) |
| 8    | mobile-app folder   | `npm start` |
| 9    | Project root (optional) | `npm run seed` |

---

## Stopping and starting again later

- **Stop backend:** In the terminal where `npm run dev` is running, press **Ctrl+C**.
- **Stop mobile app:** In the terminal where `npm start` is running, press **Ctrl+C**.
- **Start again:** From project root run `npm run dev`; from `mobile-app` run `npm start` (and ensure `config.js` and `.env` are still set).

For more detail (Node/MongoDB installation, troubleshooting), see **README.md**.
