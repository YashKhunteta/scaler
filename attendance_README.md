# AttendAI — AI-Powered Attendance & Payroll System

A modern, full-stack internal web application for remote employee attendance tracking and automated payroll calculation, powered by AI.

---

## 🚀 Quick Start (Development)

### Prerequisites
- Python 3.11+
- Node.js 20+
- pip

### 1. Backend Setup

```bash
cd backend

# Copy and configure environment
cp .env.example .env
# Edit .env with your settings (AI API key, etc.)

# Install dependencies
pip install -r requirements.txt

# Seed demo data (creates SQLite DB with sample employees)
python seed.py

# Start backend
uvicorn main:app --reload --port 8000
```

Backend runs at: http://localhost:8000  
API docs: http://localhost:8000/docs

### 2. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start frontend
npm run dev
```

Frontend runs at: http://localhost:5173

---

## 🔑 Demo Login Credentials

| Role | Email | Password |
|------|-------|----------|
| **HR Admin** | hr@company.com | Admin@123 |
| **Employee 1** | john.doe@company.com | Employee@123 |
| **Employee 2** | jane.smith@company.com | Employee@123 |
| **Employee 3** | mike.wilson@company.com | Employee@123 |
| **Employee 4** | emily.davis@company.com | Employee@123 |
| **Employee 5** | robert.brown@company.com | Employee@123 |

---

## 🤖 AI Configuration

Change AI provider by editing `.env` — **no code changes needed**:

### Google Gemini
```env
AI_PROVIDER=gemini
AI_MODEL=gemini-1.5-flash
AI_API_KEY=your_gemini_key
```

### OpenAI (GPT-4)
```env
AI_PROVIDER=openai
AI_MODEL=gpt-4o
AI_API_KEY=your_openai_key
```

### Anthropic Claude
```env
AI_PROVIDER=anthropic
AI_MODEL=claude-3-5-sonnet-20241022
AI_API_KEY=your_anthropic_key
```

### Azure OpenAI
```env
AI_PROVIDER=azure_openai
AI_MODEL=gpt-4o
AI_API_KEY=your_azure_key
AZURE_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_DEPLOYMENT=your-deployment-name
```

### Local Ollama (Free, No Key)
```env
AI_PROVIDER=openai_compatible
AI_MODEL=llama3
AI_API_KEY=ollama
AI_BASE_URL=http://localhost:11434/v1
```

---

## 🗄️ Switching to SQL Server (SSMS)

Currently using SQLite for demo. To switch to SQL Server:

1. Install ODBC Driver 17 for SQL Server
2. Update `.env`:
```env
DATABASE_URL=mssql+pyodbc://username:password@server/AttendanceDB?driver=ODBC+Driver+17+for+SQL+Server
```
3. Restart the backend — tables will be created automatically

---

## 🔐 OAuth Setup (Optional)

### Google OAuth
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create OAuth 2.0 credentials
3. Add `http://localhost:8000/auth/google/callback` as authorized redirect URI
4. Add to `.env`:
```env
GOOGLE_CLIENT_ID=xxx.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=GOCSPX-xxx
```

### Microsoft OAuth
1. Go to [Azure App Registrations](https://portal.azure.com/)
2. Register a new app
3. Add `http://localhost:8000/auth/microsoft/callback` as redirect URI
4. Add to `.env`:
```env
MICROSOFT_CLIENT_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
MICROSOFT_CLIENT_SECRET=xxx
```

---

## 📧 Email Notifications (Optional)

For AI warning emails, configure SMTP in `.env`:
```env
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your@gmail.com
SMTP_PASSWORD=your_app_password  # Use Gmail App Password, not account password
EMAIL_FROM=HR System <hr@company.com>
```

---

## 🐳 Docker Deployment

```bash
# Copy and configure environment
cp backend/.env.example backend/.env
# Edit backend/.env

# Build and start all services
docker-compose up -d --build

# Seed demo data
docker-compose exec backend python seed.py
```

Access at: http://localhost (port 80)

---

## 📁 Project Structure

```
attendance-payroll-system/
├── backend/
│   ├── main.py                 # FastAPI app entry point
│   ├── config.py               # Settings (pydantic-settings)
│   ├── seed.py                 # Demo data seeder
│   ├── requirements.txt
│   ├── database/
│   │   ├── db.py              # SQLAlchemy engine
│   │   └── models.py          # All ORM models
│   ├── auth/                  # JWT + OAuth authentication
│   ├── employees/             # Employee CRUD
│   ├── attendance/            # Check-in/out, rules engine
│   ├── leave/                 # Leave management
│   ├── payroll/               # Payroll calculation
│   ├── ai/                    # AI provider abstraction + chatbot
│   │   └── providers/         # Gemini, OpenAI, Anthropic, Azure, Compatible
│   ├── notifications/         # In-app + email notifications
│   └── reports/               # Excel + PDF export
├── frontend/
│   └── src/
│       ├── pages/
│       │   ├── employee/      # Dashboard, Attendance, Leave, Payroll, AIChat
│       │   └── hr/            # Dashboard, Employees, Attendance, Leaves, Payroll, Reports
│       ├── components/        # Reusable UI components
│       ├── services/          # API clients (axios)
│       ├── context/           # Auth context
│       └── hooks/             # Custom React hooks
├── nginx/
│   └── nginx.conf             # Reverse proxy config
└── docker-compose.yml
```

---

## 🧮 Payroll Formula

```
Per Day Salary    = Monthly Salary / Total Working Days
Deductions        = (Unpaid Leaves × Per Day) + (Half Days × Per Day × 0.5)
Overtime Pay      = Overtime Hours × (Per Day / 8) × 1.5
Final Salary      = Monthly Salary - Deductions + Overtime Pay + Bonuses
```

---

## ⚙️ Attendance Rules (Configurable in .env)

| Condition | Status |
|-----------|--------|
| Check-in after 09:30 AM | Late |
| Work hours ≥ 9 hours | Overtime |
| Work hours ≥ 8 hours | Full Day |
| Work hours 4–8 hours | Half Day |
| Work hours < 4 hours | Half Day |
| No check-in | Absent |

---

## 🛡️ AI Warning Notifications

The AI automatically sends notifications when:
- Employee checks in late
- 3+ late logins in a month (escalation)
- Absent for the day (end of day check)
- Suspicious attendance patterns detected
- Leave balance drops below 2 days

Notifications appear in-app (bell icon) and optionally via email.

---

## 📊 Features

| Feature | Employee | HR |
|---------|----------|-----|
| Attendance check-in/out | ✅ | - |
| View own attendance | ✅ | - |
| Leave application | ✅ | - |
| AI chatbot | ✅ | - |
| View salary | ✅ | - |
| Manage all employees | - | ✅ |
| Approve/reject leaves | - | ✅ |
| Generate payroll | - | ✅ |
| View analytics | - | ✅ |
| Export reports | - | ✅ |
| AI anomaly detection | - | ✅ |
| Send notifications | - | ✅ |

---

## 🔒 Security

- JWT-based authentication (configurable expiry)
- bcrypt password hashing
- Role-based access control (employee / hr)
- CORS configured for frontend origin
- All sensitive data in environment variables
