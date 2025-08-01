
* **Option A:** Simple username/password (local validation)
* **Option B:** Firebase Authentication (Google Sign-In)

This way, anyone (or Copilot) can choose which login flow to use.

---

````markdown
# Gen-Lib Admin Dashboard

This project is a **Library Admin Dashboard** built with React, Tailwind CSS, and Framer Motion.  
It shows a login page first, and only after successful login it displays the full admin dashboard.

You can set it up with:
- **Option A: Simple local username/password**
- **Option B: Firebase Authentication (Google Sign-In)**

---

## Features

- **Login flow**
  - Choose between simple local login or Firebase Auth
  - Unauthorized users cannot see the dashboard
- **Dashboard**
  - Total Books
  - Pending Requests
  - Processed Requests
  - Penalties
  - Active Users
  - Recent Activity
- **Modern design**
  - Tailwind CSS for responsive layout
  - Framer Motion for smooth animations
- **Ready to extend**
  - Add Books, Pending, Processed, Penalties, Users pages

---

## Tech Stack

- React
- Tailwind CSS
- Framer Motion
- React Router DOM
- (Optional) Firebase Authentication

---

## 1. Project Setup

### Create React App

```bash
npx create-react-app genlib_admin
cd genlib_admin
````

---

### Install dependencies

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npm install framer-motion react-router-dom
```

For Firebase (only if using Option B):

```bash
npm install firebase
```

---

### Configure Tailwind

Edit **tailwind.config.js**:

```javascript
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: { extend: {} },
  plugins: [],
};
```

Replace **src/index.css** with:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Remove `import './App.css'` from App.js.

---

## 2. Folder Structure

```
src/
  pages/
    Login.js
    Dashboard.js
  components/
    Header.js
  firebase.js (only for Option B)
  App.js
  index.js
```

---

## 3. Option A – Local Username/Password Login

Create **src/pages/Login.js**:

```javascript
import { useState } from "react";

export default function Login({ onLogin }) {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    // Local validation
    if (username === "admin" && password === "password123") {
      onLogin();
    } else {
      setError("Invalid username or password");
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-slate-100">
      <div className="bg-white shadow-lg rounded-xl p-8 w-80">
        <h1 className="text-2xl font-bold mb-6 text-center">Library Admin</h1>
        <form onSubmit={handleSubmit} className="space-y-4">
          <input
            type="text"
            placeholder="Username"
            className="w-full border p-2 rounded"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
          />
          <input
            type="password"
            placeholder="Password"
            className="w-full border p-2 rounded"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
          />
          <button
            type="submit"
            className="w-full bg-indigo-600 text-white py-2 rounded hover:bg-indigo-700"
          >
            Login
          </button>
        </form>
        {error && <p className="mt-4 text-red-500 text-center">{error}</p>}
      </div>
    </div>
  );
}
```

---

## 4. Option B – Firebase Authentication (Google Sign-In)

### Set up Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a project or use existing
3. Enable Authentication > Sign-in method > Google
4. Get the Firebase config from project settings

Create **src/firebase.js**:

```javascript
import { initializeApp } from "firebase/app";
import { getAuth, GoogleAuthProvider, signInWithPopup } from "firebase/auth";

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
export const signInWithGoogle = () => signInWithPopup(auth, provider);
```

Replace **src/pages/Login.js** with:

```javascript
import { useState } from "react";
import { signInWithGoogle, auth } from "../firebase";

export default function Login({ onLogin }) {
  const [error, setError] = useState("");

  const handleGoogleLogin = async () => {
    try {
      const result = await signInWithGoogle();
      const email = result.user.email;
      const allowed = ["admin@grietcollege.com"]; // whitelist
      if (allowed.includes(email)) {
        onLogin();
      } else {
        setError("You are not authorized.");
      }
    } catch (err) {
      setError(err.message);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-slate-100">
      <div className="bg-white shadow-lg rounded-xl p-8 w-80">
        <h1 className="text-2xl font-bold mb-6 text-center">Library Admin</h1>
        <button
          onClick={handleGoogleLogin}
          className="w-full bg-indigo-600 text-white py-2 rounded hover:bg-indigo-700"
        >
          Sign in with Google
        </button>
        {error && <p className="mt-4 text-red-500 text-center">{error}</p>}
      </div>
    </div>
  );
}
```

---

## 5. Dashboard

Create **src/pages/Dashboard.js**:

```javascript
import { motion } from "framer-motion";

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

export default function Dashboard() {
  return (
    <div className="bg-slate-50 min-h-screen flex flex-col">
      <header className="bg-white shadow px-6 py-4 flex justify-between items-center">
        <h1 className="text-xl font-bold text-indigo-700">Gen-Lib Admin</h1>
        <nav className="space-x-6 text-indigo-600 font-medium">
          <a href="#">Dashboard</a>
          <a href="#">Books</a>
          <a href="#">Pending</a>
          <a href="#">Processed</a>
          <a href="#">Penalties</a>
          <a href="#">Users</a>
        </nav>
      </header>

      <main className="flex-1 p-6">
        <h2 className="text-2xl font-bold mb-6">Admin Dashboard</h2>

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
              <div className="text-3xl font-bold text-indigo-700">{s.value}</div>
              <div className="text-gray-600 mt-2 text-center">{s.label}</div>
            </motion.div>
          ))}
        </div>

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

## 6. App.js

Replace **src/App.js**:

```javascript
import { useState } from "react";
import Login from "./pages/Login";
import Dashboard from "./pages/Dashboard";

function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return isLoggedIn ? (
    <Dashboard />
  ) : (
    <Login onLogin={() => setIsLoggedIn(true)} />
  );
}

export default App;
```

---

## Run Locally

```bash
npm start
```

---

## Deployment (Firebase Hosting)

```bash
npm run build
firebase deploy
```

---

## Summary

* Use **Option A** if you just need a quick demo.
* Use **Option B** if you want Firebase Authentication (recommended for production).
* After login, you get a modern full-page **Library Admin Dashboard**.

---
