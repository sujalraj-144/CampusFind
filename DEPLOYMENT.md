# 🌐 CampusFind 2.0 — 24/7 Cloud Deployment Guide

This guide explains how to deploy **CampusFind** to the cloud so it is accessible **24/7 from anywhere** on students\' smartphones, laptops, and college Wi-Fi, along with full **PWA offline support**.

---

## 🚀 Option 1: Free 24/7 Cloud Hosting on Render (Recommended)

Render offers free web service hosting for Python/Flask apps with automatic SSL (HTTPS).

### Steps:
1. **Push your code to GitHub**:
   ```bash
   git init
   git add .
   git commit -m "CampusFind 2.0 Release"
   git remote add origin https://github.com/<your-username>/campusfind.git
   git push -u origin main
   ```

2. **Deploy on Render**:
   - Go to [render.com](https://render.com) and sign in with GitHub.
   - Click **New +** ➔ **Web Service**.
   - Connect your `campusfind` GitHub repository.
   - Fill in:
     - **Name**: `campusfind`
     - **Environment**: `Python 3`
     - **Build Command**: `pip install -r requirements.txt && python backend/seed_data.py`
     - **Start Command**: `gunicorn wsgi:app --workers=3 --timeout=120`
   - Click **Create Web Service**.

3. **Your Live 24/7 URL**:
   In 2 minutes, your website is live at:  
   👉 `https://campusfind.onrender.com`

---

## ⚡ Option 2: 24/7 Cloud Hosting on Railway

Railway gives you an instant cloud URL and persistent storage.

1. Install Railway CLI or connect via [railway.app](https://railway.app).
2. Connect your GitHub repository.
3. Railway automatically detects `requirements.txt` and `Procfile`.
4. Generate a public domain:
   Go to **Settings** ➔ **Generate Domain** ➔ e.g. `https://campusfind-production.up.railway.app`.

---

## 📱 Option 3: Install as a Native PWA on Mobile / Desktop

Because CampusFind 2.0 includes a web manifest and service worker:

### On Android / Chrome:
1. Open your live URL (e.g. `https://campusfind.onrender.com` or local network).
2. Tap the **"📱 Install App"** button in the header or Chrome menu (⋮) ➔ **"Install app"** / **"Add to Home screen"**.
3. CampusFind is installed on your phone home screen with its custom icon!

### On iPhone (iOS Safari):
1. Open the URL in Safari.
2. Tap the **Share** button (box with upward arrow) ➔ tap **"Add to Home Screen"**.

---

## 📴 How Offline Mode & Auto-Sync Works

1. **Browsing Offline**:
   When internet drops, the **Service Worker** intercepts all requests and serves the cached app shell and inventory.
2. **Filing Offline Reports**:
   When a student fills the Lost/Found form while disconnected:
   - The ambient banner highlights: `📡 OFFLINE MODE ACTIVE`.
   - The report is safely written to **IndexedDB Outbox** (`#LF-OFFLINE-...`).
   - The header displays `📡 1 Pending Sync`.
3. **Automatic Reconnection Sync**:
   As soon as internet connectivity returns:
   - `SyncManager` detects the `online` event.
   - The outbox automatically posts the pending report to the central Flask API.
   - A success toast confirms: `✅ Synced to central database as #LF-2026-00006!`.

---

## 🔒 Production Security Checklist

- [x] Passwords salted and hashed.
- [x] Identifying distinguishing marks concealed on found items.
- [x] Parameterized SQL queries preventing SQL injection.
- [x] Production WSGI server (`gunicorn` / `waitress`) instead of Flask dev server.
- [x] Automatic HTTPS enforced by cloud host.
