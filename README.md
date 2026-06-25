# Dr. Shahidur Rahman Khan's Platform — Backend

> A production-grade REST API serving **two independent frontends** (public Next.js site + admin dashboard) for Dr. Md. Shahidur Rahman Khan, Associate Professor at NITOR, Dhaka. Built with **Express 5**, **TypeScript**, and **MongoDB**. Features **multi-channel notifications (Email + WhatsApp + Telegram)**, magic-link auth, audit trails, and 14 modular modules. Built as a **team project (2 developers)**.

[![Frontend Repo](https://img.shields.io/badge/Frontend_Repo-GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/tarekul42/Dr-Shahidur-s-Portfolio-frontend)
[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg?style=flat-square)](https://opensource.org/licenses/ISC)

---

## 📋 Overview

This is the backend API for **Dr. Md. Shahidur Rahman Khan's** professional platform — a real client deliverable for an Associate Professor of Orthopedic & Trauma Surgery at the **National Institute of Traumatology and Orthopaedic Rehabilitation (NITOR), Dhaka**. The API serves **two independent frontends** from a single codebase: a public-facing Next.js website (for patients/visitors) and a Vite + React admin dashboard (for the doctor and his team).

This was a **team project (2 developers)**. My contributions focused on the **multi-channel notification system** (Email + WhatsApp + Telegram), **magic-link authentication**, **decoupled file upload strategy**, **Redis JTI blacklist for refresh-token invalidation**, and the **activity-log middleware**.

The API follows a strict **MVC architecture** (Model → Service → Controller → Routes) across 14 modules: `auth`, `users`, `appointment`, `article`, `research`, `testimonial`, `Chembers`, `contact`, `upload`, `search`, `analytics`, `activity-log`, `app-info`, `visitor`.

---

## 🛠️ Tech Stack

| Category | Technology |
|----------|-----------|
| Runtime | Node.js (with ts-node-dev) |
| Framework | Express.js 5 |
| Language | TypeScript 6 |
| Database | MongoDB (Mongoose 9) |
| Cache | Redis (ioredis) |
| Auth | JWT (access + refresh rotation) + bcrypt (salt 12) |
| Validation | express-validator + Zod |
| Logging | Morgan + chalk |
| File Storage | ImageKit (images + PDFs) + Cloudinary (videos) |
| Email | Nodemailer + Brevo (@getbrevo/brevo) |
| WhatsApp | whatsapp-web.js |
| Telegram | node-telegram-bot-api |
| Real-time | Socket.io |
| Security | Helmet, hpp, cors, express-rate-limit, lusca, xss |
| Pagination | mongoose-paginate-v2 |
| Misc | slugify, dayjs, ua-parser-js, uuid, qrcode-terminal, compression, connect-timeout |

---

## ✨ Main Features

- **Multi-channel notification system** — when a visitor submits the contact form or books an appointment, the doctor is notified via **Email (Nodemailer/Brevo)**, **WhatsApp (whatsapp-web.js → his phone)**, and **Telegram (node-telegram-bot-api → channel)**. The WhatsApp integration runs a headless Chromium with persistent session storage and reconnection logic.
- **Magic-link authentication** — one-click login via email. The doctor doesn't need to remember a password — he clicks a link in his email and is logged in.
- **Decoupled file upload strategy** — frontend pre-uploads files to `/api/v1/upload`, gets back `{url, fileId}`, then attaches as JSON to entity endpoints. Keeps entity payloads strictly JSON, simplifies validation, and lets the frontend show upload progress independently.
- **Single backend, two frontends** — one API serves both the public Next.js site (`CLIENT_PUBLIC_URL`) and the admin dashboard (`CLIENT_DASHBOARD_URL`) with strict CORS whitelist per origin.
- **Redis JTI blacklist** — refresh tokens are invalidated immediately on logout via a Redis-backed JTI blacklist. No "logged out but token still works for 7 days" problem.
- **14 modular modules** — `auth`, `users`, `appointment`, `article`, `research`, `testimonial`, `Chembers`, `contact`, `upload`, `search`, `analytics`, `activity-log`, `app-info`, `visitor`
- **Activity-log middleware** — every protected route logs the action (who, what, when, IP, user-agent) for accountability
- **Bilingual slugify** — article and research titles in Bangla + English get SEO-friendly slugs
- **6 custom email templates** — OTP, magic-login, moderator-invite, appointment-confirmation, contact-confirmation, password-changed
- **Universal search** — searches across articles, research, and testimonials with type filter + result limit
- **Analytics** — page views (fire-and-forget), grouped by route, locations by country/city (via ua-parser-js)
- **Rate limiting** — Redis-backed rate limits for auth, search, and tracking endpoints

---

## 📦 Main Dependencies

### Runtime Dependencies
| Package | Purpose |
|---------|---------|
| `express@^5.2.1` | Web framework |
| `mongoose@^9.6.2` + `mongoose-paginate-v2@^1.9.4` | MongoDB ODM + pagination |
| `ioredis@^5.10.1` + `rate-limit-redis@^4.3.1` | Redis client + Redis-backed rate limits |
| `jsonwebtoken@^9.0.3` | JWT auth (access + refresh rotation) |
| `bcrypt@^6.0.0` | Password hashing (salt 12) |
| `uuid@^14.0.0` | OTP + magic-token generation |
| `nodemailer@^8.0.7` + `@getbrevo/brevo@^5.0.4` | Email (dual providers) |
| `whatsapp-web.js@^1.34.7` + `qrcode-terminal@^0.12.0` | WhatsApp alerts to doctor's phone (QR login in terminal) |
| `node-telegram-bot-api@^0.67.0` | Telegram contact notifications |
| `socket.io@^4.8.3` | Real-time (present, partially wired) |
| `multer@^2.1.1` | File uploads |
| `@imagekit/nodejs@^7.6.1` | Image + PDF storage |
| `cloudinary@^2.10.0` | Video storage (testimonials) |
| `express-validator@^7.3.2` + `zod@^4.4.3` | Request validation |
| `helmet@^8.1.0` + `hpp@^0.2.3` + `lusca@^1.7.0` + `xss@^1.0.15` | Security headers + XSS protection |
| `express-rate-limit@^8.5.2` | Rate limiting |
| `cookie-parser@^1.4.7` + `compression@^1.8.1` + `connect-timeout@^1.9.1` | Production middleware |
| `cors@^2.8.6` | Cross-origin (strict whitelist per frontend) |
| `slugify@^1.6.9` | Bilingual (Bangla + English) slugs |
| `ua-parser-js@^2.0.9` | User-agent parsing for analytics |
| `morgan@^1.10.1` + `chalk@^4.1.2` | HTTP logging |
| `dayjs@^1.11.20` | Date utilities |
| `axios@^1.16.1` | HTTP client (for external APIs) |
| `http-status-codes@^2.3.0` | HTTP status constants |
| `events@^3.3.0` | Event-driven patterns |

### Dev Dependencies (key ones)
| Package | Purpose |
|---------|---------|
| `typescript@^6.0.3` | Type safety |
| `ts-node-dev@^2.0.0` + `tsconfig-paths@^4.2.0` | Dev server with path aliases |
| `eslint@^10.4.0` + `typescript-eslint@^8.59.4` | Linting |
| `@types/*` | Type definitions for all runtime deps |

---

## 🚀 Run Locally

### Prerequisites
- [Node.js](https://nodejs.org/) 18+ (with npm)
- [MongoDB](https://www.mongodb.com/try/download/community) running locally, or MongoDB Atlas
- [Redis](https://redis.io/download/) running locally, or Upstash/Redis Cloud
- ImageKit account (free) — for image + PDF uploads
- Cloudinary account (free) — for video uploads
- Brevo account (free) — for transactional email
- Telegram Bot token (free) — via [@BotFather](https://t.me/BotFather)
- Google reCAPTCHA v3 keys (free)

### Installation

```bash
# 1. Clone
git clone https://github.com/tarekul42/Dr-Shahidur-s-Backend-Server.git
cd Dr-Shahidur-s-Backend-Server

# 2. Install dependencies
npm install

# 3. Configure environment
cp .env.example .env
# Edit .env — see required variables below

# 4. Run dev server (with hot reload via ts-node-dev)
npm run dev
```

Server starts at http://localhost:5000

### Environment Variables (required)

| Variable | Description | Example |
|----------|-------------|---------|
| `PORT` | Server port | `5000` |
| `NODE_ENV` | Environment | `development` |
| `MONGO_URI` | MongoDB connection string | `mongodb://localhost:27017/dr-shahidur` |
| `REDIS_URL` | Redis connection string | `redis://localhost:6379` |
| `JWT_ACCESS_SECRET` | Access token secret (min 32 chars) | `openssl rand -base64 32` |
| `JWT_REFRESH_SECRET` | Refresh token secret | (generate like above) |
| `JWT_ACCESS_EXPIRY` | Access token lifetime | `15m` |
| `JWT_REFRESH_EXPIRY` | Refresh token lifetime | `7d` |
| `CLIENT_PUBLIC_URL` | Public frontend URL (CORS) | `http://localhost:3000` |
| `CLIENT_DASHBOARD_URL` | Admin dashboard URL (CORS) | `http://localhost:3001` |
| `IMAGEKIT_PUBLIC_KEY` | ImageKit public key | (from ImageKit dashboard) |
| `IMAGEKIT_PRIVATE_KEY` | ImageKit private key | (from ImageKit dashboard) |
| `IMAGEKIT_URL_ENDPOINT` | ImageKit URL endpoint | `https://ik.imagekit.io/your_id` |
| `CLOUDINARY_CLOUD_NAME` | Cloudinary cloud name | (from Cloudinary dashboard) |
| `CLOUDINARY_API_KEY` | Cloudinary API key | (from Cloudinary dashboard) |
| `CLOUDINARY_API_SECRET` | Cloudinary API secret | (from Cloudinary dashboard) |
| `SMTP_HOST` | SMTP host | `smtp-relay.brevo.com` |
| `SMTP_PORT` | SMTP port | `587` |
| `SMTP_USER` | SMTP user | `your_email@example.com` |
| `SMTP_PASS` | SMTP password | (Brevo SMTP key) |
| `SMTP_FROM_NAME` | From name | `Dr. Shahidur Rahman Khan` |
| `SMTP_FROM_EMAIL` | From email | `noreply@example.com` |
| `RECAPTCHA_V3_SECRET` | reCAPTCHA v3 secret | (from Google reCAPTCHA admin) |
| `DOCTOR_WHATSAPP_NUMBER` | Doctor's WhatsApp number | `8801XXXXXXXXX` |
| `WHATSAPP_SESSION_PATH` | WhatsApp session storage path | `./whatsapp-session` |
| `TELEGRAM_BOT_TOKEN` | Telegram bot token | (from @BotFather) |
| `TELEGRAM_CHAT_ID` | Telegram channel/chat ID | (from Telegram) |
| `ADMIN_SEED_EMAIL` | Admin seed email | `admin@example.com` |
| `ADMIN_SEED_PASSWORD` | Admin seed password | `your_admin_password` |
| `TEMP_PASS` | Temporary password for seeded users | `your_temp_password` |
| `BREVO_API_KEY` | Brevo API key | (from Brevo dashboard) |

### Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start dev server with hot reload (ts-node-dev) |
| `npm run build` | TypeScript compile |
| `npm start` | Start server (same as dev for now) |
| `npm run type-check` | TypeScript compiler check |
| `npm run lint` | Run ESLint |
| `npm run lint:fix` | Run ESLint with auto-fix |

### WhatsApp Setup

On first server start, a QR code will appear in the terminal. Scan it with the doctor's WhatsApp:
1. Open WhatsApp on the doctor's phone
2. Go to **Settings → Linked Devices → Link a Device**
3. Scan the QR code in the terminal

After that, the session is persistent (stored at `WHATSAPP_SESSION_PATH`). The bot will send contact form submissions and appointment bookings directly to the doctor's WhatsApp.

---

## 👥 Team

This was a **team project with 2 developers**. My contributions to the backend:

* **Full authentication module** — Redis-backed OTP & magic-link authentication, forgot/reset password flow, refresh-token management, and logout invalidation
* **Rate-limiting infrastructure** — global and per-route throttling to protect APIs from abuse and excessive traffic
* **Appointment management system** — complete CRUD operations, status workflows, analytics aggregation, and validation
* **Content management modules** — Articles, Research, and Testimonials with caching, validation, search optimization, and asset lifecycle handling
* **Activity logging & auditing** — centralized middleware across protected routes with admin controls and automated retention policies
* **Email communication system** — 6 custom transactional email templates for authentication, invitations, appointments, and account events
* **Security & DevOps improvements** — CSRF protection, input sanitization, structured logging, GitHub Actions CI/CD, and CodeQL security remediation

---

## 🔗 Links

| Resource | URL |
|----------|-----|
| 🌐 **Live Frontend** | https://drshahidurrahman.vercel.app |
| 💻 **Frontend Repo** | https://github.com/tarekul42/Dr-Shahidur-s-Portfolio-frontend |
| 👨‍⚕️ **Client** | Dr. Md. Shahidur Rahman Khan, Associate Professor, NITOR Dhaka |
| 📧 **Contact** | tarekulrifat142@gmail.com |

---

## 📄 License

ISC © Tarekul Islam Rifat

---

<div align="center">

**⭐ If this project helped you, give it a star!**

Built by [Tarekul Islam Rifat](https://github.com/tarekul42) + team

</div>
