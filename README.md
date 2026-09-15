# 🔗 URL Shortener

A full-stack URL shortening service built with **React** and **Express**. Shorten long URLs into compact, shareable links - with optional user accounts to manage, track, and customize your shortened URLs.

---

## ✨ Features

- **Shorten URLs** - Generate short links from any valid URL instantly
- **Custom Slugs** - Authenticated users can create custom short URL aliases
- **Click Tracking** - Track how many times each shortened URL has been visited
- **User Dashboard** - View and manage all your shortened URLs in one place
- **Authentication** - Secure JWT-based auth with HTTP-only cookies
- **Guest Access** - Shorten URLs without creating an account

---

## 🛠 Tech Stack

### Frontend

| Technology | Purpose |
|---|---|
| [React 19](https://react.dev) | UI framework |
| [Vite](https://vite.dev) | Build tool & dev server |
| [TanStack Router](https://tanstack.com/router) | File-based routing |
| [TanStack React Query](https://tanstack.com/query) | Server state management |
| [Redux Toolkit](https://redux-toolkit.js.org) | Client state management |
| [Tailwind CSS v4](https://tailwindcss.com) | Utility-first styling |
| [Axios](https://axios-http.com) | HTTP client |

### Backend

| Technology | Purpose |
|---|---|
| [Express 5](https://expressjs.com) | Web framework |
| [MongoDB](https://www.mongodb.com) + [Mongoose](https://mongoosejs.com) | Database & ODM |
| [JSON Web Tokens](https://jwt.io) | Authentication |
| [bcrypt.js](https://github.com/dcodeIO/bcrypt.js) | Password hashing |
| [nanoid](https://github.com/ai/nanoid) | Short ID generation |

---

## 📁 Project Structure

```
URL_SHORTNER/
├── BACKEND/
│   ├── app.js                  # Express entry point
│   ├── .env                    # Environment variables
│   ├── package.json
│   └── src/
│       ├── config/             # DB connection & app config
│       ├── controller/         # Route handlers
│       │   ├── auth.controller.js
│       │   ├── short_url.controller.js
│       │   └── user.controller.js
│       ├── dao/                # Data access layer
│       ├── middleware/         # Auth middleware (JWT verification)
│       ├── models/             # Mongoose schemas
│       │   ├── short_url.model.js
│       │   └── user.model.js
│       ├── routes/             # API route definitions
│       │   ├── auth.routes.js
│       │   ├── short_url.route.js
│       │   └── user.routes.js
│       ├── services/           # Business logic
│       └── utils/              # Error handler, try-catch wrapper
│
└── FRONTEND/
    ├── index.html
    ├── vite.config.js
    ├── package.json
    └── src/
        ├── main.jsx            # App entry point
        ├── RootLayout.jsx      # Root layout component
        ├── api/                # Axios API calls
        ├── components/         # Reusable UI components
        │   ├── LoginForm.jsx
        │   ├── RegisterForm.jsx
        │   ├── NavBar.jsx
        │   ├── UrlForm.jsx
        │   └── UserUrl.jsx
        ├── pages/              # Page-level components
        │   ├── AuthPage.jsx
        │   ├── DashboardPage.jsx
        │   └── HomePage.jsx
        ├── routing/            # TanStack Router route tree
        ├── store/              # Redux store configuration
        └── utils/              # Shared utilities
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 18
- **MongoDB** running locally (or a remote connection string)

### 1. Clone the Repository

```bash
git clone https://github.com/SAMEER-2507/URL_SHORTNER.git
cd URL_SHORTNER
```

### 2. Setup the Backend

```bash
cd BACKEND
npm install
```

Create a `.env` file (or update the existing one) in the `BACKEND/` directory:

```env
MONGO_URI=mongodb://localhost:27017/url-shortner
APP_URL=http://localhost:3000/
JWT_SECRET=your_secret_key_here
```

Start the backend server:

```bash
# Development (with hot reload)
npm run dev

# Production
npm start
```

The API server will start on **http://localhost:3000**.

### 3. Setup the Frontend

```bash
cd FRONTEND
npm install
npm run dev
```

The frontend dev server will start on **http://localhost:5173**.

---

## 📡 API Reference

### Authentication

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `POST` | `/api/auth/register` | Register a new user | ✗ |
| `POST` | `/api/auth/login` | Login & receive JWT cookie | ✗ |
| `POST` | `/api/auth/logout` | Clear auth cookie | ✗ |
| `GET` | `/api/auth/me` | Get current user profile | ✔ |

### URL Shortening

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `POST` | `/api/create` | Create a shortened URL | Optional |
| `GET` | `/:id` | Redirect to the original URL | ✗ |

### User

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `POST` | `/api/user/urls` | Get all URLs for the logged-in user | ✔ |

### Request / Response Examples

**Create Short URL** (guest)
```bash
curl -X POST http://localhost:3000/api/create \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/very/long/path"}'
```
```json
{ "shortUrl": "http://localhost:3000/aBcDeFg" }
```

**Create Short URL** (authenticated, custom slug)
```bash
curl -X POST http://localhost:3000/api/create \
  -H "Content-Type: application/json" \
  --cookie "accessToken=<your_jwt>" \
  -d '{"url": "https://example.com/very/long/path", "slug": "my-link"}'
```
```json
{ "shortUrl": "http://localhost:3000/my-link" }
```

---

## 🗄 Database Models

### User

| Field | Type | Description |
|---|---|---|
| `name` | String | User's display name |
| `email` | String | Unique email address |
| `password` | String | Hashed password (excluded from queries by default) |
| `avatar` | String | Gravatar URL (default provided) |

### ShortUrl

| Field | Type | Description |
|---|---|---|
| `full_url` | String | The original long URL |
| `short_url` | String | The generated/custom short slug |
| `clicks` | Number | Redirect count (default: `0`) |
| `user` | ObjectId | Reference to the User who created it (optional) |

---

## 📜 Available Scripts

### Backend (`BACKEND/`)

| Script | Command | Description |
|---|---|---|
| `dev` | `npm run dev` | Start with nodemon (hot reload) |
| `start` | `npm start` | Start with node |

### Frontend (`FRONTEND/`)

| Script | Command | Description |
|---|---|---|
| `dev` | `npm run dev` | Start Vite dev server |
| `build` | `npm run build` | Build for production |
| `preview` | `npm run preview` | Preview production build |
| `lint` | `npm run lint` | Run ESLint |

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the [ISC License](https://opensource.org/licenses/ISC).
