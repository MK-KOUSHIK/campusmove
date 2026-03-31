# 🚌 CampusMove — Live Bus Tracker

> Real-time campus bus tracking. Drivers broadcast live GPS. Students follow their bus, set pickup stops, and get arrival alerts — all in a single HTML file.

![Made with Firebase](https://img.shields.io/badge/Backend-Firebase-orange?style=flat-square&logo=firebase)
![Leaflet Maps](https://img.shields.io/badge/Maps-Leaflet.js-green?style=flat-square)
![Single File](https://img.shields.io/badge/Deploy-Single%20HTML%20File-blue?style=flat-square)

---

## ✨ Features

### 👨‍✈️ Driver
- Select your assigned bus from a visual grid before starting
- Broadcast live GPS to all students in real time via Firebase
- Trip timer, GPS accuracy display, and update counter
- One-tap Start / Stop trip

### 🎓 Student / Faculty
- Select your bus — only **that bus** appears on your map and sidebar
- Live bus marker with real-time position updates
- **Route polyline** drawn on the map showing the full bus path
- **📍 Pickup stop** — tap anywhere on the map to drop your personal stop pin
- **ETA calculation** — live distance and estimated arrival time to your stop
- **Arrival notifications** — in-app alerts when bus is <300m and <50m away

### 🔧 Admin Panel *(separate login)*
- Separate admin login at the bottom of the main login page
- Live dashboard: active buses, total registered buses, all driver statuses
- Real-time bus table with driver email, coordinates, and last update time
- Full bus registry table

### 🔐 Auth
- Email / password sign-in and sign-up via Firebase Authentication
- Role-based routing: Driver → bus selection → driver dashboard; Student → bus selection → live map
- Admin role stored in Firebase DB — no code changes needed to promote users

---

## 🚀 Deployment (Required — Firebase won't work from a local file)

### Option 1 — GitHub Pages *(recommended)*
```bash
# 1. Create a new repo on github.com
# 2. Rename campusmove.html → index.html
# 3. Push to main branch
git init
git add index.html
git commit -m "first commit"
git remote add origin https://github.com/YOUR_USERNAME/campusmove.git
git push -u origin main
# 4. Go to Settings → Pages → Source: main → Save
# Live at: https://YOUR_USERNAME.github.io/campusmove
```

### Option 2 — Netlify Drop *(fastest, 30 seconds)*
1. Rename `campusmove.html` → `index.html`
2. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
3. Drag and drop the file — you get a live `https://` URL instantly

### Option 3 — Firebase Hosting
```bash
npm install -g firebase-tools
firebase login
firebase init hosting   # set public dir to "." and index to index.html
firebase deploy
```

> **After deploying:** Go to Firebase Console → Authentication → Settings → **Authorized domains** → add your live URL.

---

## ⚙️ Setup

### 1. Firebase Project
1. Go to [firebase.google.com](https://firebase.google.com) → Create project
2. Enable **Authentication** → Sign-in method → **Email/Password**
3. Enable **Realtime Database** → Start in test mode

### 2. Add Your Firebase Config
Open `index.html` and replace the config block:
```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. Firebase Database Rules
Go to Firebase Console → Realtime Database → Rules and paste:
```json
{
  "rules": {
    "buses": {
      ".read": "auth != null",
      "$uid": {
        ".write": "auth != null && auth.uid === $uid"
      }
    },
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid"
      }
    }
  }
}
```

### 4. Set Up an Admin Account
1. Sign up normally through the app as any role
2. In Firebase Console → Realtime Database → find `users/{uid}`
3. Change the `role` field from `"driver"` or `"student"` to `"admin"`
4. That account can now log in via the **Admin Panel** link on the login page

### 5. Set Your Campus Bus Routes
In `index.html`, find `BUS_LIST` and replace the `routeCoords` arrays with real GPS waypoints for your campus:
```js
const BUS_LIST = [
  {
    id: "BUS-01",
    label: "Bus 01",
    routeCoords: [
      [12.9716, 77.5946],   // Main Gate
      [12.9730, 77.5960],   // Block A
      [12.9750, 77.5980],   // Library
      [12.9770, 77.5990],   // Hostel
    ]
  },
  // ... add more buses
];
```
> **Tip:** Use [Google Maps](https://maps.google.com) → right-click any point → copy coordinates. Add as many waypoints as you want for accurate route lines.

---

## 📖 How to Use

### Driver
1. Open the app → select **Driver** tab → sign in
2. Pick your assigned bus from the grid
3. Tap **▶️ Start Trip** — your GPS is now live
4. Tap **⏹ Stop Trip** when done

### Student / Faculty
1. Open the app → select **Student / Faculty** → sign in
2. Pick your bus from the grid — only your bus will appear on the map
3. Your bus's route is drawn on the map as a dashed line
4. Tap **📍 Set Stop** in the sidebar → tap your pickup point on the map
5. ETA updates live as the bus moves toward your stop
6. You'll get an in-app alert when the bus is **300m** and **50m** away

### Admin
1. On the login page, tap **🔧 Admin Panel →** at the bottom
2. Sign in with your admin account
3. View live bus statuses, driver emails, coordinates, and last update times

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| HTML / CSS / JS | Single-file frontend, no build tools |
| [Firebase Auth](https://firebase.google.com/products/auth) | Email/password authentication |
| [Firebase Realtime Database](https://firebase.google.com/products/realtime-database) | Live GPS sync across all clients |
| [Leaflet.js](https://leafletjs.com/) v1.9.4 | Interactive map |
| [OpenStreetMap](https://www.openstreetmap.org/) | Free map tiles |
| [Google Fonts](https://fonts.google.com/) | Syne + DM Sans typography |
| Haversine Formula | Distance calculation for ETA |

---

## 📁 Project Structure

```
campusmove/
├── index.html      ← Entire app (all screens in one file)
├── README.md       ← This file
└── LICENSE         ← MIT License
```

---

## 🗺️ App Flow

```
Login Page
├── Driver tab   → [Bus Selection Grid] → Driver Dashboard (GPS broadcast)
├── Student tab  → [Bus Selection Grid] → Live Map (track bus + ETA + alerts)
└── Admin Panel link → Admin Login → Admin Dashboard (live overview)
```

---

## 🔒 Security Notes

- Firebase web API keys are safe to expose **only if** your Database Rules are properly locked down (see setup above)
- Admin access is enforced server-side by checking `role === "admin"` in the Firebase DB — signing in with a non-admin account will be rejected even with correct credentials
- For production, enable [Firebase App Check](https://firebase.google.com/products/app-check) to prevent abuse

---

## 🤝 Contributing

Pull requests are welcome. For major changes, open an issue first.

---

## 📄 License

[MIT](LICENSE) © 2026 CampusMove
