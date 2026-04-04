# Nutrizel — Complete Hosting Guide

Your project has two parts:
- **Frontend** — HTML/CSS/JS files (index, login, register, dashboard, admin)
- **Backend** — Node.js/Express server in the `server/` folder

---

## Part 1 — Run Locally First

### Step 1: Install Node.js

1. Open your browser → go to **https://nodejs.org**
2. Click the big **"LTS"** download button
3. Run the `.msi` installer
4. On the installer screen, make sure **"Add to PATH"** is checked
5. Click Next → Next → Install → Finish
6. **Restart your computer**

### Step 2: Verify Installation

Open **Windows PowerShell** or **Command Prompt** and run:
```
node --version
npm --version
```
You should see version numbers like `v20.x.x` and `10.x.x`.

### Step 3: Install Server Dependencies

In PowerShell, navigate to your project:
```
cd C:\Users\hp\Kiro\server
npm install
```

### Step 4: Start the Backend Server

```
npm start
```
You'll see:
```
🌿 Nutrizel server running at http://localhost:3000
```

### Step 5: Open the Frontend

Open a **second** PowerShell window, go to the project root:
```
cd C:\Users\hp\Kiro
npx serve .
```
Open the URL shown (e.g. http://localhost:5000) in your browser.

---

## Part 2 — Host Online (Free Options)

---

### Option A — Frontend Only on GitHub Pages (Free, Easiest)

Best if you want to keep using `localStorage` (no real backend needed).

1. Create a free account at **https://github.com**
2. Create a new repository called `nutrizel`
3. Upload all your files (everything except the `server/` folder)
4. Go to repository **Settings → Pages**
5. Under "Source" select **main branch → / (root)**
6. Click Save

Your site will be live at:
```
https://yourusername.github.io/nutrizel
```

---

### Option B — Full Stack on Render (Free, Recommended)

Hosts both your frontend AND the Node.js backend with SQLite.

**Step 1 — Push to GitHub first**
1. Install Git: **https://git-scm.com/download/win**
2. In PowerShell from your project folder:
```
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/nutrizel.git
git push -u origin main
```

**Step 2 — Deploy backend on Render**
1. Go to **https://render.com** → Sign up free
2. Click **New → Web Service**
3. Connect your GitHub repo
4. Fill in:
   - Name: `nutrizel-server`
   - Root Directory: `server`
   - Build Command: `npm install`
   - Start Command: `npm start`
   - Instance Type: **Free**
5. Click **Create Web Service**

Your backend URL:
```
https://nutrizel-server.onrender.com
```

**Step 3 — Deploy frontend on Render (Static Site)**
1. Click **New → Static Site**
2. Connect same GitHub repo
3. Fill in:
   - Root Directory: ` ` (leave empty — project root)
   - Build Command: (leave empty)
   - Publish Directory: `.`
4. Click **Create Static Site**

Your frontend URL:
```
https://nutrizel.onrender.com
```

---

### Option C — Full Stack on Railway (Free tier, Very Easy)

1. Go to **https://railway.app** → Sign up with GitHub
2. Click **New Project → Deploy from GitHub repo**
3. Select your repo
4. Set the root directory to `server`
5. Add environment variable: `PORT = 3000`
6. Deploy — live URL in ~2 minutes

---

### Option D — VPS Hosting (DigitalOcean / Hostinger)

For production with a custom domain.

**DigitalOcean Droplet ($6/month):**
1. Create account at **https://digitalocean.com**
2. Create a Droplet → Ubuntu 22.04 → Basic → $6/month
3. SSH into your server:
```
ssh root@YOUR_SERVER_IP
```
4. Install Node.js:
```
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```
5. Clone your project and install:
```
git clone https://github.com/YOURUSERNAME/nutrizel.git
cd nutrizel/server
npm install
```
6. Keep it running with PM2:
```
npm install -g pm2
pm2 start index.js --name nutrizel
pm2 startup
pm2 save
```
7. Serve frontend with Nginx:
```
sudo apt install nginx
sudo cp -r /root/nutrizel /var/www/html/nutrizel
```

---

## Part 3 — Connect Frontend to Real Backend

Once your backend is deployed, add this to the top of `js/auth.js` and `js/app.js`:

```js
const API = "https://nutrizel-server.onrender.com/api";
```

Example API call replacing localStorage:
```js
async function login(email, password) {
  const res = await fetch(`${API}/auth/login`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email, password })
  });
  return res.json();
}
```

---

## Quick Recommendation

| Goal                          | Best Option              |
|-------------------------------|--------------------------|
| Just show the site online fast | GitHub Pages (Option A) |
| Full backend + database free   | Render (Option B)       |
| Easiest full stack deploy      | Railway (Option C)      |
| Custom domain + production     | DigitalOcean (Option D) |

Start with **GitHub Pages** to get online in 5 minutes, then move to **Render** when you're ready for the real backend.
