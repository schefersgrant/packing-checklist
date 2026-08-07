# Packing Checklist — Setup Guide

One file (`index.html`) is the whole app. It needs two things to go live: a free database (Firebase) so check-offs sync between everyone's phones, and free hosting so it has a URL.

## 1. Create the database — done ✓

Firebase project is already created and `firebaseConfig` in `index.html` is filled in. Nothing to do here.

## 2. Push the code to GitHub

1. In a terminal, `cd` into the `packing-checklist` folder.
2. ```bash
   git init
   git add .
   git commit -m "Initial packing checklist"
   ```
3. On [github.com](https://github.com), click **New repository** (top right → your avatar → "New repository"). Name it e.g. `packing-checklist`, keep it private or public (either works), don't initialize with a README.
4. GitHub shows you push commands — run the ones under "…or push an existing repository from the command line", something like:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/packing-checklist.git
   git branch -M main
   git push -u origin main
   ```

## 3. Deploy on Vercel (~2 min)

1. Go to [vercel.com](https://vercel.com) → **Sign up / Log in with GitHub** (use the same GitHub account).
2. Click **Add New… → Project**.
3. Find `packing-checklist` in the repo list → **Import**.
4. Vercel auto-detects it as a static site — no build settings needed. Click **Deploy**.
5. In ~30 seconds you get a live URL like `packing-checklist.vercel.app`. Share that with your teams.

**Every future update is automatic**: edit `index.html` locally, then
```bash
git add .
git commit -m "update items"
git push
```
Vercel redeploys the new version within seconds — no dashboard steps needed.

## 4. Firestore test-mode expiration (important)

Test mode Firestore rules **expire after 30 days**, after which the app will stop loading data. Before that happens:

1. Firebase console → **Firestore Database → Rules**.
2. Replace the rules with:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```
3. Click **Publish**. This keeps it open permanently (fine for an internal tool like this with nothing sensitive in it).

## Day-to-day management

- **Add/remove items**: just do it in the app itself (the `+` bar at the bottom, `×` to delete). No file editing needed.
- **Reset before a competition**: each tab has a "Reset checks" button — unchecks everything but keeps the list, so you're not retyping items every event.
- **Rename teams / tabs**: open `index.html`, find the `LISTS` array near the top of the script, edit the `label` field (e.g. `"Team A"` → `"Team Overdrive"`). Commit and push to publish the change (see above).
- **Change default items**: same `LISTS` array, `defaultItems`. Note this only affects a list the *first* time it's ever opened (before anyone's added real data) — after that, edit items in-app.
- **Updating the live site**: `git add . && git commit -m "..." && git push` — Vercel redeploys automatically.

## Costs

Firebase's free tier (Spark plan) and Vercel's free (Hobby) tier are both far more generous than this app will ever need — this will not cost anything.
