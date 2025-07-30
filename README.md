
# Gen-Lib Admin Dashboard (React + Tailwind + Firebase)

This project is a **modern admin dashboard** for Gen-Lib.
It is built entirely with React, Tailwind CSS, and Framer Motion for smooth animations.

Admins can log in (Firebase authentication with Gmail), view library stats, approve requests, and see recent activity – all in a clean, interactive web dashboard.

---

## 1. Features

- **Modern, clean design**
  - Light colors, responsive layout
  - Animated cards and transitions
- **Pages**
  - Dashboard (stats + recent activity)
  - Books
  - Pending Requests
  - Processed Requests
  - Penalties
  - Users
- **Firebase Integration**
  - Firebase Authentication (Email/Password + Gmail)
  - Firestore for real-time data
- **Animations**
  - Smooth fade-in, hover effects, and page transitions using Framer Motion

---

## 2. Tech Stack

- React (Create React App)
- Tailwind CSS (styling)
- Framer Motion (animations)
- Firebase (backend)

---

## 3. Project Setup

### 1. Create React App

```bash
npx create-react-app genlib_admin
cd genlib_admin
````

### 2. Install dependencies

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npm install framer-motion firebase react-router-dom
```

---

## 4. Tailwind Configuration

Edit **tailwind.config.js** to include:

```javascript
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: { extend: {} },
  plugins: [],
};
```

Replace all content of **src/index.css** with:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Remove `App.css` and any `import './App.css'` lines.

---

## 5. Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com/).
2. Create a project or use an existing one.
3. Enable:

   * **Authentication** (Email/Password and Google)
   * **Cloud Firestore**
4. Add a **Web app** and copy your config.

Create a file **src/firebase.js**:

```javascript
import { initializeApp } from "firebase/app";
import { getAuth, GoogleAuthProvider } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "YOUR_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const provider = new GoogleAuthProvider();
export const db = getFirestore(app);
```

---

## 6. Replace `src/App.js`

Replace everything in **src/App.js** with:

```javascript
import { motion } from "framer-motion";
import "./index.css";

const stats = [
  { label: "Total Books", value: 1240 },
  { label: "Pending Requests", value: 12 },
  { label: "Processed Requests", value: 320 },
  { label: "Penalties", value: 5 },
  { label: "Active Users", value: 210 },
];

const recentActivity = [
  'User john.doe@grietcollege.com borrowed "React for Beginners"',
  "Request #1234 approved",
  "Penalty marked as paid for jane.smith@grietcollege.com",
];

export default function App() {
  return (
    <div className="bg-slate-50 min-h-screen flex flex-col">
      {/* Header */}
      <header className="bg-white shadow px-6 py-4 flex justify-between items-center">
        <h1 className="text-xl font-bold text-indigo-700">Gen-Lib Admin</h1>
        <nav className="space-x-6 text-indigo-600 font-medium">
          <a href="#">Dashboard</a>
          <a href="#">Books</a>
          <a href="#">Pending Requests</a>
          <a href="#">Processed Requests</a>
          <a href="#">Penalties</a>
          <a href="#">Users</a>
        </nav>
      </header>

      <main className="flex-1 p-6">
        <h2 className="text-2xl font-bold mb-6">Admin Dashboard</h2>

        {/* Stats grid */}
        <div className="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-5 gap-6 mb-10">
          {stats.map((s, i) => (
            <motion.div
              key={i}
              className="bg-white rounded-2xl shadow p-6 flex flex-col items-center"
              whileHover={{ scale: 1.05 }}
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ delay: i * 0.1 }}
            >
              <div className="text-3xl font-bold text-indigo-700">
                {s.value}
              </div>
              <div className="text-gray-600 mt-2 text-center">{s.label}</div>
            </motion.div>
          ))}
        </div>

        {/* Recent Activity */}
        <section>
          <h3 className="text-xl font-semibold mb-4">Recent Activity</h3>
          <ul className="list-disc pl-6 space-y-2 text-gray-700">
            {recentActivity.map((item, i) => (
              <motion.li
                key={i}
                initial={{ opacity: 0, x: -10 }}
                animate={{ opacity: 1, x: 0 }}
                transition={{ delay: 0.5 + i * 0.1 }}
              >
                {item}
              </motion.li>
            ))}
          </ul>
        </section>
      </main>

      <footer className="bg-white text-center py-4 text-gray-500 text-sm">
        © 2025 Gen-Lib. All rights reserved.
      </footer>
    </div>
  );
}
```

---

## 7. Run the app

```bash
npm start
```

You should now see a **responsive, modern dashboard** with cards and animations.

---

## 8. Next Steps

* Add **React Router** for multi-page navigation.
* Connect components to live Firestore data.
* Use Firebase authentication (redirect non-admins away).

---

## 9. Deployment

```bash
npm run build
firebase init hosting
firebase deploy
```

---

## Folder Structure (after setup)

```
genlib_admin/
  public/
  src/
    firebase.js
    App.js
    index.css
    index.js
  tailwind.config.js
  package.json
```

---

This README walks you from zero to a styled interactive dashboard with Firebase connectivity.

```
