# MongoDB Troubleshooting

## Connection Errors

### "MongoServerSelectionError" or "connection refused"

**If using local MongoDB:**
- Ensure the MongoDB service is running (Windows: Services; Mac: `brew services start mongodb-community`).
- Check that the port in `.env` is correct (default `27017`).
- Try: `mongodb://localhost:27017/smart-ambulance`

**If using MongoDB Atlas:**
- **Network Access:** In Atlas → Network Access, add your IP or `0.0.0.0/0` (see MONGODB_ATLAS_SETUP.md). Wait 1–2 minutes.
- **Database user:** Verify username and password in the connection string. No extra spaces or special characters unencoded.
- **Connection string:** Must include database name, e.g. `...mongodb.net/smart-ambulance?retryWrites=true&w=majority`.

### "Authentication failed"

- Wrong username or password in `MONGODB_URI`.
- For Atlas: use the database user password (not your Atlas account password).
- Ensure the user has read/write permissions on the database.

### "IP not whitelisted" / "network access not allowed"

- Atlas → Network Access → Add IP Address → "Allow Access from Anywhere" or add your current IP.
- Wait a few minutes and restart the backend.

## SSL/TLS Issues

- Atlas uses SSL by default. If you see TLS errors, ensure your Node.js and `mongoose` are up to date.
- Connection string for Atlas usually includes `tls=true` or is implied by `mongodb+srv://`.

## After Fixing

1. Restart the backend (`npm run dev` or `npm start`).
2. Look for: `✅ MongoDB connected successfully`.
3. Test login and API from the app.

For more setup steps, see **MONGODB_ATLAS_SETUP.md** and **README.md** Section 4.
