# Deployment Guide

Complete step-by-step guide to push to GitHub and deploy to Vercel.

## Step 1: Prepare Your Project

### 1.1 Install Dependencies (if not already done)
```bash
npm install
```

### 1.2 Verify `.env` is in `.gitignore`
The `.env` file should NOT be committed to GitHub. Check that `.gitignore` contains:
```
.env
.env.local
```

### 1.3 Create `.env.example`
Already created, but verify it's in the repo (without actual API key):
```bash
cat .env.example
```

### 1.4 Create `.env` locally (not committed)
```bash
cp .env.example .env
# Edit .env and add your GEMINI_API_KEY
```

---

## Step 2: Push to GitHub

### 2.1 Initialize Git Repository (if needed)
```bash
git init
```

### 2.2 Add All Files (except .env)
```bash
git add .
git status  # Verify .env is NOT listed
```

### 2.3 Create Initial Commit
```bash
git commit -m "Initial commit: ModelMirror Chrome extension with Vercel backend"
```

### 2.4 Create a GitHub Repository
1. Go to [github.com](https://github.com)
2. Click "New repository"
3. Name it: `modelmirror` (or your preferred name)
4. Do NOT initialize with README (we already have one)
5. Click "Create repository"

### 2.5 Connect and Push
```bash
git remote add origin https://github.com/YOUR_USERNAME/modelmirror.git
git branch -M main
git push -u origin main
```

---

## Step 3: Deploy Backend to Vercel

### Option A: Using Vercel CLI (Easiest)

#### 3A.1 Install Vercel CLI
```bash
npm install -g vercel
```

#### 3A.2 Login to Vercel
```bash
vercel login
```
Follow the browser prompt to authenticate with your Vercel account.

#### 3A.3 Deploy
```bash
vercel
```

You'll be asked:
- **Which scope?** → Select your account
- **Link to existing project?** → No (first time)
- **Project name?** → `modelmirror` (or your name)
- **Modify settings?** → No

#### 3A.4 Add Environment Variables
After deployment completes:
1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Click your project (`modelmirror`)
3. Settings → Environment Variables
4. Add: `GEMINI_API_KEY` = `your_actual_key`
5. Click "Save"
6. Click the three dots menu → Redeploy

---

### Option B: Using GitHub + Vercel Dashboard

#### 3B.1 Deploy via Vercel Dashboard
1. Go to [vercel.com/new](https://vercel.com/new)
2. Click "Import Git Repository"
3. Paste: `https://github.com/YOUR_USERNAME/modelmirror`
4. Click "Continue"

#### 3B.2 Configure Project
- **Project name:** `modelmirror`
- **Framework:** Express
- Leave other settings as default

#### 3B.3 Add Environment Variables
- Click "Environment Variables"
- Name: `GEMINI_API_KEY`
- Value: `your_actual_key`
- Click "Add"
- Click "Deploy"

#### 3B.4 Setup Automatic Deployments (Optional)
Every `git push` to `main` will automatically redeploy.

---

## Step 4: Update Extension with Deployment URL

### 4.1 Get Your Vercel URL
After deployment, Vercel shows your URL (e.g., `https://modelmirror-xyz123.vercel.app`).

### 4.2 Update background.js
Edit [background.js](./background.js) line ~5:
```javascript
// Change from:
const DEFAULT_SERVER_URL = 'https://modelmirror.vercel.app';

// To your actual URL:
const DEFAULT_SERVER_URL = 'https://modelmirror-xyz123.vercel.app';
```

### 4.3 Commit and Push
```bash
git add background.js
git commit -m "Update server URL to Vercel deployment"
git push
```

Vercel will auto-redeploy on push (if connected to GitHub).

---

## Step 5: Reload Extension and Test

### 5.1 Reload Extension in Chrome
1. Open `chrome://extensions`
2. Find "ModelMirror"
3. Click the reload icon

### 5.2 Test the Extension
1. Visit any code-hosting website (GitHub, CodePen, etc.)
2. Select some code
3. Click the ModelMirror extension icon
4. Click "Grab Selection"
5. Select an action (e.g., "Explain")
6. Verify it works (should show green dot for healthy server)

### 5.3 Check Server Health
The side panel should show a green dot next to "Server Status" indicating the Vercel backend is connected.

---

## Common Issues & Fixes

### Issue: Vercel deployment fails with Node version error
**Fix:** Update `vercel.json` to use a compatible Node version:
```json
{
  "buildCommand": "npm install",
  "functions": {
    "server.js": {
      "runtime": "nodejs20.x"
    }
  }
}
```

### Issue: "Backend not responding" in extension
**Fix:**
1. Confirm Vercel URL in `background.js` is correct
2. Check Vercel project logs: [vercel.com/dashboard](https://vercel.com/dashboard)
3. Verify `GEMINI_API_KEY` is set in Vercel environment variables
4. If issues persist, check that `https://*.vercel.app/*` is in `manifest.json` host_permissions

### Issue: API key errors in Vercel logs
**Fix:**
1. In Vercel dashboard, verify the environment variable is set
2. The key must be **exactly** correct
3. After updating, click "Redeploy" to apply changes

### Issue: Local testing still works, but Vercel doesn't
**Fix:**
- Ensure all code changes are committed and pushed
- Vercel rebuilds on each push
- Check Vercel logs for build errors

---

## Verification Checklist

- [x] `.env` file exists locally (with your API key)
- [x] `.env` is in `.gitignore` (not pushed to GitHub)
- [x] `.env.example` is in the repository (with placeholder values)
- [x] GitHub repository is created and updated
- [x] Vercel project is deployed
- [x] `GEMINI_API_KEY` is set in Vercel environment variables
- [x] `background.js` has the correct Vercel URL
- [x] Extension is reloaded in Chrome
- [x] Extension can connect to backend (green server status)
- [x] At least one action test passes (e.g., Explain)

---

## Next Steps

### Optional Enhancements

1. **Custom domain:** 
   - Go to Vercel Settings → Domains
   - Add your custom domain (e.g., api.mysite.com)

2. **Upgrade to production Gemini model:**
   - Edit `.env`: `GEMINI_MODEL=gemini-1.5-pro`
   - Update in Vercel environment variables
   - Redeploy

3. **Share the extension:**
   - Publish to Chrome Web Store (requires $5 developer account)
   - Share the GitHub repo with others

---

Need help? Check the [README.md](./README.md) or review Vercel/Google Generative AI documentation.
