# Wedding-Site 💍

Simple wedding-planning site deployed to GitHub Pages.

## Deployment

This repository deploys directly to GitHub Pages via:

- `.github/workflows/deploy.yml`

### Setup GitHub Pages

1. Go to **Settings** → **Pages**
2. Set **Source** to **GitHub Actions**

After that, every push to `main` deploys the site automatically.

## Data storage

Guest data is stored in the browser using `localStorage`.
This is **single-device only** storage (no account login and no cross-device sync).
