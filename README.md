# Zomato Clone — MERN Stack

A full-stack **Zomato-like** restaurant discovery & food-ordering app built with the **MERN** stack (MongoDB, Express, React, Node). This README gives a clear, copy-pasteable setup, dev commands, environment variable examples, folder layout, API summary, and deployment tips so you can run or fork the project quickly.

---

## Table of contents

1. Project overview
2. Features
3. Tech stack
4. Demo / Screenshots
5. Prerequisites
6. Quick start (local)
7. Environment variables (`.env`) examples
8. Database seed & sample data
9. Scripts (backend & frontend)
10. API endpoints (summary)
11. Project structure
12. Testing / E2E (suggested)
13. Deployment (Heroku / Vercel / Render)
14. Contributing
15. License

---

# Project overview

This project replicates core Zomato features: browse restaurants, search & filter, view menus, user auth, add to cart, place orders (mock), reviews & ratings, and a basic admin panel to add restaurants/menus.

It’s ideal as a learning project or starting point for a production-ready food-delivery app.

---

# Features

* User authentication (JWT) — sign up, login, logout
* Browse restaurants by location / cuisine / cost
* Restaurant detail page (menu, ratings, photos)
* Add-to-cart, checkout flow (mock payments)
* User order history, profile management
* Restaurant admin (CRUD restaurants, menus)
* Search with suggestions & filters
* Responsive React frontend (mobile-first)
* REST API with Express + MongoDB
* Sample data seeding script

---

# Tech stack

* Frontend: React (create-react-app or Vite), React Router, React Context or Redux (optional)
* Backend: Node.js, Express.js
* Database: MongoDB (Atlas or local)
* Auth: JWT (access token + optionally refresh token)
* Styling: Tailwind CSS / CSS Modules / Chakra UI (pick one)
* Optional: Cloudinary for images, Stripe for payments (demo), Socket.IO for live order updates

---

# Demo / Screenshots

*(Replace these with your own screenshots or links)*

* Home / Search
* Restaurant page (menu + reviews)
* Cart / Checkout
* Admin dashboard

---

# Prerequisites

* Node.js v16+ (recommended)
* npm or yarn
* MongoDB Atlas account (or local `mongod`)
* Optional: Cloudinary account (images), Stripe account (payments)

---

# Quick start (local)

Clone the repo:

```bash
git clone https://github.com/<your-user>/zomato-clone.git
cd zomato-clone
```

The repo is organized into two main folders:

* `backend/` — Express API
* `frontend/` — React app

## Backend

```bash
cd backend
# install
npm install
# create .env (example below)
# seed sample data (if provided)
node scripts/seed.js
# run in dev
npm run dev
# or prod
npm start
```

## Frontend

Open a new terminal:

```bash
cd frontend
npm install
# create .env (example below)
npm start
```

After starting backend and frontend, open `http://localhost:3000` (or the port configured) to view the app.

---

# Environment variables

## Backend (`backend/.env`)

```env
PORT=5000
MONGO_URI=mongodb+srv://<user>:<password>@cluster0.mongodb.net/zomato?retryWrites=true&w=majority
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRES_IN=7d
CLOUDINARY_CLOUD_NAME=your_cloud_name    # optional
CLOUDINARY_API_KEY=your_api_key          # optional
CLOUDINARY_API_SECRET=your_api_secret    # optional
STRIPE_SECRET_KEY=sk_test_...            # optional (payments)
NODE_ENV=development
```

## Frontend (`frontend/.env`)

```env
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_MAPS_API_KEY=your_maps_key      # optional, if using maps
REACT_APP_CLOUDINARY_NAME=your_cloud_name
```

**Important:** Never commit `.env` to Git. Use `.env.example` in repo instead.

---

# Database seed & sample data

Include a script at `backend/scripts/seed.js` to insert sample restaurants, menus, and a test user.

Example usage:

```bash
cd backend
node scripts/seed.js             # seeds sample data
node scripts/clearSeed.js        # optional: remove seeded data
```

`seed.js` should:

* Connect to MongoDB
* Create sample restaurants with nested menu items
* Create a test user with hashed password (e.g. `test@example.com` / `password123`)
* Print created IDs and sample JWT for convenience

---

# Scripts

## Backend (`backend/package.json`)

* `npm run dev` — start with nodemon (dev)
* `npm start` — production start
* `npm run seed` — run seed script
* `npm run test` — run tests (if present)

## Frontend (`frontend/package.json`)

* `npm start` — start dev server (port 3000)
* `npm run build` — create production build
* `npm run lint` — linting
* `npm test` — React tests (optional)

---

# API endpoints (summary)

> Base: `/api`

### Auth

* `POST /api/auth/register` — register user (body: name, email, password)
* `POST /api/auth/login` — login (returns JWT)
* `GET /api/auth/me` — get current user (auth required)

### Restaurants

* `GET /api/restaurants` — list restaurants (query: city, cuisine, cost, rating, q (search))
* `GET /api/restaurants/:id` — get restaurant details + menu
* `POST /api/restaurants` — create restaurant (admin)
* `PUT /api/restaurants/:id` — update restaurant (admin)
* `DELETE /api/restaurants/:id` — delete (admin)

### Menu Items

* `POST /api/restaurants/:id/menu` — add menu item (admin)
* `PUT /api/menu/:menuItemId` — update menu item (admin)
* `DELETE /api/menu/:menuItemId` — delete menu item (admin)

### Orders

* `POST /api/orders` — create order (user)
* `GET /api/orders` — user order history
* `GET /api/orders/:id` — order details

### Reviews

* `POST /api/restaurants/:id/review` — add review (user)

### Misc

* `GET /api/search` — search suggestions
* `GET /api/config` — return config like stripe public key (optional)

> Protect routes using JWT middleware. Admin routes verify `user.role === 'admin'`.

---

# Project structure (example)

```
/backend
  /controllers
  /models
  /routes
  /middleware
  /scripts
  server.js
  config.js

/frontend
  /public
  /src
    /components
    /pages
    /contexts
    /hooks
    /services (api client)
    /styles
    App.jsx
    index.jsx
```

---

# Frontend tips

* Use a single `apiClient` (axios) configured with baseURL and interceptors to attach JWT.
* Store JWT in `httpOnly` cookie (recommended) or localStorage (simple approach).
* Use optimistic UI updates for cart interactions.
* Use lazy loading for images and code splitting routes for performance.

---

# Deployment

## Backend (Heroku / Render)

* Set `MONGO_URI` env var in dashboard
* Set `JWT_SECRET`
* Ensure `PORT` is set by host
* If using `client` build in same repo, serve `frontend/build` as static files from Express for a single-app deploy.

## Frontend (Vercel / Netlify)

* Point `REACT_APP_API_URL` to the deployed backend
* Build command: `npm run build`, publish `build/` folder
* Use environment variables in Vercel/Netlify settings

---

# Security & production notes

* Use HTTPS in production.
* Use strong `JWT_SECRET` and short access token lifetimes + refresh tokens if needed.
* Sanitize user input and validate with `express-validator` or Joi.
* Rate-limit auth endpoints (express-rate-limit).
* Store sensitive secrets in the host's env, never in repo.

---

# Testing & E2E

* Unit test backend controllers with Jest + Supertest.
* Use Cypress or Playwright for E2E tests (user sign up → order → admin acceptance flow).

---

# Contributing

1. Fork the repo
2. Create a feature branch `feature/<name>`
3. Commit changes, write clear commit messages
4. Open a PR describing changes, link issues
5. Add unit tests for new features

---

# Helpful commands (copy/paste)

```bash
# from repo root
cd backend && npm install && npm run dev
cd frontend && npm install && npm start
```

---

# Example `.gitignore` (important)

```
/node_modules
/.env
/frontend/node_modules
/backend/node_modules
/build
.DS_Store
```

---

# License

MIT License — feel free to use and modify. Add your name and year in LICENSE file.
