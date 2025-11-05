# 🚀 COMPLETE DEPLOYMENT GUIDE - Railway.app (FREE)

## ✅ **Why Railway.app?**

- ✅ **$5 free credit/month** (enough for development/testing)
- ✅ **WebSocket support** (essential for real-time features)
- ✅ **GitHub integration** (auto-deploy on push)
- ✅ **Environment variables** (secure credentials)
- ✅ **Automatic HTTPS**
- ✅ **Custom domains** (optional)
- ✅ **Easy setup** (5-10 minutes)

---

## 📋 **Prerequisites**

1. ✅ GitHub account
2. ✅ MongoDB Atlas account (free tier)
3. ✅ Groq API key (free tier)
4. ✅ Code pushed to GitHub repository

---

## 🚀 **STEP-BY-STEP DEPLOYMENT**

### **Step 1: Sign Up for Railway**

1. Go to https://railway.app
2. Click **"Start a New Project"**
3. Sign in with **GitHub**
4. Authorize Railway to access your repositories

---

### **Step 2: Deploy Backend**

#### **2.1 Create New Project**

1. Click **"New Project"**
2. Select **"Deploy from GitHub repo"**
3. Choose your repository: `eduscribe_realtime`
4. Railway will detect your project

#### **2.2 Configure Backend Service**

1. Click **"Add Service"** → **"GitHub Repo"**
2. Select `backend` folder as root directory
3. Railway will auto-detect Python

#### **2.3 Set Environment Variables**

Click on your backend service → **"Variables"** tab:

```env
# MongoDB
MONGODB_URL=mongodb+srv://your_username:your_password@cluster.mongodb.net/eduscribe

# Groq API
GROQ_API_KEY=your_groq_api_key_here

# JWT Secret
JWT_SECRET_KEY=your_super_secret_key_here_min_32_chars

# LLM Model
LLM_MODEL=llama-3.1-8b-instant

# Port (Railway sets this automatically)
PORT=${{RAILWAY_PORT}}
```

**How to get these:**

**MongoDB URL:**
1. Go to MongoDB Atlas → Clusters
2. Click "Connect" → "Connect your application"
3. Copy the connection string
4. Replace `<password>` with your actual password
5. Replace `myFirstDatabase` with `eduscribe`

**Groq API Key:**
1. Go to https://console.groq.com
2. Sign up/Login
3. Go to "API Keys"
4. Create new key
5. Copy the key

**JWT Secret:**
```bash
# Generate a secure random key
python -c "import secrets; print(secrets.token_urlsafe(32))"
```

#### **2.4 Deploy Backend**

1. Click **"Deploy"**
2. Wait for build to complete (~3-5 minutes)
3. Once deployed, you'll get a URL like: `https://your-backend.railway.app`
4. **Copy this URL** - you'll need it for frontend!

---

### **Step 3: Deploy Frontend**

#### **3.1 Create Frontend Service**

1. In the same Railway project, click **"New Service"**
2. Select **"GitHub Repo"** again
3. Choose `frontend` folder as root directory
4. Railway will auto-detect Node.js/Vite

#### **3.2 Set Environment Variables**

Click on frontend service → **"Variables"** tab:

```env
# Backend URL (use your Railway backend URL from Step 2.4)
VITE_API_URL=https://your-backend.railway.app

# WebSocket URL (same as backend but with wss://)
VITE_WS_URL=wss://your-backend.railway.app
```

**IMPORTANT:** Replace `your-backend.railway.app` with your actual backend URL!

#### **3.3 Deploy Frontend**

1. Click **"Deploy"**
2. Wait for build to complete (~2-3 minutes)
3. Once deployed, you'll get a URL like: `https://your-frontend.railway.app`
4. **This is your live app URL!** 🎉

---

### **Step 4: Update CORS Settings**

#### **4.1 Update Backend CORS**

Railway will provide your frontend URL. Update CORS in backend:

1. Go to your GitHub repository
2. Edit `backend/optimized_main.py`
3. Find the CORS middleware section (around line 27)
4. Update to:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:3000",  # Local development
        "https://your-frontend.railway.app",  # Production
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

5. Commit and push:
```bash
git add backend/optimized_main.py
git commit -m "Update CORS for production"
git push realtime main
```

6. Railway will auto-deploy the update!

---

### **Step 5: Test Your Deployment**

1. Open your frontend URL: `https://your-frontend.railway.app`
2. Sign up for a new account
3. Create a subject
4. Start a lecture
5. Upload a PDF document
6. Click "Start Recording"
7. Speak for 1-2 minutes
8. Watch real-time transcription and notes appear!

---

## 🎯 **QUICK DEPLOYMENT CHECKLIST**

- [ ] Code pushed to GitHub
- [ ] Railway account created
- [ ] Backend service deployed
- [ ] MongoDB Atlas URL added to backend env vars
- [ ] Groq API key added to backend env vars
- [ ] JWT secret generated and added
- [ ] Backend URL copied
- [ ] Frontend service deployed
- [ ] Backend URL added to frontend env vars
- [ ] CORS updated with frontend URL
- [ ] App tested and working!

---

## 💰 **Cost Breakdown (FREE TIER)**

### **Railway:**
- **Free Credit:** $5/month
- **Usage:** ~$3-4/month for both services
- **Verdict:** ✅ **FREE** for development/testing

### **MongoDB Atlas:**
- **Free Tier:** 512MB storage
- **Verdict:** ✅ **FREE** forever

### **Groq API:**
- **Free Tier:** 6000 tokens/minute
- **Verdict:** ✅ **FREE** for moderate usage

**Total Cost:** **$0/month** 🎉

---

## 🔧 **Troubleshooting**

### **Backend won't start:**
```bash
# Check logs in Railway dashboard
# Common issues:
- Missing environment variables
- MongoDB connection string incorrect
- Port binding issue (should use $PORT)
```

### **Frontend can't connect to backend:**
```bash
# Check:
- VITE_API_URL is correct
- VITE_WS_URL uses wss:// (not ws://)
- CORS allows your frontend domain
```

### **WebSocket connection fails:**
```bash
# Ensure:
- Backend URL uses wss:// for WebSocket
- Railway supports WebSocket (it does!)
- No firewall blocking connections
```

### **MongoDB connection fails:**
```bash
# Check:
- Connection string is correct
- Password doesn't have special characters (or URL encode them)
- IP whitelist in MongoDB Atlas (set to 0.0.0.0/0 for Railway)
```

---

## 🚀 **Alternative: Render.com (Also Free)**

If Railway doesn't work, try Render:

### **Render.com Setup:**

1. Go to https://render.com
2. Sign up with GitHub
3. Create **Web Service** for backend
4. Create **Static Site** for frontend
5. Same environment variables as Railway
6. **Free Tier:** 750 hours/month (enough for 1 service)

**Pros:**
- ✅ Also free
- ✅ WebSocket support
- ✅ Easy deployment

**Cons:**
- ⚠️ Slower cold starts
- ⚠️ Services sleep after 15 min inactivity

---

## 📊 **Deployment Comparison**

| Feature | Railway | Render | Vercel |
|---------|---------|--------|--------|
| **Backend** | ✅ Full support | ✅ Full support | ⚠️ Serverless only |
| **Frontend** | ✅ Static + SSR | ✅ Static only | ✅ Best for React |
| **WebSocket** | ✅ Yes | ✅ Yes | ❌ No |
| **Free Tier** | $5 credit/month | 750 hrs/month | Unlimited |
| **Cold Start** | Fast | Slow | Very fast |
| **Best For** | **Full-stack** | Full-stack | Frontend only |

**Winner for EduScribe:** **Railway.app** 🏆

---

## 🎉 **SUCCESS!**

Your EduScribe app is now live and accessible worldwide!

**Share your app:**
- Frontend: `https://your-frontend.railway.app`
- Backend API: `https://your-backend.railway.app`

**Next steps:**
- Add custom domain (optional)
- Set up monitoring
- Enable auto-deploy on push
- Add more features!

---

## 📝 **Important Notes**

### **Railway Free Tier Limits:**
- **$5 credit/month**
- **500 hours** of usage
- **100GB** bandwidth
- **1GB** RAM per service

**Estimated usage for EduScribe:**
- Backend: ~$2-3/month
- Frontend: ~$1/month
- **Total: ~$3-4/month** (within free tier!)

### **When you exceed free tier:**
- Railway will notify you
- You can add a credit card
- Or optimize your usage
- Or use Render as backup

---

## 🔐 **Security Best Practices**

1. ✅ **Never commit `.env` files**
2. ✅ **Use environment variables** for all secrets
3. ✅ **Rotate JWT secret** periodically
4. ✅ **Use strong passwords** for MongoDB
5. ✅ **Enable MongoDB IP whitelist** (0.0.0.0/0 for Railway)
6. ✅ **Monitor API usage** (Groq dashboard)
7. ✅ **Set up CORS** properly

---

## 📞 **Need Help?**

- **Railway Docs:** https://docs.railway.app
- **Railway Discord:** https://discord.gg/railway
- **Render Docs:** https://render.com/docs
- **MongoDB Docs:** https://docs.atlas.mongodb.com

---

**Your app is now LIVE! 🚀📝✨**
