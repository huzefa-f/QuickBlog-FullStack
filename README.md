# QuickBlog — Full-Stack Blogging Platform

QuickBlog is a full-stack blog application with an admin dashboard, AI-assisted content generation, image uploads, and a public-facing blog with comments. Built with React (Vite) on the frontend and Express + MongoDB on the backend.

**Live demo:** [quick-blog-full-stack-b1ug.vercel.app](https://quick-blog-full-stack-b1ug.vercel.app/)

---

## Features

- 📝 Create, edit, publish/unpublish, and delete blog posts
- 🤖 AI-powered blog content generation (Google Gemini)
- 🖼️ Image uploads with ImageKit
- 💬 Public comments on blog posts with admin approval workflow
- 🔐 JWT-based admin authentication
- 📊 Admin dashboard with blog and comment stats
- 📱 Responsive UI built with React + Tailwind CSS

---

## Tech Stack

**Frontend**
- React 19 + Vite
- React Router
- Tailwind CSS
- Axios
- Quill (rich text editor)
- React Hot Toast

**Backend**
- Node.js + Express 5
- MongoDB + Mongoose
- JSON Web Tokens (JWT) for auth
- Multer (file upload handling)
- ImageKit (image storage/CDN)
- Google Gemini API (AI content generation)

**Deployment**
- Vercel (client and server deployed as separate projects)

---

## Project Structure

```
QuickBlog-FullStack/
├── client/                 # React frontend (Vite)
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/          # Route-level pages (incl. admin pages)
│   │   ├── context/        # Global app state (AppContext)
│   │   └── assets/
│   └── vercel.json
│
└── server/                 # Express backend
    ├── configs/            # DB, ImageKit, Gemini configs
    ├── controllers/        # Route handler logic
    ├── middleware/         # Auth + Multer middleware
    ├── models/             # Mongoose schemas (Blog, Comment)
    ├── routes/              # Express routers
    ├── server.js
    └── vercel.json
```

---

## Getting Started (Local Development)

### Prerequisites
- Node.js (v18+)
- A MongoDB database (local or Atlas)
- ImageKit account (for image uploads)
- Google Gemini API key (for AI content generation)

### 1. Clone the repository
```bash
git clone <your-repo-url>
cd QuickBlog-FullStack
```

### 2. Backend setup
```bash
cd server
npm install
```

Create a `.env` file in `server/` with:
```env
PORT=3000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
ADMIN_EMAIL=your_admin_email
ADMIN_PASSWORD=your_admin_password
IMAGEKIT_PUBLIC_KEY=your_imagekit_public_key
IMAGEKIT_PRIVATE_KEY=your_imagekit_private_key
IMAGEKIT_URL_ENDPOINT=your_imagekit_url_endpoint
GEMINI_API_KEY=your_gemini_api_key
```

Run the server:
```bash
npm run server   # with nodemon (dev)
# or
npm start        # plain node
```

### 3. Frontend setup
```bash
cd ../client
npm install
```

Create a `.env` file in `client/` with:
```env
VITE_BASE_URL=http://localhost:3000
```

Run the frontend:
```bash
npm run dev
```

The app should now be running locally, with the client on Vite's default port (usually `http://localhost:5173`) and the API on `http://localhost:3000`.

---

## Deployment (Vercel)

This project is deployed as **two separate Vercel projects** — one for `client`, one for `server` — since the frontend is a static Vite build and the backend runs as a serverless Express app.

### Backend
1. Import the repo into Vercel, setting **Root Directory** to `server`.
2. Add the same environment variables listed above (`MONGODB_URI`, `JWT_SECRET`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`, ImageKit keys, `GEMINI_API_KEY`) under Project Settings → Environment Variables.
3. Deploy. Note the resulting URL (e.g. `https://your-server.vercel.app`).

### Frontend
1. Import the repo into Vercel, setting **Root Directory** to `client`.
2. Add an environment variable:
   ```
   VITE_BASE_URL=https://your-server.vercel.app
   ```
3. Deploy (or redeploy after adding the env variable — Vite bakes env vars in at build time, so a fresh build is required whenever this changes).

---

## API Overview

### Admin Routes (`/api/admin`)
| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/login` | Admin login, returns JWT | ❌ |
| GET | `/comments` | Get all comments | ✅ |
| GET | `/blogs` | Get all blogs (admin view) | ✅ |
| POST | `/delete-comment` | Delete a comment | ✅ |
| POST | `/approve-comment` | Approve a comment | ✅ |
| GET | `/dashboard` | Dashboard stats | ✅ |

### Blog Routes (`/api/blog`)
| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/add` | Create a new blog (with image upload) | ✅ |
| GET | `/all` | Get all published blogs | ❌ |
| GET | `/:blogId` | Get a single blog by ID | ❌ |
| POST | `/delete` | Delete a blog | ✅ |
| POST | `/toggle-publish` | Toggle publish status | ✅ |
| POST | `/add-comment` | Add a comment to a blog | ❌ |
| POST | `/comments` | Get comments for a blog | ❌ |
| POST | `/generate` | Generate blog content via AI | ✅ |

✅ = requires `Authorization` header with a valid admin JWT.

---

## License

This project is for personal/educational use. Add a license of your choice if distributing publicly.
