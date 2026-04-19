# Jojo's Log — Deploy to Vercel

## Step 1 — GitHub
1. Go to https://github.com/new
2. Name it `jojos-log`, set to **Private**, click Create
3. Click **uploading an existing file**, drag in `index.html` and `vercel.json`
4. Click **Commit changes**

## Step 2 — Vercel
1. Go to https://vercel.com → **Add New Project**
2. Import your `jojos-log` repo
3. Leave all settings default → **Deploy**
4. Done — live in ~30 seconds at `jojos-log.vercel.app`

## Step 3 — Home screen shortcut
- **iOS Safari**: Share → Add to Home Screen
- **Android Chrome**: Menu → Add to Home Screen

## About your data
- **270 historical entries (March 23 – April 18)** are baked into the HTML file itself — they cannot be lost
- **New entries** are saved to localStorage in each browser
- **Two phones won't sync automatically** — use ↓ CSV regularly as your backup, or ask to add Supabase sync later

## Updating
Replace `index.html` in GitHub → Vercel redeploys automatically in seconds
