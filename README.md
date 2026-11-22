# Resume-analyzer-and--job-description
json-server is now active!
# SkillBridge AI – AI-Powered Resume Analyzer & Career Guidance

SkillBridge AI is a web application designed to help students and job seekers review their resumes, understand their skills, identify potential skill gaps, and explore relevant job roles.

The project combines a React frontend with a Python FastAPI backend and a SQLite database. It includes resume upload and text extraction, skill analysis utilities, resume scoring, job-related functionality, and an AI-analysis integration through OpenRouter.

> **Project status:** This README describes the functionality and structure present in the supplied project. Some analysis routes use different implementations; verify the live AI response and scoring behaviour before presenting every result as AI-generated.

---

## Features

- User registration and login
- Password hashing and JWT-based authentication
- Resume upload with PDF and DOCX file support in the backend
- Resume text extraction
- Candidate detail and skill extraction utilities
- Resume score calculation
- Missing-skill / skill-gap functionality
- Resume analysis endpoints, including an OpenRouter integration
- Dashboard statistics, including resume count and average score
- Job creation and job listing APIs
- React pages for login, registration, resume upload, dashboard, and resume analysis

## Technology Stack

| Area | Technologies |
|---|---|
| Frontend | React, JavaScript, Vite, React Router |
| API client | Axios |
| Backend | Python, FastAPI, Uvicorn |
| Database | SQLite, SQLAlchemy |
| Authentication | JWT, Passlib |
| Resume processing | PyPDF2, python-docx |
| AI integration | OpenRouter API |

---

## Project Structure

```text
skillBridge-AI/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── config.py
│   │   └── database.py
│   ├── models/
│   │   ├── user.py
│   │   ├── resume.py
│   │   └── job.py
│   ├── routers/
│   │   ├── user.py
│   │   ├── resume.py
│   │   ├── dashboard.py
│   │   └── job.py
│   ├── schemas/
│   ├── services/
│   ├── uploads/             # Keep user-uploaded resumes out of Git
│   └── requirements.txt
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── context/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── index.html
│
└── README.md
```

---

## Getting Started

### Prerequisites

Install the following before running the project:

- Python 3.10 or newer
- Node.js and npm
- Git

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/skillBridge-AI.git
cd skillBridge-AI
```

Replace `YOUR_USERNAME` with your GitHub username and use the exact repository URL.

### 2. Set up the backend

Open a terminal in the project root:

```bash
cd backend
```

Create and activate a virtual environment.

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install Python dependencies:

```bash
pip install -r requirements.txt
```

### 3. Configure the OpenRouter API key

The supplied project configuration contains an API-key setting in `backend/app/config.py`. Do not publish a real key in source code or commit it to Git.

Before running or publishing the project:

1. Revoke and replace any API key that has previously been committed or shared.
2. Read the key from an environment variable instead of hardcoding it.
3. Keep the `.env` file out of version control.

For example, update the configuration to read an environment variable:

```python
import os

OPENROUTER_API_KEY = os.getenv("OPENROUTER_API_KEY", "")
```

Set the variable in PowerShell for the current terminal:

```powershell
$env:OPENROUTER_API_KEY="your_new_openrouter_api_key"
```

For macOS / Linux:

```bash
export OPENROUTER_API_KEY="your_new_openrouter_api_key"
```

If the AI integration is not configured, AI-powered endpoints may not work.

### 4. Run the backend

From the `backend` directory:

```bash
uvicorn app.main:app --reload
```

The API will be available at:

```text
http://127.0.0.1:8000
```

FastAPI interactive API documentation:

```text
http://127.0.0.1:8000/docs
```

### 5. Set up the frontend

Open a second terminal:

```bash
cd frontend
npm install
npm run dev
```

Vite will display the local frontend URL, usually:

```text
http://localhost:5173
```

The frontend API client currently uses `http://127.0.0.1:8000` as its backend base URL. If your backend runs elsewhere, update `frontend/src/services/api.js`.

---

## API Overview

The backend registers the following route groups:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/` | Backend welcome message |
| GET | `/users/` | Users API status |
| POST | `/users/register` | Register a user |
| POST | `/users/login` | Log in |
| POST | `/resume/upload` | Upload a resume (authenticated) |
| GET | `/resume/` | List resumes |
| GET | `/resume/{resume_id}` | Retrieve a resume |
| GET | `/resume/download/{resume_id}` | Download a resume |
| DELETE | `/resume/{resume_id}` | Delete a resume |
| POST | `/resume/analyze/{resume_id}` | Run resume analysis |
| POST | `/resume/gemini/{resume_id}` | Run the Gemini-named analysis route |
| POST | `/resume/skill-gap/{resume_id}` | Run skill-gap analysis |
| GET | `/dashboard/` | Retrieve dashboard statistics |
| POST | `/jobs/add` | Add a job |
| GET | `/jobs/` | List jobs |

For request schemas, response formats, and interactive testing, run the backend and visit `/docs`.

---

## How It Works

1. A user registers or logs in.
2. The user uploads a resume.
3. The backend validates the file type and size, stores the file, and extracts its text.
4. The backend's analysis utilities can extract candidate details and skills.
5. Resume analysis, scoring, and skill-gap routes process the resume data.
6. Dashboard and job endpoints provide related information to the frontend.

The exact analysis logic depends on the endpoint used. Review the corresponding backend service before describing a particular score or recommendation as an AI-generated result.

---

## Security and Privacy Notes

Before making the repository public:

- **Rotate exposed API keys.** Removing a key from the latest file is not enough if it exists in Git history. Revoke it with the provider and clean the repository history if needed.
- Do not commit `.env` files, API keys, database files, or personal resume uploads.
- Do not publish real users' resumes or personal information.
- Add access-control checks so users can only view, download, analyze, or delete their own resumes.
- Restrict and validate uploaded file size and content.
- Configure CORS with only the trusted frontend origins used in deployment.
- Use HTTPS and secure token handling in production.
- Remove or protect debug and user-list endpoints before deployment.

A suggested `.gitignore`:

```gitignore
# Python
__pycache__/
*.py[cod]
.venv/
venv/

# Secrets
.env
.env.*

# Local database
*.db
*.sqlite
*.sqlite3

# User uploads
backend/uploads/
uploads/

# Frontend
frontend/node_modules/
frontend/dist/

# OS / editor
.DS_Store
Thumbs.db
.vscode/
```

If sensitive files or credentials were already committed, add them to `.gitignore`, remove them from tracking, and rotate the credentials. `.gitignore` alone does not remove files from Git history.

---

## Future Improvements

- Match resumes against a specific job description
- Improve ATS keyword and formatting checks
- Provide explainable resume score breakdowns
- Add personalized learning roadmaps for missing skills
- Improve job recommendations using job requirements
- Add automated backend and frontend tests
- Add deployment configuration and production environment settings

---

## Contribution

Contributions, issues, and suggestions are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Commit your changes.
4. Push the branch.
5. Open a Pull Request.

---

## License

Add a license file before describing this project as open source.

---

**SkillBridge AI** – Helping job seekers understand their resumes, strengthen their skills, and take the next step in their careers.
