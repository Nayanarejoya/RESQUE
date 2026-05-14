# MongoDB Atlas Network Access Setup

## Current Situation
Your MongoDB Atlas IP Access List only allows specific IP addresses. If your IP changes, you won't be able to connect.

## Solution: Allow Access from Anywhere

### Step 1: Add IP Address
1. Click the green **"+ ADD IP ADDRESS"** button (top right)

### Step 2: Select "Allow Access from Anywhere"
1. In the popup that appears, you'll see options:
   - **"Allow Access from Anywhere"** - Click this option
   - This will add `0.0.0.0/0` to your IP list
   - This allows connections from ANY IP address

2. **OR** if you want to be more secure:
   - Select "Add Current IP Address" to add your current IP
   - But you'll need to update this if your IP changes

### Step 3: Confirm
1. Click "Confirm" button
2. Wait 1-2 minutes for changes to take effect

### Step 4: Verify
- You should see a new entry: `0.0.0.0/0` with status "Active"
- This means connections from anywhere are now allowed

## Security Note

**"Allow Access from Anywhere" (0.0.0.0/0)** is convenient but less secure. For production:
- Use specific IP addresses
- Or use MongoDB Atlas VPC peering
- Or use IP whitelist with your server's static IP

For development/testing, allowing from anywhere is fine.

## After Adding IP Access

1. **Restart your backend server:**
   ```bash
   cd C:\Users\1353\Videos\project
   npm start
   ```

2. **Check for success message:**
   - You should see: `✅ MongoDB connected successfully`

3. **Test the app:**
   - Reload mobile app
   - Try logging in

## Alternative: Add Your Current IP

If you prefer to only allow your current IP:

1. Click "+ ADD IP ADDRESS"
2. Select "Add Current IP Address"
3. Add a comment like "My Development Machine"
4. Click "Confirm"

**Note:** You'll need to update this if your IP changes (e.g., when you connect to a different WiFi network).
