# Wedding-Site 💍

Private wedding-planning app for Roshan & Anuja.

- **Access**: Google Sign-In only — restricted to `roshan.jamkatel@gmail.com`
- **Data**: Cloud Firestore (online, synced across every device you sign in to)
- **No passwords** and **no guest data** are stored in this repository

---

## One-time Firebase Setup

You need a free Firebase project to run this app. Follow these steps once.

### 1 — Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → give it a name (e.g. `wedding-site`) → click through the wizard

### 2 — Register a Web App and copy the config

1. In your project, click the **`</>`** (Web) icon to add a web app
2. Give the app a nickname → click **Register app**
3. Copy the `firebaseConfig` object shown — you'll need these values in the next step

### 3 — Put your config in `index.html`

Open `index.html` and find this block near the bottom:

```js
const FIREBASE_CONFIG = {
  apiKey:            "REPLACE_WITH_YOUR_API_KEY",
  authDomain:        "REPLACE_WITH_YOUR_AUTH_DOMAIN",
  projectId:         "REPLACE_WITH_YOUR_PROJECT_ID",
  storageBucket:     "REPLACE_WITH_YOUR_STORAGE_BUCKET",
  messagingSenderId: "REPLACE_WITH_YOUR_MESSAGING_SENDER_ID",
  appId:             "REPLACE_WITH_YOUR_APP_ID"
};
```

Replace every `"REPLACE_…"` string with the matching value from your Firebase config.

> **Note:** Firebase config values are *not* secrets — they are intentionally embedded in
> client-side code. Security is enforced by Firestore Security Rules and Firebase Auth,
> not by hiding the config.

### 4 — Enable Google Sign-In

1. In Firebase Console → **Authentication** → **Sign-in method**
2. Enable **Google** provider → save

### 5 — Add your domain to Authorised Domains

1. Still in **Authentication** → **Settings** → **Authorised domains**
2. Add your GitHub Pages domain, e.g. `roshanjamkatel.github.io`

### 6 — Create the Firestore database

1. In Firebase Console → **Firestore Database** → **Create database**
2. Choose **Production mode** (the rules file handles access control)
3. Pick any region (e.g. `us-central`)

### 7 — Deploy Firestore Security Rules

Install the Firebase CLI (one time):

```bash
npm install -g firebase-tools
firebase login
```

Then from this repo folder:

```bash
firebase use --add          # select your project
firebase deploy --only firestore:rules
```

This deploys `firestore.rules` so that **only `roshan.jamkatel@gmail.com`** can read or
write any data, even if someone finds your Firebase config.

### 8 — Deploy to GitHub Pages

Push your changes. Enable **GitHub Pages** in the repo Settings (source: `main` branch,
root `/`). Your site will be live at `https://<your-username>.github.io/Wedding-Site/`.

---

## How it works

| Feature | Details |
|---|---|
| Login | Google Sign-In popup — any other Google account is rejected immediately |
| Data storage | Firestore document `users/{uid}` — invisible in this repo, synced in real time |
| Security rules | Server-side rules in `firestore.rules` block all other users at the database level |