# Email Notification Setup Guide

## Overview
The Rescue app sends email notifications to:
- **Drivers** – when assigned a new booking (to the driver's registered email)
- **Hospitals** – when an ambulance is en route (to hospital contact email and hospital user's email)
- **Traffic Police** – when traffic clearance is needed (to each traffic police user's email)
- **Patients** – when their booking is confirmed (to the patient's registered email)

## Where to change the email that receives notifications

**Option 1 – One inbox for all notifications (easiest)**  
In the project root, edit the **`.env`** file and set:

```env
NOTIFICATION_RECIPIENT_EMAIL=your-email@example.com
```

All notification emails will be sent to this address. Leave it blank (or remove the line) to use the default behaviour below.

**Option 2 – Default (role-based)**  
If `NOTIFICATION_RECIPIENT_EMAIL` is not set, emails go to:
- **Driver** → `server/models/User.js` (driver's `email` from the user who is the driver)
- **Hospital** → `server/models/Hospital.js` (`contact.email`) and the hospital user's `email`
- **Traffic** → each User with role `traffic_police` (their `email`)
- **Patient** → the patient User's `email`

To change those, update the user's or hospital's email in the app (profile/registration) or in the database.

**Sender (From) address**  
The address that **sends** the emails is set in `.env`:

```env
EMAIL_USER=your-sender@gmail.com
EMAIL_PASS=your-app-password
```

## Email Configuration

### Step 1: Add Email Settings to .env

Add these lines to your `.env` file:

```env
# Email Configuration (who sends)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password

# Optional: send all notifications to this address
NOTIFICATION_RECIPIENT_EMAIL=your-notifications@example.com
```

### Step 2: Choose Your Email Provider

#### Option A: Gmail (Recommended for Testing)

1. **Enable 2-Factor Authentication** on your Gmail account
2. **Generate App Password:**
   - Go to: https://myaccount.google.com/apppasswords
   - Select "Mail" and "Other (Custom name)"
   - Enter "Smart Ambulance"
   - Copy the 16-character password
3. **Update .env:**
   ```
   EMAIL_HOST=smtp.gmail.com
   EMAIL_PORT=587
   EMAIL_SECURE=false
   EMAIL_USER=your-email@gmail.com
   EMAIL_PASS=your-16-character-app-password
   ```

#### Option B: Outlook/Hotmail

```env
EMAIL_HOST=smtp-mail.outlook.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_USER=your-email@outlook.com
EMAIL_PASS=your-password
```

#### Option C: Custom SMTP Server

```env
EMAIL_HOST=your-smtp-server.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_USER=your-email@domain.com
EMAIL_PASS=your-password
```

### Step 3: Test Email Configuration

1. Restart your backend server
2. Create a booking as a patient
3. Check the backend console for:
   - `✅ Email sent successfully` (if configured correctly)
   - `⚠️ Email not configured` (if not set up)
   - `❌ Error sending email` (if there's a configuration issue)

## Email Templates

The system includes professional email templates for:

1. **Driver Notification:**
   - Subject: "🚑 New Ambulance Booking Assigned"
   - Includes: Booking ID, priority, pickup location, hospital, distance, ETA

2. **Hospital Notification:**
   - Subject: "🏥 Incoming Patient Alert"
   - Includes: Patient details, medical condition, estimated arrival time

3. **Traffic Police Notification:**
   - Subject: "🚨 Traffic Clearance Required"
   - Includes: Route information, priority, distance, ETA, traffic conditions

4. **Patient Notification:**
   - Subject: "✅ Ambulance Booking Confirmed"
   - Includes: Booking confirmation, status, hospital, ETA

## Troubleshooting

### Email Not Sending?

1. **Check .env file:**
   - Make sure EMAIL_HOST, EMAIL_USER, EMAIL_PASS are set
   - No extra spaces or quotes

2. **Gmail App Password:**
   - Must use App Password, not regular password
   - 2FA must be enabled

3. **Check Backend Console:**
   - Look for error messages
   - Check if email service is initialized

4. **Test Connection:**
   - Try sending a test email manually
   - Check spam folder

### Common Errors

- **"Invalid login"** - Wrong password or need App Password for Gmail
- **"Connection timeout"** - Wrong EMAIL_HOST or port
- **"Authentication failed"** - Check EMAIL_USER and EMAIL_PASS

## Production Recommendations

For production, consider:
- **SendGrid** - Professional email service (free tier available)
- **AWS SES** - Scalable email service
- **Mailgun** - Developer-friendly email API
- **Postmark** - Transactional email service

Update `server/services/emailService.js` to use these services instead of SMTP.

## Current Status

✅ Email service created
✅ Email templates ready
✅ Integration with notification service complete
⚠️ **Requires .env configuration to work**

After adding email settings to .env and restarting the server, emails will be sent automatically when bookings are created!
