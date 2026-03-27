# Quick Start Guide

Get your ModelMirror extension running on GitHub and Vercel in 5 minutes.

## Prerequisites

- Node.js 18+ installed
- Git installed
- GitHub account
- Vercel account (free at [vercel.com](https://vercel.com))
- OpenRouter API key (free at [openrouter.ai](https://openrouter.ai))

## 1️⃣ Local Setup (2 minutes)

```bash
# Install dependencies
npm install

# Copy environment template
cp .env.example .env

# Edit .env with your OpenRouter API key
# (Open with your editor and add: OPENROUTER_API_KEY=your_key)
```

**Test locally:**
```bash
node server.js
# Server should run on http://localhost:3000
# Open chrome://extensions, reload the extension, and test
```

## 2️⃣ Push to GitHub (1 minute)

```bash
# Create a repo on GitHub (https://github.com/new)
# Then:

git init
git add .
git commit -m "Initial commit: ModelMirror extension"
git remote add origin https://github.com/YOUR_USERNAME/modelmirror.git
git branch -M main
git push -u origin main
```

## 3️⃣ Deploy to Vercel (2 minutes)

### Using Vercel CLI (Easiest)
```bash
npm install -g vercel
vercel login
vercel
# When prompted, add OPENROUTER_API_KEY in the environment variables
```

### Or Using Vercel Dashboard
1. Go to [vercel.com/new](https://vercel.com/new)
2. Import your GitHub repo
3. Add `OPENROUTER_API_KEY` environment variable
4. Deploy

## 4️⃣ Update Extension with Vercel URL

After deployment, you'll get a URL like `https://modelmirror-xyz.vercel.app`

Edit `background.js` line 5:
```javascript
const DEFAULT_SERVER_URL = 'https://modelmirror-xyz.vercel.app'; // ← Your Vercel URL
```

## 5️⃣ Reload & Test

1. Open `chrome://extensions`
2. Reload the ModelMirror extension
3. Visit a website with code and test the extension
4. You should see a green server status dot

---

## ✅ Done!

Your extension is now:
- ✅ On GitHub (for version control and sharing)
- ✅ Running on Vercel (no local server needed)
- ✅ Ready to use (will auto-update when you push to GitHub)

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `Backend not responding` | Update Vercel URL in `background.js` |
| `API key error` | Check `.env` locally, and Environment Variables in Vercel |
| `Server won't start locally` | Run `npm install` first |
| `Git push fails` | Run `git remote -v` to verify the GitHub URL |

---

See [DEPLOYMENT.md](./DEPLOYMENT.md) for detailed instructions.
