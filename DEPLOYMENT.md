# Deployment Guide – Sports Competition Management System

## Local Development

1. Start PostgreSQL (or use Docker):
   ```bash
   docker compose up db -d
   ```

2. Copy environment:
   ```bash
   cp .env.example .env
   ```

3. Install dependencies (from root):
   ```bash
   npm install
   cd apps/api && npm install
   cd ../web && npm install
   ```

4. Run migrations & seed:
   ```bash
   cd apps/api
   npx prisma migrate dev --name init
   npx prisma db seed
   ```

5. Start services:
   ```bash
   # Terminal 1 – API
   cd apps/api && npm run dev

   # Terminal 2 – Web
   cd apps/web && npm run dev
   ```

- Web: http://localhost:3000
- API: http://localhost:4000
- Login: `admin@federation.gov` / `ChangeMe123!`

## Production with Docker Compose

```bash
docker compose up -d --build
```

## Recommended Production Stack

| Component   | Recommendation                          |
|-------------|-----------------------------------------|
| Database    | Neon / Supabase / AWS RDS / DigitalOcean|
| API         | Railway / Render / Fly.io / AWS ECS     |
| Frontend    | Vercel (best for Next.js)               |
| File Storage| Cloudflare R2 or AWS S3                 |
| CDN         | Cloudflare                              |

## Security Checklist

- [ ] Change all default secrets and passwords
- [ ] Enable HTTPS only
- [ ] Configure CORS to your production domain
- [ ] Set strong JWT secrets (32+ chars)
- [ ] Enable rate limiting
- [ ] Regular database backups
- [ ] Monitor audit logs
- [ ] Keep dependencies updated

## Environment Variables (Production)

```
DATABASE_URL=
JWT_SECRET=
JWT_REFRESH_SECRET=
CORS_ORIGIN=https://your-domain.com
NEXT_PUBLIC_API_URL=https://api.your-domain.com/api
NODE_ENV=production
```
