# Jojo's Log — Setup & Deploy Guide

## Why this version is different
This version uses **Firebase Firestore** as a real-time shared database.
Both phones see every change within 1–2 seconds of it being made.
It also works offline — changes sync automatically when back online.

---

## Step 1 — Create a free Firebase project (10 min)

1. Go to **https://console.firebase.google.com**
2. Click **Add project** → name it `jojos-log` → Continue
3. Disable Google Analytics (not needed) → **Create project**
4. In the left sidebar click **Firestore Database** → **Create database**
   - Choose **Start in test mode** (allows read/write for now)
   - Pick the region closest to you — `nam5 (us-central)` for Canada/US
   - Click **Enable**
5. Click the **Rules** tab in Firestore, replace the rules with this, then click **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /feeds/{feedId} {
         allow read, write: if true;
       }
     }
   }
   ```

---

## Step 2 — Get your Firebase config

1. Click the **⚙️ gear icon** (top left) → **Project settings**
2. Scroll down to **Your apps** → click the **</>** Web icon
3. Register your app — name it `jojos-log-web` → click **Register app**
4. You'll see a `firebaseConfig` object like this:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "jojos-log-xxxxx.firebaseapp.com",
     projectId: "jojos-log-xxxxx",
     storageBucket: "jojos-log-xxxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abc123"
   };
   ```
5. Copy those values

---

## Step 3 — Paste config into index.html

Open `index.html` in any text editor and find this block near the top of the `<script>` section:

```js
const FIREBASE_CONFIG = {
  apiKey:            "PASTE_API_KEY",
  authDomain:        "PASTE_PROJECT_ID.firebaseapp.com",
  projectId:         "PASTE_PROJECT_ID",
  storageBucket:     "PASTE_PROJECT_ID.appspot.com",
  messagingSenderId: "PASTE_SENDER_ID",
  appId:             "PASTE_APP_ID"
};
```

Replace each `PASTE_...` value with the values from your Firebase config. Save the file.

---

## Step 4 — Deploy to GitHub + Vercel

### GitHub
1. Go to **https://github.com/new**
2. Name: `jojos-log` | Set to **Private** | Click **Create repository**
3. Click **uploading an existing file** → drag in `index.html` + `vercel.json`
4. Click **Commit changes**

### Vercel
1. Go to **https://vercel.com** → **Add New Project**
2. Import your `jojos-log` GitHub repo
3. Leave all settings default → **Deploy**
4. Live in ~30 seconds ✓

---

## Step 5 — Add to home screen on both phones

- **iOS Safari**: tap Share → **Add to Home Screen**
- **Android Chrome**: tap Menu → **Add to Home Screen**

---

## How the sync works

| Action | What happens |
|---|---|
| You add a feeding | Written to Firestore instantly |
| Your wife opens the app | Firestore sends her the update in ~1 second |
| Either phone is offline | Changes save locally, sync automatically when back online |
| App is opened fresh | Loads from Firestore (not the device) |

## First load behaviour
On the very first open, the app will upload all 270 historical entries to Firestore.
This takes a few seconds. Both phones will then share the same live database forever.

## Security note
Your Firebase API key is visible in the HTML — this is normal and by design for Firebase.
The Firestore rules control who can read/write. The current rules (`allow read, write: if true`)
mean anyone with your URL can access the data. Since this is a private family app at a
private URL, this is fine. If you ever want to lock it down further, add Firebase Authentication.

## Backing up your data
Use the **↓ CSV** button any time to download a full backup of all entries.
