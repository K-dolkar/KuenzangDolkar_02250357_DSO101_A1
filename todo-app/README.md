# Todo App - DSO101 Assignment 1

A full-stack todo application with a Node.js/Express backend and a static HTML/CSS/JavaScript frontend, containerized with Docker and deployed on Render.

## Project Structure

```
todo-app/
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   ├── server.js
│   └── .env
├── frontend/
│   ├── Dockerfile
│   ├── index.html
│   ├── style.css
│   └── package.json
└── render.yaml
```

## Features

- **Add Tasks**: Create new todo items
- **View Tasks**: Display all tasks in real-time
- **Edit Tasks**: Modify existing task text
- **Delete Tasks**: Remove tasks from the list
- **Responsive Design**: Works on desktop and mobile devices
- **Real-time Sync**: Auto-refreshes every 5 seconds

## Tech Stack

### Backend
- **Node.js** with Express.js
- **CORS** enabled for cross-origin requests
- **dotenv** for environment variables
- In-memory task storage

### Frontend
- **HTML5** for structure
- **CSS3** with gradients and animations
- **Vanilla JavaScript** (no dependencies)
- **Fetch API** for HTTP requests

### Deployment
- **Docker** for containerization
- **Render** for hosting (static frontend + backend service)
- **Nginx** for serving static frontend files

## Local Development

### Prerequisites
- Node.js (v14+)
- npm
- Docker (optional, for containerized deployment)

### Setup

1. **Install Backend Dependencies**
   ```bash
   cd backend
   npm install
   ```

2. **Install Frontend Dependencies**
   ```bash
   cd ../frontend
   npm install
   ```

3. **Start Backend** (from backend folder)
   ```bash
   npm start
   ```
   Backend runs on `http://localhost:5000`

4. **Start Frontend** (from frontend folder)
   ```bash
   npm run dev
   ```
   Frontend runs on `http://localhost:3000`

5. **Access the App**
   Open http://localhost:3000 in your browser

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Health check |
| GET | `/tasks` | Fetch all tasks |
| POST | `/tasks` | Create a new task |
| PUT | `/tasks/:id` | Update a task |
| DELETE | `/tasks/:id` | Delete a task |

### Request/Response Format

**POST /tasks** - Create a task
```json
Request Body:
{
  "text": "Buy groceries"
}

Response:
{
  "id": 1234567890,
  "text": "Buy groceries"
}
```

**PUT /tasks/:id** - Update a task
```json
Request Body:
{
  "text": "Updated task text"
}

Response:
{
  "message": "Task updated"
}
```

**DELETE /tasks/:id** - Delete a task
```json
Response:
{
  "message": "Task deleted"
}
```

## Docker Build & Deployment

### Build Docker Images

**Frontend**
```bash
cd frontend
docker build -t kuenzangdolkar/fe-todo:02250357 .
docker push kuenzangdolkar/fe-todo:02250357
```

**Backend**
```bash
cd backend
docker build -t kuenzangdolkar/be-todo:02250357 .
docker push kuenzangdolkar/be-todo:02250357
```

### Deploy on Render

1. Connect your GitHub repository to Render
2. Create two Web Services:
   - **Frontend Service**: Use frontend Docker image
   - **Backend Service**: Use backend Docker image
3. Update the frontend's API_URL with the backend service URL
4. Deploy both services

## File Descriptions

### Backend Files

**server.js**
- Express server configuration
- CORS setup for frontend communication
- Task CRUD operations
- Logging middleware for debugging
- Runs on port 5000 (or PORT env variable)

**Dockerfile**
- Uses Node.js alpine image for lightweight deployment
- Installs dependencies
- Exposes port 5000
- Starts the server

### Frontend Files

**index.html**
- Single-page application structure
- Task input form
- Edit task interface
- Task list display
- JavaScript fetch calls for API communication

**style.css**
- Modern gradient background
- Responsive flexbox layout
- Smooth animations
- Mobile-friendly design
- Color scheme: Purple/blue gradient

**Dockerfile**
- Uses Nginx alpine image
- Serves static files
- Exposes port 80
- Nginx runs in foreground

## Environment Variables

### Backend (.env file)
```
PORT=5000
```

## Troubleshooting

**"Failed to add task" error**
- Check if backend is running on localhost:5000
- Open Developer Console (F12) to see error messages
- Verify CORS is enabled on backend
- Check browser console for network errors

**Frontend shows blank page**
- Ensure index.html and style.css are not empty
- Check if frontend server is running on correct port
- Clear browser cache (Ctrl+Shift+Delete)

**Docker build fails**
- Ensure you're in the correct directory (frontend or backend)
- Verify Dockerfile exists and is not empty
- Check Docker daemon is running

## Future Enhancements

- User authentication and accounts
- Task categories/projects
- Due dates and reminders
- Task priority levels
- Local storage for offline functionality
- Database integration (MongoDB/PostgreSQL)
- Task filtering and sorting

## Notes

- Tasks are stored in memory (not persistent across server restarts)
- No user authentication required
- Frontend auto-refreshes tasks every 5 seconds
- All endpoints accept/return JSON
