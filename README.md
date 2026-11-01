# Sports Competition Management System (SCMS)

Enterprise-grade Sports Competition Management System designed for national and regional sporting organizations (Football Associations, Rugby Unions, Athletics Federations, etc.).

## Features

- **Role-Based Access Control**: Super Admin · Admin · Viewer
- Competition & Tournament management
- Team registration & profiles (with logos)
- Player registration (unique ID, photo, medical notes)
- Automatic & manual fixture generation (Round Robin, Knockout, League)
- Team Sheets (Match Day slips) + PDF export
- Official Match Reports + PDF
- Live Results & automatic standings calculation
- Points / League tables with tie-breakers
- Reports & Analytics (PDF, Excel, CSV)
- Global search & advanced filters
- Notifications
- Full Audit Logging
- Modern responsive dashboard (dark mode ready)

## Tech Stack

| Layer      | Technology                          |
|------------|-------------------------------------|
| Frontend   | Next.js 14 (App Router) + TypeScript + Tailwind + shadcn/ui |
| Backend    | Node.js + Express + TypeScript      |
| ORM        | Prisma                              |
| Database   | PostgreSQL                          |
| Auth       | JWT + Refresh Tokens + RBAC         |
| PDF        | @react-pdf/renderer / Puppeteer     |
| File Upload| Multer + Sharp + S3/R2 compatible   |

## Project Structure

```
sports-cms/
├── apps/
│   ├── api/                 # Express REST API
│   └── web/                 # Next.js frontend
├── packages/
│   └── shared/              # Shared types & utilities
├── docker-compose.yml
└── README.md
```

## Quick Start

### 1. Prerequisites
- Node.js 20+
- PostgreSQL 15+
- pnpm (recommended) or npm

### 2. Clone & Install

```bash
cd sports-cms
pnpm install
```

### 3. Environment

```bash
cp .env.example .env
# Edit DATABASE_URL, JWT_SECRET, etc.
```

### 4. Database

```bash
cd apps/api
pnpm prisma migrate dev
pnpm prisma db seed
```

### 5. Run Development

```bash
# From root
pnpm dev          # starts both API (4000) and Web (3000)
```

- Frontend: http://localhost:3000
- API: http://localhost:4000
- Default Super Admin: `admin@federation.gov` / `ChangeMe123!`

## Default Roles

| Role         | Create/Edit/Delete | View | Manage Users | Audit Logs |
|--------------|--------------------|------|--------------|------------|
| SUPER_ADMIN  | ✅                 | ✅   | ✅           | ✅         |
| ADMIN        | ✅                 | ✅   | ❌           | ❌         |
| VIEWER       | ❌                 | ✅   | ❌           | ❌         |

## Production Deployment

See `DEPLOYMENT.md` for detailed instructions (Docker, Vercel, Railway, AWS).

## License

Proprietary – for authorized sporting organizations only.
