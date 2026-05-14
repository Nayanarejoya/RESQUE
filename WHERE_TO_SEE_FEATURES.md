# Where to See Features in the App

## Patient
- **Home / Book ambulance:** HomeScreen – set location, select hospital, tap "Book Ambulance".
- **My bookings:** BookingScreen (Bookings tab) – list of your bookings and status.
- **Tracking:** TrackingScreen – open a booking to see map and status (when driver has accepted).

## Driver
- **Driver Dashboard:** DriverDashboardScreen – available bookings (accept), current booking, status buttons (Driver en route, Patient picked up, En route to hospital, Arrived at hospital).
- **Location:** Location is sent when the app has permission; ambulance location is updated via the API.

## Hospital
- **Hospital Dashboard:** HospitalDashboardScreen – list of bookings for your hospital(s), acknowledge / acknowledge all / clear acknowledged.
- **Filters:** All, To Acknowledge, Acknowledged.

## Traffic Police
- **Traffic dashboard:** TrafficDashboardScreen (shown when logged in as traffic_police) – traffic clearance requests with booking details, route, time, map; Approve and Route Cleared buttons.
- Requests appear when a driver accepts a booking or taps "En route to hospital".

## Auth
- **Login:** LoginScreen.
- **Register:** RegisterScreen – choose role (Patient, Driver, Hospital, Traffic Police); hospital users can set hospital location.

See **README.md** Section 8 for step-by-step use of each role.
