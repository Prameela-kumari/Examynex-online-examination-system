# ExamyNex - Complete Online Examination System

A modern, feature-rich online examination platform with AI-powered proctoring, real-time monitoring, and comprehensive exam management capabilities.

## 🚀 Features

### For Students
- **Secure Authentication** - JWT-based login with role management
- **Exam Taking Interface** - Clean, distraction-free exam environment
- **AI Proctoring** - Webcam-based monitoring with face detection
- **Real-time Auto-save** - Answers saved automatically as you type
- **Profile Management** - View stats, manage settings, track activity
- **Exam Results** - Detailed score breakdown and performance analytics

### For Administrators
- **Exam Creation** - Create exams with customizable duration and descriptions
- **Question Management** - Add MCQ, text, and code questions
- **Live Monitoring** - Real-time view of active exam sessions via WebSocket
- **Submissions Review** - View and analyze all student submissions
- **Proctoring Oversight** - Monitor violations and student behavior
- **Analytics Dashboard** - Comprehensive statistics and insights

### Technical Features
- **Glassmorphism UI** - Modern, gradient-based design
- **Responsive Design** - Works on desktop, tablet, and mobile
- **WebSocket Integration** - Real-time updates for admins
- **Face Detection** - OpenCV-based proctoring system
- **Auto-grading** - Instant MCQ scoring
- **RESTful API** - Clean, documented endpoints

## 📋 Prerequisites

### Backend Requirements
- Python 3.8+
- SQLite (included with Python)
- Virtual environment (recommended)

### Frontend Requirements
- Modern web browser (Chrome, Firefox, Edge, Safari)
- Live Server extension for VS Code (or any HTTP server)

## 🛠️ Installation & Setup

### 1. Clone the Repository
```bash
cd examynex
```

### 2. Backend Setup

#### On Windows:
```bash
cd backend
python -m venv #
#\Scripts\activate
pip install -r requirments.txt
```

#### On Mac/Linux:
```bash
cd backend
python3 -m venv #
source #/bin/activate
pip install -r requirments.txt
```

### 3. Database Initialization
The database will be created automatically on first run. To reset the database:

**Windows:**
```bash
reset_db.bat
```

**Mac/Linux:**
```bash
chmod +x reset_db.sh
./reset_db.sh
```

### 4. Start the Backend Server
```bash
cd backend
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

The backend will be available at `http://localhost:8000`

### 5. Frontend Setup

#### Option A: Using VS Code Live Server (Recommended)
1. Open the `frontend` folder in VS Code
2. Install "Live Server" extension
3. Right-click on `login.html`
4. Select "Open with Live Server"

The frontend will open at `http://127.0.0.1:5500` or `http://localhost:5500`

#### Option B: Using Python HTTP Server
```bash
cd frontend
python -m http.server 8080
```

Access at `http://localhost:8080/login.html`

## 📁 Project Structure

```
examynex/
├── backend/
│   ├── app/
│   │   ├── main.py              # FastAPI application entry
│   │   ├── database.py          # Database configuration
│   │   ├── models.py            # SQLAlchemy models
│   │   ├── models_proctor.py   # Proctoring models
│   │   ├── schemas.py           # Pydantic schemas
│   │   ├── auth.py              # JWT authentication
│   │   ├── dependencies.py      # Auth dependencies
│   │   ├── proctor.py           # Proctoring logic
│   │   └── routes/
│   │       ├── user.py          # User & auth endpoints
│   │       ├── exam.py          # Exam management
│   │       ├── question.py      # Question management
│   │       ├── submission.py    # Submission handling
│   │       └── proctor.py       # Proctoring endpoints
│   ├── requirments.txt          # Python dependencies
│   └── exam.db                  # SQLite database (auto-created)
│
└── frontend/
    ├── config.js                # API configuration
    ├── styles.css               # Global styles
    ├── login.html               # Login page
    ├── register.html            # Registration page
    ├── dashboard.html           # Role-based redirect
    ├── student-dashboard.html   # Student home
    ├── student-profile.html     # Student profile & settings
    ├── exam-taking.html         # Exam interface
    ├── exam-results.html        # Results page
    ├── admin-dashboard.html     # Admin home
    ├── admin-create-exam.html   # Create exams
    ├── admin-add-question.html  # Add questions
    ├── admin-monitor.html       # Live monitoring
    └── admin-submissions.html   # View submissions
```

## 🔧 Configuration

### Backend Configuration
Update CORS origins in `backend/app/main.py` if needed:
```python
allow_origins=[
    "http://localhost:3000",
    "http://127.0.0.1:5500",
    # Add your frontend URL here
]
```

### Frontend Configuration
Update API base URL in `frontend/config.js`:
```javascript
const API_CONFIG = {
    BASE_URL: 'http://localhost:8000',  // Change to your backend URL
    // ...
};
```

## 👥 Default Users

### Create Admin User
Register through the UI or via API:
```bash
curl -X POST http://localhost:8000/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@examynex.com",
    "password": "admin123",
    "role": "admin"
  }'
```

### Create Student User
```bash
curl -X POST http://localhost:8000/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "student@examynex.com",
    "password": "student123",
    "role": "student"
  }'
```

## 📖 Usage Guide

### For Administrators

1. **Login** at `login.html` with admin credentials
2. **Create an Exam**:
   - Click "Create New Exam"
   - Enter title, description, and duration
   - Submit
3. **Add Questions**:
   - Click "Add Questions"
   - Select the exam
   - Add MCQ, text, or code questions
4. **Monitor Exams**:
   - Click "Live Monitoring" to see active sessions
   - View violations and student behavior
5. **Review Submissions**:
   - Click "View Submissions"
   - Filter by exam, sort by score
   - View detailed breakdowns

### For Students

1. **Login** at `login.html` with student credentials
2. **View Available Exams** on the dashboard
3. **Start an Exam**:
   - Click "Start Exam"
   - Grant webcam permission for proctoring
   - Answer questions
   - Answers auto-save as you go
4. **Submit Exam**:
   - Click "Submit Exam" when done
   - View results immediately

## 🔐 API Endpoints

### Authentication
- `POST /auth/register` - Register new user
- `POST /auth/login` - Login (returns JWT token)
- `GET /users/me` - Get current user info (requires auth)

### Exams
- `GET /exams/` - List all exams
- `GET /exams/{id}` - Get exam by ID
- `POST /exams/` - Create exam (admin only)
- `GET /exams/{id}/questions` - Get questions for exam

### Questions
- `POST /questions/` - Add question (admin only)
- `GET /questions/{exam_id}` - Get questions by exam

### Submissions
- `POST /submissions/answer` - Save answer (auto-save)
- `POST /submissions/submit` - Submit exam
- `GET /submissions/{exam_id}/result` - Get result

### Proctoring
- `POST /proctor/start` - Start proctoring session
- `POST /proctor/frame` - Upload frame for analysis
- `WS /ws/admin` - WebSocket for live monitoring

## 🐛 Troubleshooting

### Backend Issues

**Error: "ModuleNotFoundError"**
```bash
# Make sure virtual environment is activated
cd backend
#\Scripts\activate  # Windows
source #/bin/activate  # Mac/Linux
pip install -r requirments.txt
```

**Error: "Port 8000 already in use"**
```bash
# Find and kill the process using port 8000
# Windows:
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# Mac/Linux:
lsof -i :8000
kill -9 <PID>
```

**Error: "CORS policy"**
- Add your frontend URL to CORS origins in `backend/app/main.py`

### Frontend Issues

**Error: "Failed to fetch"**
- Ensure backend is running on the correct port
- Check `config.js` has correct API URL
- Verify CORS is configured properly

**Error: "Token expired"**
- Login again to get a new token

**Webcam not working**
- Grant camera permission in browser
- Use HTTPS in production (required for camera access)

## 🚀 Deployment

### Backend Deployment (Example: Heroku)
```bash
# Add Procfile
web: uvicorn app.main:app --host 0.0.0.0 --port $PORT

# Deploy
heroku create examynex-api
git push heroku main
```

### Frontend Deployment (Example: Netlify)
1. Update `config.js` with production backend URL
2. Deploy `frontend` folder to Netlify
3. Ensure backend CORS allows your domain

## 📝 Development

### Adding New Features

1. **Backend**: Add routes in `backend/app/routes/`
2. **Models**: Update `backend/app/models.py`
3. **Schemas**: Update `backend/app/schemas.py`
4. **Frontend**: Create new HTML file in `frontend/`
5. **Update Config**: Add new endpoint to `frontend/config.js`

### Testing

```bash
# Backend tests (if available)
cd backend
pytest

# Manual testing
# Use Postman or Thunder Client to test API endpoints
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License.

## 🙏 Acknowledgments

- FastAPI for the excellent backend framework
- OpenCV for face detection capabilities
- Google Fonts for Inter font family

## 📞 Support

For issues and questions:
- Open an issue on GitHub
- Email: support@examynex.com

---

**Built with ❤️ for better online education**
