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

### 3 — Store your Firebase config as GitHub Actions Secrets

Because this is a public repository, the real Firebase config is kept out of source
control and injected at deploy time by GitHub Actions.

1. Go to your repo on GitHub → **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret** and add each of the following:

   | Secret name                  | Where to find the value                                   |
   |------------------------------|-----------------------------------------------------------|
   | `FIREBASE_API_KEY`           | Firebase Console → Project Settings → Your Apps → Web App |
   | `FIREBASE_AUTH_DOMAIN`       | same                                                      |
   | `FIREBASE_PROJECT_ID`        | same                                                      |
   | `FIREBASE_STORAGE_BUCKET`    | same                                                      |
   | `FIREBASE_MESSAGING_SENDER_ID` | same                                                    |
   | `FIREBASE_APP_ID`            | same                                                      |

The deploy workflow (`.github/workflows/deploy.yml`) reads these secrets and replaces the
`%%PLACEHOLDER%%` markers in `index.html` before uploading to GitHub Pages. The real
values are never stored in the repository.

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

Push your changes. Enable **GitHub Pages** in the repo Settings:

1. Go to **Settings** → **Pages**
2. Set **Source** to **GitHub Actions**

Every push to `main` will now trigger the deploy workflow, which injects your Firebase
config from secrets and publishes the site to
`https://<your-username>.github.io/Wedding-Site/`.

You can also trigger a deploy manually from the **Actions** tab →
**Deploy to GitHub Pages** → **Run workflow**.

---

## How it works

| Feature | Details |
|---|---|
| Login | Google Sign-In popup — any other Google account is rejected immediately |
| Data storage | Firestore document `users/{uid}` — invisible in this repo, synced in real time |
| Security rules | Server-side rules in `firestore.rules` block all other users at the database level |