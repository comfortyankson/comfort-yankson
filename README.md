# COMFORT COLLECTIONS E-Commerce Website

This is a full-stack e-commerce platform built for a premium bathroom towel, bed sheet, and door curtains collection based in Dansoman, Ghana.

## Tech Stack
- Frontend: React (Vite) with TypeScript
- Backend: Node.js, Express, TypeScript
- Database: PostgreSQL (via Prisma ORM)
- Authentication: JWT-based

## Project Structure
- `/frontend` - React application (customer & admin dashboard)
- `/backend` - Express REST API and Prisma database models

## Setup Instructions

### 1. Backend Setup
1. Open a terminal and navigate to the `backend` folder:
   ```bash
   cd backend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Copy the `.env.example` file to `.env` and fill in your database credentials:
   ```bash
   cp .env.example .env
   ```
   *Make sure you have a PostgreSQL database running and update `DATABASE_URL`.*
   *If you use Supabase, connect through the **Session pooler** on port 5432. The
   direct `db.<ref>.supabase.co` host is IPv6-only and will not resolve for IPv4
   clients. URL-encode any special characters in your password.*
4. Run Prisma migrations to set up the database schema:
   ```bash
   npx prisma migrate dev --name init
   ```
5. Seed the database with sample products:
   ```bash
   npx prisma db seed
   ```
6. Create your admin account. Credentials are supplied at runtime and never
   committed to this repository:
   ```bash
   npm run create-admin -- you@example.com "your-strong-password"
   ```
   *Passwords must be at least 12 characters. To add a second admin, set
   `ALLOW_ADDITIONAL_ADMINS=true`. There is deliberately no HTTP endpoint for
   creating admins — an unauthenticated route is a permanent backdoor once
   deployed, and a bootstrap script has no network surface at all.*
7. Start the development server:
   ```bash
   npm run dev
   ```
   *The `dev` and `create-admin` scripts are already defined in `package.json`.*

### 2. Frontend Setup
1. Open a new terminal and navigate to the `frontend` folder:
   ```bash
   cd frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```

## Features Implemented So Far
- **Prisma Database Models:** Customer, Admin, Product, Review, Order, OrderItem, WishlistItem.
- **Backend Authentication:** JWT protection for admin and customer routes.
- **REST APIs:** Full CRUD operations for Products, Orders, Reviews, and Customers.
- **Seed Script:** Populates initial products only. Admin accounts are created
  exclusively via `npm run create-admin`.
- **Security:**
  - No endpoint for creating admins; they are created by CLI script only.
  - `JWT_SECRET` (32+ chars) and `FRONTEND_URL` are required in production.
    The server refuses to start without them rather than running insecure.
  - CORS is restricted to `FRONTEND_URL` instead of allowing every origin.
  - Rate limiting on admin login, customer login/registration, and order
    creation, plus a general API ceiling.
  - Order totals are computed server-side from database prices; the price
    submitted by the client is ignored. Stock is decremented transactionally on
    create and returned exactly once if the order is cancelled. A cancelled
    order cannot be reopened, since its stock has already been released.
  - `.env` is gitignored.
- **Project Structure:** Fully scaffolded React and Express environments.

## Next Steps
The customer UI and admin dashboard are scaffolded and rendering. Remaining work:
1. Customer authentication pages and Wishlist.
2. Implementing the WhatsApp Order confirmation flow.
3. Adding tests. There are none, so the security work above is verified only
   by manual probing against a live database.
4. Deploying the API to a host that can run Express + Prisma (Netlify serves
   the frontend only), then wiring the two together via environment variables.
