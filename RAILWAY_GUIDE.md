# 🚂 Railway Deployment Guide for HSM Tech Bot

Deploy your WhatsApp bot on Railway with persistent sessions and automatic restarts.

---

## 📋 Prerequisites

1. A [Railway](https://railway.app/) account (free tier available)
2. A [GitHub](https://github.com/) account
3. Your bot code pushed to a GitHub repository
4. Environment variables ready (from `.env` file)

---

## 🚀 Step-by-Step Deployment

### Step 1: Push Code to GitHub

If your code isn't on GitHub yet:

```bash
# Initialize git (if needed)
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit - HSM Tech Bot"

# Add your GitHub repo as remote
git remote add origin https://github.com/YOUR_USERNAME/hsm-tech-bot.git

# Push to GitHub
git push -u origin main
```

### Step 2: Create Railway Project

1. Go to [railway.app](https://railway.app/)
2. Click **"Login"** → Sign in with GitHub
3. Click **"New Project"**
4. Select **"Deploy from GitHub repo"**
5. Find and select your `hsm-tech-bot` repository
6. Click **"Deploy Now"**

### Step 3: Configure Environment Variables

1. In your Railway project dashboard, click on your service
2. Go to the **"Variables"** tab
3. Click **"Raw Editor"** and paste your variables:

```env
# Required Variables
OWNER_NUMBER=923001234567
BOT_NAME=HSM Tech Bot
BOT_PREFIX=!

# Gemini AI (Required for AI features)
GEMINI_API_KEY=your_gemini_api_key_here

# Optional Features
ENABLE_AI=true
ENABLE_LOGS=true
LOG_LEVEL=info
NODE_ENV=production
```

4. Click **"Update Variables"**

### Step 4: Add Persistent Volume (Important!)

WhatsApp sessions need persistent storage. Without this, you'll need to re-scan QR code after every deploy.

1. In your project, click **"+ New"**
2. Select **"Database"** → **"Add Volume"** (or right-click your service)
3. Or go to service **Settings** → **"Mount Volume"**
4. Set mount path: `/app/auth`
5. Click **"Save"**

### Step 5: First-Time QR Code Scan

1. Go to **"Deployments"** tab in Railway
2. Click on the latest deployment
3. Click **"View Logs"**
4. You'll see a QR code in the logs - scan it with WhatsApp
5. Once scanned, the bot will connect!

---

## 📁 Project Files for Railway

Your project should have these files (already configured):

### `Procfile`
```
web: npm start
```

### `package.json` (scripts section)
```json
{
  "scripts": {
    "start": "node bot.js"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

### `runtime.txt`
```
nodejs-20.x
```

---

## 🔧 Railway Configuration

### railway.json (Optional - Add for custom settings)

Create this file for advanced configuration:

```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "npm start",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

---

## 💰 Railway Pricing

| Plan | Usage | Price |
|------|-------|-------|
| **Trial** | $5 credit | Free |
| **Hobby** | $5/month | $5/mo |
| **Pro** | Usage-based | From $20/mo |

**Estimated Bot Usage:**
- CPU: ~0.1-0.2 vCPU
- Memory: ~200-400 MB
- Cost: ~$2-5/month on Hobby plan

---

## 🔄 Auto-Deploy on Git Push

Railway automatically deploys when you push to GitHub:

```bash
# Make changes to your code
git add .
git commit -m "Update bot features"
git push origin main
# Railway will auto-deploy!
```

---

## 🐛 Troubleshooting

### QR Code Not Showing
- Check logs immediately after deploy
- QR code expires in ~60 seconds
- Redeploy if you miss it

### Bot Disconnects After Deploy
- Ensure volume is mounted at `/app/auth`
- Check that `auth/` folder is in `.gitignore`

### Memory Issues
- Upgrade to Hobby plan if on Trial
- Check for memory leaks in logs

### Session Invalid
1. Delete the volume data
2. Redeploy
3. Scan new QR code

### View Logs
```bash
# In Railway Dashboard
# Click: Deployments → Latest → View Logs
```

---

## 🔐 Security Best Practices

1. **Never commit `.env`** - Use Railway Variables instead
2. **Keep `auth/` in `.gitignore`** - Session files are sensitive
3. **Use Railway's encrypted variables** - They're secure by default
4. **Enable 2FA on Railway** - Protect your deployments

---

## 📊 Monitoring Your Bot

### Health Check Endpoint (Optional)

Add to your `bot.js` for monitoring:

```javascript
// Simple HTTP server for Railway health checks
const http = require('http');

const server = http.createServer((req, res) => {
    if (req.url === '/health') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ status: 'ok', uptime: process.uptime() }));
    } else {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('HSM Tech Bot is running!');
    }
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`Health check server running on port ${PORT}`);
});
```

---

## 📞 Support

- **Railway Docs**: [docs.railway.app](https://docs.railway.app/)
- **Railway Discord**: [discord.gg/railway](https://discord.gg/railway)
- **Issues**: Create an issue in your GitHub repo

---

## ✅ Deployment Checklist

- [ ] Code pushed to GitHub
- [ ] Railway project created
- [ ] GitHub repo connected
- [ ] Environment variables configured
- [ ] Persistent volume mounted at `/app/auth`
- [ ] QR code scanned successfully
- [ ] Bot responding to messages
- [ ] Auto-deploy working

---

**Happy Deploying! 🚀**
