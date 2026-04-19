# TaskFlow — Full-Stack Project Management App

> React + Express + MongoDB + JWT Auth

---

## Tech Stack

| Layer     | Tech                                      |
|-----------|-------------------------------------------|
| Frontend  | React 18, React Router v6, TanStack Query, Recharts |
| Backend   | Node.js, Express 4, Mongoose              |
| Database  | MongoDB                                   |
| Auth      | JWT (Bearer tokens)                       |
| Fonts     | Syne (display) + DM Sans (body)           |

---

## Project Structure

```
taskflow/
├── backend/
│   ├── src/
│   │   ├── config/         # MongoDB connection
│   │   ├── controllers/    # authController, projectController, taskController, statsController
│   │   ├── middleware/     # JWT protect, adminOnly
│   │   ├── models/         # User, Project, Task (Mongoose schemas)
│   │   ├── routes/         # auth, projects, tasks, stats
│   │   └── index.js        # Express entry point
│   ├── .env.example
│   └── package.json
│
└── frontend/
    ├── src/
    │   ├── api/            # Axios client + service functions
    │   ├── components/     # Layout (sidebar), Modal
    │   ├── context/        # AuthContext (JWT + user state)
    │   ├── pages/          # Dashboard, Projects, ProjectDetail, Tasks, Login, Register
    │   ├── App.jsx         # Router with private/public routes
    │   └── index.js        # Entry + QueryClient setup
    └── package.json
```

---

## Quick Start

### 1. Prerequisites

- Node.js >= 18
- MongoDB running locally (`mongod`) **or** a MongoDB Atlas connection string

---

### 2. Backend Setup

```bash
cd backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env
# Edit .env — set MONGODB_URI and JWT_SECRET

# Start dev server (with nodemon)
npm run dev

# API will be available at http://localhost:5000
```

**`.env` values:**

| Key           | Example                                      |
|---------------|----------------------------------------------|
| `PORT`        | `5000`                                       |
| `MONGODB_URI` | `mongodb://localhost:27017/taskflow`         |
| `JWT_SECRET`  | Any long random string                       |
| `JWT_EXPIRE`  | `7d`                                         |
| `NODE_ENV`    | `development`                                |

---

### 3. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start dev server
npm start

# App will open at http://localhost:3000
# Requests to /api/* are proxied to http://localhost:5000
```

---

## API Reference

### Auth

| Method | Endpoint           | Auth | Description         |
|--------|--------------------|------|---------------------|
| POST   | `/api/auth/register` | —  | Register new user   |
| POST   | `/api/auth/login`    | —  | Login, get JWT      |
| GET    | `/api/auth/me`       | ✓  | Get current user    |
| PUT    | `/api/auth/me`       | ✓  | Update profile      |

**Register / Login body:**
```json
{ "name": "Jane", "email": "jane@example.com", "password": "secret123" }
```

---

### Projects

| Method | Endpoint            | Auth | Description            |
|--------|---------------------|------|------------------------|
| GET    | `/api/projects`     | ✓   | List user's projects   |
| POST   | `/api/projects`     | ✓   | Create project         |
| GET    | `/api/projects/:id` | ✓   | Get single project     |
| PUT    | `/api/projects/:id` | ✓   | Update project         |
| DELETE | `/api/projects/:id` | ✓   | Delete project + tasks |

**Create Project body:**
```json
{
  "name": "Website Redesign",
  "description": "Full rebrand",
  "color": "#7c6af7",
  "status": "active",
  "dueDate": "2025-06-30"
}
```

---

### Tasks

| Method | Endpoint         | Auth | Description                     |
|--------|------------------|------|---------------------------------|
| GET    | `/api/tasks`     | ✓   | List tasks (filter by query params) |
| POST   | `/api/tasks`     | ✓   | Create task                     |
| GET    | `/api/tasks/:id` | ✓   | Get single task                 |
| PUT    | `/api/tasks/:id` | ✓   | Update task                     |
| DELETE | `/api/tasks/:id` | ✓   | Delete task                     |

**Query params for GET /api/tasks:**
- `project=<id>`
- `status=todo|in_progress|review|done`
- `priority=low|medium|high|urgent`
- `assignee=<userId>`

**Create Task body:**
```json
{
  "title": "Design homepage",
  "description": "Figma mockup first",
  "project": "<projectId>",
  "status": "todo",
  "priority": "high",
  "dueDate": "2025-05-15"
}
```

---

### Stats

| Method | Endpoint              | Auth | Description               |
|--------|-----------------------|------|---------------------------|
| GET    | `/api/stats/overview` | ✓   | Dashboard analytics data  |

**Response includes:**
- `totalProjects`, `totalTasks`, `overdueCount`
- `tasksByStatus` — count per status
- `tasksByPriority` — count per priority
- `completedByDay` — last 7 days completion trend
- `projectStats` — per-project progress %

---

## Frontend Pages

| Route              | Page              | Description                            |
|--------------------|-------------------|----------------------------------------|
| `/login`           | Login             | JWT login form                         |
| `/register`        | Register          | Create account                         |
| `/`                | Dashboard         | Stats cards + 3 charts + project bars  |
| `/projects`        | Projects          | Grid of projects, create/delete        |
| `/projects/:id`    | Project Detail    | Project info + embedded task list      |
| `/tasks`           | All Tasks         | Full task list with filters, CRUD      |

---

## Features

- **JWT Authentication** — Register, login, protected routes, token stored in localStorage, auto-refresh on page load
- **Projects CRUD** — Create with custom color, track status (active / completed / archived), delete cascades to tasks
- **Tasks CRUD** — Full create/edit/delete, status toggle (click circle), inline status dropdown, priority badges, due date with overdue highlight
- **Dashboard** — Live stats cards, line chart (7-day completions), pie chart (by status), bar chart (by priority), project progress bars
- **Filters** — Filter tasks by status and priority
- **Collapsible sidebar** — Icon-only mode, responsive on mobile
- **Dark theme** — CSS variables throughout, easy to retheme

---

## Extending the App

**Add user assignment to tasks:**
```js
// Already in Task model — just populate assignee in queries
// Add a user search endpoint to assign teammates
```

**Add comments to tasks:**
```js
// Add Comment model: { task, author, body, createdAt }
// POST /api/tasks/:id/comments
```

**Add file attachments:**
```js
// Use multer for uploads, store paths in Task.attachments[]
```

**Deploy:**
- Backend → Railway, Render, or Fly.io
- Frontend → Vercel or Netlify (set `REACT_APP_API_URL`)
- Database → MongoDB Atlas (free tier)

---

## License

MIT — use freely, build something great.
