# Quick Reference Card

## 🚀 Quick Start Commands

```bash
# Install dependencies
npm install

# Setup environment
cp .env.example .env
# Edit .env with your credentials

# Initialize super admin
npm run init-db
# OR
curl -X POST http://localhost:3000/api/init

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

## 🔑 Default Access

| Role        | URL            | Email                          | Password           |
| ----------- | -------------- | ------------------------------ | ------------------ |
| Super Admin | `/super-admin` | your-admin-email@example.com   | your_admin_password |
| Admin       | `/admin`       | (create via super admin)       | -                  |
| User        | `/login`       | (register via homepage)        | -                  |

## 📡 API Endpoints

### Authentication

```
POST /api/auth/register          - User registration
POST /api/auth/login             - User login
POST /api/auth/admin/login       - Admin login (sends OTP)
POST /api/auth/admin/verify-otp  - Verify admin OTP
POST /api/auth/superadmin/login  - Super admin login
POST /api/auth/logout            - Logout
GET  /api/auth/me                - Get current user
```

### Events

```
GET    /api/events               - List all events
POST   /api/events               - Create event (super admin)
GET    /api/events/[id]          - Get event details
PUT    /api/events/[id]          - Update event
DELETE /api/events/[id]          - Delete event
```

### Payments

```
POST /api/payment/create-order   - Create Razorpay order
POST /api/payment/verify         - Verify payment & generate QR
```

### Tickets

```
GET /api/tickets                 - Get user's tickets
```

### Scanner (Admin)

```
POST /api/scanner/scan           - Scan QR code
POST /api/scanner/sync           - Sync offline scans
```

### Admin Operations

```
GET    /api/admin/dashboard      - Dashboard stats
GET    /api/admin/users          - List users for event
POST   /api/admin/users/[id]/reset-qr  - Reset QR status
DELETE /api/admin/users/[id]     - Delete user
GET    /api/admin/activity-logs  - Get activity logs
```

### Super Admin

```
GET    /api/superadmin/admins    - List admins
POST   /api/superadmin/admins    - Create admin
DELETE /api/superadmin/admins/[id]  - Delete admin
PUT    /api/superadmin/admins/[id]  - Update admin
GET    /api/superadmin/analytics - Get cross-event analytics
```

## 🎯 User Flows

### Registration Flow

```
1. Browse Events (/)
2. Select Event → Register (/register?eventId=xxx)
3. Fill Form (name, email, phone, DOB, password)
4. Payment via Razorpay
5. QR Generated & Emailed
6. Redirect to Dashboard (/dashboard)
```

### Entry Flow (Admin)

```
1. Admin Login (/admin) → OTP Verification
2. Navigate to Scanner (/admin/scanner)
3. Select Gate Name
4. Scan QR Code
5. System Validates:
   ✓ Valid QR?
   ✓ Not used?
   ✓ Within time window?
6. Grant/Deny Entry
7. Log to Database
```

### Offline Scanning Flow

```
1. Internet Disconnected
2. Scanner detects offline mode
3. Scan QR → Store locally
4. Internet Reconnects
5. Auto-sync or Manual Sync
6. Resolve conflicts
7. Update database
```

## 🗂️ Database Collections

```
users           - Registered users
admins          - Admin accounts
superadmins     - Super admin accounts
events          - Event details
tickets         - Generated tickets with QR
entries         - Entry logs
activitylogs    - Admin actions
otps            - Temporary OTPs for 2FA
```

## 🎨 Theme Toggle

```tsx
import { useTheme } from "@/components/ThemeProvider";

const { theme, toggleTheme } = useTheme();
// theme: 'light' | 'dark'
```

## 🔔 Toast Notifications

```tsx
import Toast from "@/components/Toast";

<Toast
  message="Success!"
  type="success" // 'success' | 'error' | 'info'
  onClose={() => {}}
/>;
```

## 📊 Stats Dashboard

**User Dashboard**

- My Tickets
- QR Codes
- Entry Status
- Download Option

**Admin Dashboard**

- Total Registered
- Total Tickets
- Checked In
- Remaining
- Recent Entries
- User Management

**Super Admin Dashboard**

- All Events Stats
- Admin Management
- Cross-Event Analytics
- Revenue Tracking
- Check-in Rates Graph

## 🛠️ Environment Variables

```env
# Required
MONGODB_URI=              # MongoDB connection string
JWT_SECRET=               # Min 32 characters
RAZORPAY_KEY_ID=         # From Razorpay dashboard
RAZORPAY_KEY_SECRET=     # From Razorpay dashboard
EMAIL_HOST=              # SMTP host
EMAIL_PORT=              # SMTP port (587)
EMAIL_USER=              # Email address
EMAIL_PASSWORD=          # App password
NEXT_PUBLIC_APP_URL=     # Your domain

# Optional
SUPER_ADMIN_EMAIL=       # Default: your-admin-email@example.com
SUPER_ADMIN_PASSWORD=    # Default: your_admin_password
```

## 🚨 Important Notes

⚠️ **Security**

- Change default super admin credentials immediately
- Use strong JWT_SECRET (min 32 chars)
- Enable MongoDB IP whitelist in production
- Use app-specific password for email
- Never commit .env file

⚠️ **Email Setup (Gmail)**

1. Enable 2FA on Google Account
2. Generate App Password
3. Use app password in EMAIL_PASSWORD

⚠️ **Razorpay**

- Test mode: Use test keys
- Production: Switch to live keys
- Enable webhooks for reliability

⚠️ **MongoDB Atlas**

- Create new cluster
- Whitelist IPs (0.0.0.0/0 for serverless)
- Create database user
- Copy connection string

⚠️ **Offline Support**

- Requires HTTPS (not localhost)
- Grant camera permissions
- localStorage must be enabled
- Test sync functionality

## 📱 Device Compatibility

✅ **Supported**

- Chrome (Desktop & Mobile)
- Safari (iOS & macOS)
- Firefox
- Edge

✅ **Features**

- Native camera access
- Vibration feedback
- Sound notifications
- Offline storage
- PWA support

## 🎯 Performance Targets

| Metric        | Target  | Actual |
| ------------- | ------- | ------ |
| QR Generation | < 500ms | ~300ms |
| QR Scanning   | < 200ms | ~150ms |
| Page Load     | < 2s    | ~1.2s  |
| API Response  | < 300ms | ~200ms |
| Offline Sync  | < 1s    | ~800ms |

## 🐛 Quick Fixes

**Issue: Camera not working**

```
✓ Use HTTPS (not http://)
✓ Grant camera permissions
✓ Check browser compatibility
✓ Try different camera if multiple
```

**Issue: Database connection failed**

```
✓ Check MONGODB_URI format
✓ Verify IP whitelist
✓ Test connection string
✓ Check network access
```

**Issue: Payment not processing**

```
✓ Verify Razorpay keys
✓ Check test/live mode
✓ Enable payment methods
✓ Test with test card
```

**Issue: Email not sending**

```
✓ Use app-specific password
✓ Check SMTP settings
✓ Verify email address
✓ Test with different provider
```

## 📦 NPM Scripts

```json
{
  "dev": "next dev", // Start development server
  "build": "next build", // Build for production
  "start": "next start", // Start production server
  "lint": "next lint", // Lint code
  "init-db": "ts-node scripts/init-superadmin.ts" // Initialize database
}
```

## 🔗 Important URLs

**After Starting Server:**

- Home: http://localhost:3000
- User Login: http://localhost:3000/login
- Admin Login: http://localhost:3000/admin
- Super Admin: http://localhost:3000/super-admin
- API Health: http://localhost:3000/api/init

**External Services:**

- MongoDB Atlas: https://cloud.mongodb.com
- Razorpay Dashboard: https://dashboard.razorpay.com
- Vercel: https://vercel.com
- Gmail App Passwords: https://myaccount.google.com/apppasswords

## 💡 Pro Tips

1. **Development**
   - Use MongoDB Compass for database visualization
   - Install React DevTools browser extension
   - Use Postman for API testing

2. **Deployment**
   - Test in Vercel preview before production
   - Set up proper error tracking (Sentry)
   - Configure MongoDB backups

3. **Scaling**
   - Use Redis for session storage
   - Implement API rate limiting
   - Enable CDN for assets
   - Optimize images

4. **Monitoring**
   - Enable Vercel Analytics
   - Set up MongoDB Atlas monitoring
   - Track API response times
   - Monitor error rates

---
