# Task Manager - Full Stack Setup Guide

A complete task management application with React frontend and Node.js/Express backend with MongoDB.

## Architecture Overview

```
Task Manager/
├── server/                 # Backend (Node.js + Express + MongoDB)
│   ├── models/            # Database schemas (User, Task)
│   ├── routes/            # API endpoints (auth, tasks)
│   ├── middleware/        # JWT authentication
│   ├── server.js          # Main server file
│   ├── seed.js            # Database seeding script
│   ├── package.json       # Dependencies
│   └── .env               # Environment variables
│
└── task/                  # Frontend (React)
    ├── src/
    │   ├── components/    # React components
    │   ├── utils/         # API utilities
    │   └── App.js         # Main app
    └── package.json       # Dependencies
```

## Prerequisites

- **Node.js** (v14+) - Download from https://nodejs.org
- **MongoDB** - Either:
  - Local: https://www.mongodb.com/try/download/community
  - Cloud (MongoDB Atlas): https://www.mongodb.com/cloud/atlas (recommended, free tier available)

## Step 1: Setup MongoDB

### Option A: Local MongoDB
1. Download and install MongoDB Community Edition
2. Start MongoDB service (Windows: `mongod` or use MongoDB Compass GUI)
3. Verify connection: `mongodb://localhost:27017/task-manager`

### Option B: MongoDB Atlas (Cloud - Recommended)
1. Go to https://www.mongodb.com/cloud/atlas
2. Create a free account
3. Create a cluster
4. Get connection string: `mongodb+srv://username:password@cluster.mongodb.net/task-manager`
5. Update `.env` in server folder with your connection string

## Step 2: Setup Backend Server

```bash
# Navigate to server directory
cd "D:\CapOasis\IntraNet\Task Manager\server"

# Install dependencies
npm install

# Seed database with demo data
npm run seed
```

The seed script creates:
- **Admin User**: admin@company.com / admin123
- **Employee Users**: john@company.com, employee@company.com, mike@company.com (all with password: emp123)
- **Demo Tasks**: Pre-populated tasks assigned to employees

Start the backend server:

```bash
# Development mode (with auto-reload)
npm run dev

# Or production mode
npm start
```

You should see: `Server is running on port 5000`

## Step 3: Setup Frontend React App

In a new terminal:

```bash
# Navigate to frontend directory
cd "D:\CapOasis\IntraNet\Task Manager\task"

# Install dependencies (if not already done)
npm install

# Start development server
npm start
```

The app opens at `http://localhost:3000`

## Step 4: Test the Application

### Admin Login:
1. Email: `admin@company.com`
2. Password: `admin123`
3. Access to:
   - View all tasks
   - Assign tasks to employees
   - Track employee activity

### Employee Login:
1. Email: `employee@company.com` (or john@company.com, mike@company.com)
2. Password: `emp123`
3. Access to:
   - View assigned tasks
   - See task details (who assigned, when, priority, due date)
   - Update task status

## API Endpoints

### Authentication
```
POST /api/auth/register
Body: { name, email, password, role }
Response: { token, user }

POST /api/auth/login
Body: { email, password }
Response: { token, user }
```

### Tasks (All require Authorization header with JWT token)
```
GET /api/tasks
- Admin: Returns all tasks
- Employee: Returns only their assigned tasks

GET /api/tasks/:id
- Returns single task details

POST /api/tasks (Admin only)
Body: { title, description, priority, assignedTo, dueDate }
Response: Created task

PUT /api/tasks/:id
Body: { status, priority }
- Employees can update status of their tasks
- Admins can update any task field

DELETE /api/tasks/:id (Admin only)
- Delete a task
```

## Troubleshooting

### Backend won't start
- Check if MongoDB is running
- Verify `.env` file has correct MONGODB_URI
- Check if port 5000 is available
- Try: `npm install` again

### Frontend shows "Failed to connect to server"
- Make sure backend is running on http://localhost:5000
- Check if CORS is enabled (it is in server.js)
- Check browser console for errors

### Login fails
- Make sure database is seeded: `npm run seed` in server folder
- Check if backend is running
- Try demo credentials: admin@company.com / admin123

### Tasks not appearing
- Refresh the page
- Check browser console for errors
- Ensure you're logged in
- Check if backend API is responding: Visit http://localhost:5000/api/health

## File Structure

### Backend Files Created
- `server/server.js` - Main Express server
- `server/models/User.js` - User schema with password hashing
- `server/models/Task.js` - Task schema with relationships
- `server/middleware/auth.js` - JWT authentication middleware
- `server/routes/auth.js` - Login/Register endpoints
- `server/routes/tasks.js` - Task CRUD endpoints
- `server/seed.js` - Database seeding script
- `server/.env` - Environment configuration
- `server/package.json` - Dependencies

### Frontend Files Updated
- `task/src/components/Home.jsx` - Integrated with backend
- `task/src/components/Login.jsx` - Real authentication
- `task/src/components/AdminDashboard.jsx` - Uses real tasks
- `task/src/components/EmployeeDashboard.jsx` - Shows assigned tasks
- `task/src/components/TaskAssignModal.jsx` - Creates tasks via API
- `task/src/utils/api.js` - API client functions

## Environment Variables

### Server (.env)
```
MONGODB_URI=mongodb://localhost:27017/task-manager
JWT_SECRET=your_jwt_secret_key_change_in_production
PORT=5000
NODE_ENV=development
```

### Frontend
Uses `http://localhost:5000/api` as base URL (hardcoded in `utils/api.js`)

## Next Steps (Optional Enhancements)

1. **Authentication Persistence**: Store token in localStorage and restore session on refresh
2. **Task Filters**: Filter tasks by status, priority, date range
3. **Search**: Search tasks by title or description
4. **Task Comments**: Add comment/note system for tasks
5. **Real-time Updates**: Use Socket.io for live task updates
6. **Email Notifications**: Send emails when tasks are assigned
7. **User Management**: Add/remove employees from admin panel
8. **Deployment**: Deploy to Heroku (backend) and Netlify/Vercel (frontend)

## Support

For issues:
1. Check browser console (F12)
2. Check server terminal logs
3. Verify MongoDB is running
4. Check API endpoints with Postman
5. Review `server/README.md` for more backend details
