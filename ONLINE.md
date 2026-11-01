# 🌐 Get Sports CMS Online (Free Tier – ~15 minutes)

This guide puts your system live on the internet using free services.

## Architecture (Free)

| Part       | Service     | Cost   | URL style                          |
|------------|-------------|--------|------------------------------------|
| Database   | Neon        | Free   | managed PostgreSQL                 |
| Backend API| Railway **or** Render | Free | `https://your-api.up.railway.app` |
| Frontend   | Vercel      | Free   | `https://sports-cms.vercel.app`    |

---

## Step 1 – Database (Neon) – 3 minutes

1. Go to → https://neon.tech and sign up (GitHub login is fastest)
2. Create a new project → name it `sports-cms`
3. Copy the **connection string** (it looks like):
   ```
   postgresql://user:password@ep-xxx.region.aws.neon.tech/neondb?sslmode=require
   ```
4. Keep this string – you will need it twice.

---

## Step 2 – Backend API (Railway – recommended)

1. Go to → https://railway.app and sign up with GitHub
2. Click **New Project** → **Deploy from GitHub repo**
   - First push this code to a GitHub repository (see “Push to GitHub” below)
3. Select the repository and set the **Root Directory** to `apps/api`
4. Add these **Variables** (Settings → Variables):

```
DATABASE_URL=postgresql://... (paste Neon string)
JWT_SECRET=any-long-random-string-at-least-32-characters-here
JWT_REFRESH_SECRET=another-different-long-random-string-here
CORS_ORIGIN=*
NODE_ENV=production
PORT=4000
```

5. In Settings → Deploy, set:
   - **Build Command**: `npm install && npx prisma generate && npx prisma db push && npm run build`
   - **Start Command**: `npx prisma db seed && node dist/server.js`
     (or just `node dist/server.js` after first successful seed)

6. Deploy. Railway will give you a public URL, e.g.:
   ```
   https://sports-cms-api-production-xxxx.up.railway.app
   ```
7. Test it: open `https://YOUR-API-URL/health` → should return `{"status":"ok"}`

### Alternative: Render.com

1. https://render.com → New → Web Service
2. Connect GitHub repo, Root Directory = `apps/api`
3. Build: `npm install && npx prisma generate && npx prisma db push && npm run build`
4. Start: `node dist/server.js`
5. Add the same environment variables as above

---

## Step 3 – Frontend (Vercel) – 5 minutes

1. Go to → https://vercel.com and sign up with GitHub
2. **Add New Project** → Import your GitHub repo
3. Configure:
   - **Framework Preset**: Next.js
   - **Root Directory**: `apps/web`
   - **Build Command**: `npm run build` (default)
4. Add Environment Variable:
   ```
   NEXT_PUBLIC_API_URL = https://YOUR-API-URL/api
   ```
   (use the Railway/Render URL from Step 2 + `/api`)

5. Deploy. Vercel gives you:
   ```
   https://sports-cms-xxxxx.vercel.app
   ```

6. (Optional but recommended) After first deploy, go back to Railway and update:
   ```
   CORS_ORIGIN=https://sports-cms-xxxxx.vercel.app
   ```
   Then redeploy the API.

---

## Step 4 – Login

Open your Vercel URL and login with:

```
Email:    admin@federation.gov
Password: ChangeMe123!
```

---

## Push code to GitHub (required for Railway / Vercel)

```bash
cd sports-cms
git init
git add .
git commit -m "Sports Competition Management System"
# Create a new empty repo on GitHub, then:
git remote add origin https://github.com/YOUR-USERNAME/sports-cms.git
git branch -M main
git push -u origin main
```

---

## Quick Checklist

- [ ] Neon database created + connection string copied
- [ ] Code pushed to GitHub
- [ ] Railway/Render API deployed + `/health` works
- [ ] Vercel frontend deployed with `NEXT_PUBLIC_API_URL` set
- [ ] Can login at the Vercel URL

---

## Troubleshooting

| Problem                    | Fix |
|---------------------------|-----|
| API 500 on first request  | Check `DATABASE_URL` and that `prisma db push` ran |
| CORS error in browser     | Set `CORS_ORIGIN=*` temporarily, or exact Vercel URL |
| Login fails               | Confirm seed ran (`npx prisma db seed`) |
| Frontend can’t reach API  | Check `NEXT_PUBLIC_API_URL` ends with `/api` |

---

## After it is online

1. Change the default passwords immediately
2. Restrict `CORS_ORIGIN` to your real domain
3. Generate strong random JWT secrets
4. Consider a custom domain (Vercel + Railway both support free custom domains)

Your system will then be publicly accessible like a real national federation platform.
