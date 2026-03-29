# campusmove
# 🚌 CampusMove — Live Bus Tracker

A real-time campus bus tracking web app built with **Firebase** and **Leaflet.js**. Drivers broadcast their live GPS location; students and faculty can watch buses move on an interactive map in real time.

---

## ✨ Features

- 🔴 **Live GPS tracking** — Driver's location updates every few seconds via Firebase Realtime Database
- 🗺️ **Interactive map** — Powered by Leaflet.js with OpenStreetMap tiles (dark theme)
- 👤 **Role-based login** — Separate dashboards for Drivers and Students/Faculty
- 🔐 **Firebase Authentication** — Secure sign-in and sign-up with email & password
- 📱 **Mobile responsive** — Works on phones, tablets, and desktops
- ⚡ **Single HTML file** — No build tools, no npm — just one file

---

## 🚀 Getting Started

### Prerequisites

- A [Firebase](https://firebase.google.com) project with:
  - **Authentication** (Email/Password) enabled
  - **Realtime Database** enabled
- A web host (GitHub Pages, Netlify, Firebase Hosting, etc.)

> ⚠️ **Important:** Firebase Authentication does NOT work when the file is opened directly from your filesystem (`file://` or `content://`). You must host it on an `https://` domain.

---

### Setup

1. **Clone or download** this repository
   ```bash
   git clone https://github.com/yourusername/campusmove.git
   cd campusmove
   ```

2. **Configure Firebase** — Open `index.html` and replace the Firebase config block with your own project credentials:
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

3. **Set Firebase Database Rules** — In your Firebase Console → Realtime Database → Rules:
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

4. **Add your domain to Firebase Authorized Domains** — Firebase Console → Authentication → Settings → Authorized domains → Add your hosted URL.

---

### Hosting (Free Options)

#### GitHub Pages
1. Push the repo to GitHub
2. Rename `campusmove.html` → `index.html`
3. Go to Settings → Pages → Source: `main` branch
4. Your live URL: `https://yourusername.github.io/campusmove`

#### Netlify Drop
1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag and drop `index.html`
3. Get an instant `https://` URL

#### Firebase Hosting
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

---

## 📖 How to Use

### As a Driver
1. Open the app and select the **Driver** tab
2. Sign up or sign in with your email
3. Tap **▶️ Start Trip** — your GPS will be broadcast live
4. Tap **⏹ Stop Trip** when your route is done

### As a Student / Faculty
1. Select the **Student / Faculty** tab
2. Sign up or sign in
3. See all active buses on the live map
4. Click any bus in the sidebar to zoom to its location

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| HTML / CSS / JS | Single-file frontend |
| [Firebase Auth](https://firebase.google.com/products/auth) | User authentication |
| [Firebase Realtime Database](https://firebase.google.com/products/realtime-database) | Live GPS data sync |
| [Leaflet.js](https://leafletjs.com/) | Interactive map |
| [OpenStreetMap](https://www.openstreetmap.org/) | Map tiles |
| [Google Fonts](https://fonts.google.com/) | Syne + DM Sans typography |

---

## 📁 Project Structure

```
campusmove/
├── index.html      # Entire app (login, driver dashboard, student map)
├── README.md       # This file
└── LICENSE         # MIT License
```

---

## 🔒 Security Notes

- Never commit your Firebase API keys to a public repository — consider using environment variables or Firebase App Check for production
- Tighten Firebase Database rules before deploying to production
- Firebase API keys for web apps are safe to expose **only if** your Database rules are properly configured

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙏 Acknowledgements

- [Firebase](https://firebase.google.com/) for the real-time backend
- [Leaflet.js](https://leafletjs.com/) for the beautiful map
- [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors for map data
