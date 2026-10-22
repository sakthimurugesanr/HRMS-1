# HRMS — Human Resource Management System

A full-stack HR management application built with React + TypeScript on the frontend and Node.js + Express on the backend, backed by PostgreSQL.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Clone the repository](#1-clone-the-repository)
  - [2. Configure the server](#2-configure-the-server)
  - [3. Set up the database](#3-set-up-the-database)
  - [4. Start the server](#4-start-the-server)
  - [5. Start the client](#5-start-the-client)
- [Environment Variables](#environment-variables)
- [Available Scripts](#available-scripts)
- [API Overview](#api-overview)
- [Roles & Permissions](#roles--permissions)
- [Default Credentials](#default-credentials)

---

## Features

- **Employee Management** — Create, update, and manage employee profiles including personal details, job information, supervisors, and profile photos
- **Leave Management** — Apply for, approve, reject, and cancel leave requests with balance tracking and entitlement management
- **Performance Management** — Appraisal cycles, templates, KPI tracking, and supervisor/self-review ratings
- **HR Admin** — User management, role assignment, and access control
- **Reports** — Export leave summaries and detailed reports as Excel files
- **Data Migration** — Bulk import employees and leave data via Excel upload
- **Role-based Access Control** — Granular permissions for employees, supervisors, and HR admins
- **Email Notifications** — Automated emails via SMTP for password resets and key HR events
- **Scheduled Jobs** — Cron-based background tasks for entitlement resets and expiry checks

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React 18 | UI framework |
| TypeScript 5 | Type safety |
| Vite 4 | Build tool and dev server |
| Redux Toolkit | Global state management |
| React Router v6 | Client-side routing |
| Tailwind CSS 3 | Utility-first styling |
| Axios | HTTP client |
| Lucide React | Icon library |

### Backend
| Technology | Purpose |
|---|---|
| Node.js | Runtime |
| Express 4 | HTTP framework |
| PostgreSQL | Relational database |
| `pg` | PostgreSQL client |
| JWT | Authentication tokens |
| bcryptjs | Password hashing |
| Multer | File uploads |
| ExcelJS | Excel export and import |
| Nodemailer | Email delivery |
| node-cron | Scheduled background jobs |
| Winston | Logging |

---

## Project Structure

```
hrms/
├── client/                  # React frontend
│   ├── src/
│   │   ├── api/             # Axios API call functions
│   │   ├── app/             # Redux store hooks
│   │   ├── assets/          # Static images and logos
│   │   ├── components/      # Shared and layout components
│   │   ├── config/          # Role constants, route paths, UI constants
│   │   ├── constants/       # Reusable message and placeholder strings
│   │   ├── hooks/           # Custom React hooks
│   │   ├── pages/           # Feature pages (employees, leave, performance, etc.)
│   │   ├── store/           # Redux slices
│   │   ├── types/           # TypeScript interfaces and types
│   │   └── utils/           # Helpers (toast, avatar, errors, etc.)
│   ├── index.html
│   ├── package.json
│   └── vite.config.ts
│
└── server/                  # Node.js backend
    ├── src/
    │   ├── config/          # Database connection
    │   ├── constants/       # Role definitions and shared constants
    │   ├── controllers/     # Route handler logic
    │   ├── jobs/            # Cron job definitions
    │   ├── middleware/       # Auth, error handling, file upload middleware
    │   ├── models/          # Database query functions
    │   ├── repositories/    # Data access layer
    │   ├── routes/          # Express route definitions
    │   ├── services/        # Business logic services
    │   ├── utils/           # Logger, response helpers, AppError
    │   ├── validators/      # Input validation
    │   └── index.js         # App entry point
    ├── .env
    └── package.json
```

---

## Prerequisites

- **Node.js** v18 or higher
- **npm** v9 or higher
- **PostgreSQL** v14 or higher

---

## Getting Started

### 1. Clone the repository

```bash
git clone <repository-url>
cd hrms
```

### 2. Configure the server

```bash
cd server
cp .env.example .env
```

Update `.env` with your PostgreSQL credentials and SMTP settings. See [Environment Variables](#environment-variables) for the full list.

### 3. Set up the database

Create the database, then run migrations to create all tables:

```bash
# Create the database in PostgreSQL
createdb hrms

# Run migrations
npm run migrate

# Seed the default admin user
npm run defaultuser
```

### 4. Start the server

```bash
# Development (with auto-restart via nodemon)
npm run dev

# Production
npm start
```

The server runs on `http://localhost:5001` by default.

### 5. Start the client

Open a new terminal:

```bash
cd client
npm install
npm run dev
```

The client runs on `http://localhost:5173` by default.

---

## Environment Variables

Create a `.env` file inside the `server/` directory with the following variables:

```env
# Application
NODE_ENV=development
PORT=5001

# Authentication
JWT_SECRET=your-secret-key-change-this-in-production
JWT_EXPIRES_IN=8h
REMEMBER_ME_DURATION=30d

# CORS — comma-separated list of allowed frontend origins
CORS_ORIGINS=http://localhost:5173,http://localhost:5174

# Database
DATABASE_URL=postgres://username:password@localhost:5432/hrms
DB_HOST=localhost
DB_PORT=5432
DB_NAME=hrms
DB_USER=postgres
DB_PASSWORD=your-password

# SMTP (for email notifications)
SMTP_USER=your-email@example.com
SMTP_PASS=your-app-password
MAIL_FROM=your-email@example.com

# Leave migration settings
LEAVE_MIGRATION_HOURS_PER_DAY=8
LEAVE_MIGRATION_DATE_ORDER=DMY

# Default password for newly created users
DEFAULT_PASSWORD=admin!@123
```

> **Note:** Never commit `.env` to version control. Add it to `.gitignore`.

---

## Available Scripts

### Server (`/server`)

| Script | Description |
|---|---|
| `npm run dev` | Start server in development mode with nodemon |
| `npm start` | Start server in production mode |
| `npm run migrate` | Run pending database migrations |
| `npm run migrate:status` | Show migration status |
| `npm run defaultuser` | Seed the default admin user |

### Client (`/client`)

| Script | Description |
|---|---|
| `npm run dev` | Start Vite dev server |
| `npm run build` | Type-check and build for production |
| `npm run preview` | Preview the production build locally |

---

## API Overview

All API routes are prefixed with `/api`.

| Prefix | Description |
|---|---|
| `/api/auth` | Login, logout, password reset |
| `/api/employees` | Employee CRUD, profile images, supervisors |
| `/api/leave` | Leave requests, approvals, rejections, cancellations, exports |
| `/api/entitlements` | Leave entitlement management |
| `/api/performance` | Appraisal cycles, templates, KPIs, ratings |
| `/api/hradmin` | HR admin user management and role access |
| `/api/roles` | Role definitions |
| `/api/reports` | Report generation and export |
| `/api/migration` | Bulk data import via Excel |

---

## Roles & Permissions

| Role | Access |
|---|---|
| `employee` | View own profile, apply for leave, submit self-appraisal |
| `supervisor` | Manage team leave, approve/reject requests, submit supervisor appraisal |
| `hradmin` | Full access — employee management, entitlements, reports, role assignment |

---

## Default Credentials

After running `npm run defaultuser`, log in with:

```
Email:    admin@example.com
Password: admin!@123
```

> Change the default password immediately after the first login.
