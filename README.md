# TaskFlow — Task Management System

A full-stack task management web application built with **React.js**, **Node.js**, **Express.js**, **MySQL**, and **JWT** authentication.

## Features

- **JWT Authentication** — Secure login/register with token-based auth and role-based access control
- **Role-Based Access** — Admin, Manager, and Member roles with different permissions
- **Task CRUD** — Create, read, update, delete tasks with filters and pagination
- **Kanban Board** — Drag-and-drop board to manage task status visually
- **Categories** — Organize tasks into color-coded categories
- **Team Management** — Manage users, roles, and activation status
- **MVC Architecture** — Clean separation of controllers, routes, and models
- **Optimized Queries** — Composite MySQL indexes for fast data retrieval

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React.js 18, React Router v6, Axios |
| Backend | Node.js, Express.js |
| Database | MySQL 8 |
| Auth | JWT (jsonwebtoken), bcryptjs |
| Validation | express-validator |
| Security | Helmet, CORS |

## Project Structure

```
task-management-system/
├── backend/
│   ├── config/
│   │   ├── db.js           # MySQL connection pool
│   │   └── schema.sql      # Database schema + seed data
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── taskController.js
│   │   ├── categoryController.js
│   │   └── userController.js
│   ├── middleware/
│   │   └── auth.js         # JWT authenticate + authorize middleware
│   ├── routes/
│   │   ├── auth.js
│   │   ├── tasks.js
│   │   ├── categories.js
│   │   └── users.js
│   ├── .env.example
│   └── server.js
└── frontend/
    └── src/
        ├── components/
        │   ├── Sidebar.jsx
        │   └── TaskModal.jsx
        ├── context/
        │   └── AuthContext.js
        ├── pages/
        │   ├── AuthPage.jsx
        │   ├── Dashboard.jsx
        │   ├── Tasks.jsx
        │   ├── Board.jsx
        │   ├── Categories.jsx
        │   └── Users.jsx
        └── utils/
            └── api.js       # Axios instance + API helpers
```

## Setup

### Prerequisites
- Node.js 18+
- MySQL 8+

### 1. Database

```bash
mysql -u root -p < backend/config/schema.sql
```

### 2. Backend

```bash
cd backend
cp .env.example .env
# Edit .env with your DB credentials and a strong JWT_SECRET
npm install
npm run dev
```

Server runs at `http://localhost:5000`

### 3. Frontend

```bash
cd frontend
npm install
npm start
```

App runs at `http://localhost:3000`

## API Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register new user |
| POST | `/api/auth/login` | Login, returns JWT |
| GET | `/api/auth/me` | Get current user (auth required) |

### Tasks
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tasks` | List tasks (filters: status, priority, search, page) |
| GET | `/api/tasks/stats` | Dashboard statistics |
| GET | `/api/tasks/:id` | Get single task with comments |
| POST | `/api/tasks` | Create task |
| PUT | `/api/tasks/:id` | Update task |
| DELETE | `/api/tasks/:id` | Delete task |

### Categories
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/categories` | List all categories |
| POST | `/api/categories` | Create category (manager+) |
| PUT | `/api/categories/:id` | Update category (manager+) |
| DELETE | `/api/categories/:id` | Delete category (admin) |

### Users
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/users` | List users (manager+) |
| PUT | `/api/users/:id` | Update user profile |
| PUT | `/api/users/:id/password` | Change password |

## Demo Credentials

```
Email:    admin@taskmanager.com
Password: Admin@123
```

## Database Schema

```
users ────────────┐
                  ├─── tasks ──── task_comments
categories ───────┘
```

Key optimizations:
- Composite index on `(status, priority)` for filtered queries
- Separate indexes on `assigned_to`, `created_by`, `due_date`, `category_id`
- Connection pooling (10 connections) for concurrent requests
