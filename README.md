# Gen-Lib_Admin

```markdown
# Gen-Lib Admin Dashboard (Web)

This project is a **React-based admin dashboard** for the Gen-Lib library system.  
It allows librarians/admins to manage the library from a browser with real-time Firebase integration.  
The website has:
- Clean light-colored design
- Smooth, interactive animations
- Firebase Authentication (email/password and Gmail login)
- Firestore database for real-time updates

---

## 1. Features

### Authentication
- Login with Gmail or email/password using Firebase Authentication.
- Only admin emails can access the dashboard (e.g., `admin@grietcollege.com`).

### Admin Dashboard Capabilities
- View all books and update stock
- Approve/reject book borrow requests
- Handle return requests
- Manage penalties (mark as paid)
- View processed requests
- Search users, see borrowed books, export user records

### UI and Animations
- Light theme with pastel colors
- Smooth fade/slide animations between pages
- Animated hover effects on buttons and cards
- Responsive layout (desktop and mobile)

---

## 2. Tech Stack

- **Frontend:** React (Create React App)
- **Routing:** React Router DOM
- **Styling:** Tailwind CSS for utility-first styling
- **Animations:** Framer Motion
- **Icons:** React Icons
- **Backend:** Firebase (Auth + Firestore + Hosting)

---

## 3. Design Details

### Color Palette
- **Primary background:** `#F8FAFC` (soft gray-white)
- **Primary text:** `#1E293B` (dark gray)
- **Accent colors:** 
  - Buttons / highlights: `#4F46E5` (indigo)
  - Success: `#10B981` (green)
  - Error: `#EF4444` (red)

### Fonts
- `Inter`, sans-serif (Google Fonts)

### Layout
- **Header:**  
  - Fixed top navigation bar with logo on the left, user profile/avatar on the right.
  - Smooth shadow when scrolling.

- **Sidebar:**  
  - On desktop: a collapsible vertical menu with icons and labels.
  - On mobile: hidden behind a hamburger menu.

- **Footer:**  
  - Simple footer with copyright © and Gen-Lib branding.
  - Background: white, small text, top border.

---

## 4. Folder Structure

```

genlib-admin/
├─ public/
│  ├─ index.html
│  └─ logo.png
├─ src/
│  ├─ components/
│  │  ├─ Header.js
│  │  ├─ Sidebar.js
│  │  ├─ Footer.js
│  │  └─ AnimatedCard.js
│  ├─ pages/
│  │  ├─ Login.js
│  │  ├─ Dashboard.js
│  │  ├─ Books.js
│  │  ├─ PendingRequests.js
│  │  ├─ ProcessedRequests.js
│  │  ├─ Penalties.js
│  │  ├─ Users.js
│  │  └─ UserBorrowedBooks.js
│  ├─ firebase.js
│  ├─ App.js
│  ├─ App.css
│  └─ index.js
└─ README.md

````

---

## 5. Setup Instructions

### 1. Clone and Install

```bash
git clone https://github.com/yourusername/genlib-admin.git
cd genlib-admin
npm install
````

### 2. Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project (or select existing).
3. Enable:

   * **Authentication** (Google provider + Email/Password)
   * **Cloud Firestore**
4. Go to **Project Settings > General > Your apps > Web app** and copy your config.

Create a file `src/firebase.js`:

```javascript
import { initializeApp } from "firebase/app";
import { getAuth, GoogleAuthProvider } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "YOUR-KEY",
  authDomain: "YOUR-DOMAIN",
  projectId: "YOUR-PROJECT-ID",
  storageBucket: "YOUR-BUCKET",
  messagingSenderId: "YOUR-SENDER-ID",
  appId: "YOUR-APP-ID"
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const provider = new GoogleAuthProvider();
export const db = getFirestore(app);
```

---

### 3. Tailwind Setup

Install Tailwind:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

Edit `tailwind.config.js`:

```javascript
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

Add Tailwind to `src/index.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

### 4. Start the App

```bash
npm start
```

---

## 6. Deployment

Build and deploy to Firebase Hosting:

```bash
npm run build
firebase init hosting
firebase deploy
```

---

## 7. Animations

Use **Framer Motion** for animations:

```bash
npm install framer-motion
```

Example usage:

```javascript
import { motion } from "framer-motion";

<motion.div
  initial={{ opacity: 0, y: 10 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.4 }}
>
  Content here
</motion.div>
```

* Apply animations to page transitions, cards, buttons.

---

## 8. Security Rules (Important)

In Firebase console, ensure Firestore security rules restrict admin data to only admin users.
Example:

```javascript
match /{document=**} {
  allow read, write: if request.auth != null && 
    request.auth.token.email == "admin@grietcollege.com";
}
```

---

## 9. Admin Email Whitelisting

In your login handler, redirect only if `user.email === "admin@grietcollege.com"`.

---

## 10. Future Enhancements

* Add charts and analytics
* Dark mode toggle
* Multi-admin support with role-based permissions

---

## License

MIT

```
