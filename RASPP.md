
> **Stack:** React + TypeScript + FastAPI + MySQL  
> **Users:** Admin, Teacher, Student, Parent  
> **Goal:** Build a working demo that feels “real AI” + actionable intervention system.

---

This MVP focuses on **winning hackathon points**:

✅ Multi-role authentication (4 logins)  
✅ Teacher data entry (marks + attendance)  
✅ AI risk prediction (Safe / Medium / High)  
✅ Explainable output (“Top reasons”)  
✅ Student + Parent dashboards  
✅ Intervention system (recommendations + tracking)  
✅ Clean UI + charts + demo dataset  

---

# 👤 User Roles & Features (Complete)

## 1) 👑 Admin Login Features (Full Control)

### A) User & System Management
- Admin login (JWT)
- Create/Update/Delete Teachers
- Create/Update/Delete Students
- Create/Update/Delete Parents
- Link Parent ↔ Student (K–12)
- Manage Classes (1–12) and Subjects

### B) AI/Model Control (MVP)
- View model status (enabled/disabled)
- Upload training CSV (optional)
- Trigger model training (optional)
- View model metrics (accuracy, f1-score)
- View prediction history (all students)

### C) Admin Dashboard Analytics
- Total students
- Total teachers
- Total parents
- At-risk students count
- Risk distribution chart (Safe/Medium/High)
- Recent predictions stream

---

## 2) 👨‍🏫 Teacher Login Features (Most Important Role)

### A) Student Data Entry
- View students list (class-wise)
- Add/Update Marks (subject-wise)
- Add/Update Attendance (%)
- Add/Update Quiz score
- Add assignment status (Submitted / Pending)

### B) AI Risk Prediction (Core Demo)
- Button: **Predict Student Risk**
- Output:
  - Prediction label: Safe / Medium Risk / High Risk
  - Confidence %
  - Top factors (3–5)
  - Natural language explanation

Example:
- Attendance 62% → negative impact
- Quiz Avg 45/100 → negative impact
- Assignment pending → negative impact

### C) Teacher Dashboard
- At-risk students list (filterable)
- Class performance summary
- Student detail page:
  - marks chart
  - attendance
  - risk trend (optional)

### D) Intervention System (Hackathon Differentiator)
Teacher can create interventions like:
- Attendance counseling
- Extra practice plan
- Parent meeting request
- Assignment support
- Remedial class schedule

Tracking:
- Status: Pending / Applied / Completed
- Outcome: Success / Partial / Failed (optional)

---

## 3) 👨‍🎓 Student Login Features (Self Improvement)

### A) Student Dashboard
- Marks summary (subject-wise)
- Attendance %
- Weekly progress chart

### B) AI Prediction Report
- Risk level + confidence
- Top reasons (simple text)
- Teacher recommendations list

### C) Self Improvement Tips
- “What to improve this week”
- “Focus subject” suggestion
- Attendance reminder if low

---

## 4) 👨‍👩‍👧 Parent Login Features (K–12 Monitoring)

### A) Parent Dashboard (Child Overview)
- Child marks summary
- Child attendance %
- Weekly progress chart
- Recent prediction result

### B) AI Risk Alerts
- Risk level: Safe / Medium / High
- Simple explanation (Top 3 reasons)
- “Urgent alert” banner for High Risk

### C) Teacher Notes / Interventions
- View teacher recommendations
- View intervention status
- Option: “Request meeting” message (simple form)

---

# 🧠 AI Prediction (MVP Implementation)

## Risk Output Format (Standard API Response)

```json
{
  "student_id": 101,
  "risk_level": "High Risk",
  "confidence": 0.82,
  "top_factors": [
    { "feature": "Attendance", "value": 62, "impact": "negative" },
    { "feature": "Quiz Average", "value": 45, "impact": "negative" },
    { "feature": "Assignment Pending", "value": 3, "impact": "negative" }
  ],
  "natural_language": "High risk due to low attendance (62%), low quiz performance (45/100), and 3 pending assignments."
}
```

---

## AI Options (Choose One)

### Option A (Recommended for 30 Hours): Lightweight ML
- Logistic Regression OR RandomForest
- Train once with sample dataset
- Store model as `joblib`
- Use simple explainability:
  - feature importance (RF)
  - coefficient weights (LR)

### Option B (Fastest): Smart Rule-Based (Looks like AI)
- Rules using weighted scoring
- Still return same output format (confidence + top factors)
- Great for hackathon if ML training time is limited

---

# 🗄️ Database Schema (MVP)

## Required Tables
- `users` (id, name, email, password_hash, role)
- `students` (id, name, class, roll_no, parent_id)
- `parents` (id, name, phone, email)
- `teachers` (id, name, email, subject)
- `marks` (id, student_id, subject, marks, created_at)
- `attendance` (id, student_id, attendance_percent, week, created_at)
- `predictions_history` (id, student_id, risk_level, confidence, explanation_json, created_at)
- `interventions` (id, student_id, teacher_id, type, description, status, created_at)

---

# 🔐 Authentication & Permissions

## Roles
- ADMIN
- TEACHER
- STUDENT
- PARENT

## Rules
- Admin: all access
- Teacher: only assigned classes (MVP: allow all)
- Student: only self data
- Parent: only linked child data

---

# 📌 API Endpoints (MVP)

## Auth
- POST `/auth/login`
- POST `/auth/register` (admin-only or seeded)

## Admin
- GET `/admin/dashboard`
- CRUD `/admin/teachers`
- CRUD `/admin/students`
- CRUD `/admin/parents`
- POST `/admin/link-parent/{parent_id}/student/{student_id}`
- GET `/admin/predictions`

## Teacher
- GET `/teacher/students`
- POST `/teacher/marks`
- POST `/teacher/attendance`
- POST `/teacher/predict/{student_id}`
- POST `/teacher/intervention/{student_id}`
- GET `/teacher/interventions`

## Student
- GET `/student/me`
- GET `/student/marks`
- GET `/student/attendance`
- GET `/student/prediction`
- GET `/student/interventions`

## Parent
- GET `/parent/child`
- GET `/parent/child/marks`
- GET `/parent/child/attendance`
- GET `/parent/child/prediction`
- GET `/parent/child/interventions`
- POST `/parent/request-meeting`

---

# 🎨 Frontend Pages (MVP)

## Shared
- Login page (role-based)
- Navbar (role-based)
- Protected routes

## Admin UI
- Admin Dashboard
- Manage Teachers
- Manage Students
- Manage Parents
- Prediction History

## Teacher UI
- Teacher Dashboard
- Students List
- Student Detail Page
- Add Marks / Attendance
- Predict Risk
- Add Intervention

## Student UI
- Student Dashboard
- Marks page
- Attendance page
- Prediction page
- Recommendations page

## Parent UI
- Parent Dashboard
- Child Progress page
- Alerts page
- Teacher Notes page

---

# ⏳ 30-Hour Roadmap (Antigravity Execution Plan)

## Hour 0–2: Planning Mode (Antigravity)
**Deliverables:**
- Task list artifact
- DB schema
- API list
- UI wireframe list

Prompt:
> “Plan a 30-hour MVP for RASPP with 4 logins and AI risk prediction. Generate task list, schema, endpoints, and acceptance criteria.”

---

## Hour 2–6: Backend Setup (FastAPI + MySQL)
- Project structure
- Database connection
- JWT auth + role middleware
- Create tables/models

---

## Hour 6–10: Frontend Setup (React + Tailwind)
- App layout
- Login + routing
- Role-based dashboards (blank skeleton)

---

## Hour 10–16: Teacher Workflow (Core)
- Student list
- Add marks + attendance forms
- Store in DB
- Student detail page

---

## Hour 16–20: Prediction Service (AI Module)
- Implement risk scoring or lightweight ML
- Build `/teacher/predict/{student_id}`
- Save results to `predictions_history`

---

## Hour 20–24: Student + Parent Dashboards
- Student: view own marks + risk
- Parent: view child summary + alert banner

---

## Hour 24–27: Intervention System
- Teacher adds intervention
- Student/Parent can view interventions

---

## Hour 27–29: Charts + Explainability UI
- Risk distribution pie chart
- Top factors bar chart
- Weekly progress line chart

---

## Hour 29–30: Demo Polish
- Seed sample dataset
- Fix UI spacing
- Create 2-minute demo script

---

# 🧪 Antigravity Prompt Pack (Copy-Paste)

## 1) Backend Schema + Auth
> “Create FastAPI + SQLAlchemy models and migrations for users, students, parents, teachers, marks, attendance, predictions_history, interventions. Implement JWT auth and role-based access.”

## 2) Teacher Endpoints
> “Build teacher endpoints for adding marks, attendance, predicting risk, and adding interventions. Ensure student_id validation.”

## 3) Prediction Output
> “Implement predict endpoint returning risk_level, confidence, top_factors, and natural_language. Save prediction history.”

## 4) Frontend Role Routing
> “Create React protected routes for admin/teacher/student/parent dashboards with Tailwind layout.”

## 5) Charts UI
> “Build Recharts components: risk distribution, top factors bar chart, weekly progress line chart.”

---

# 🏆 Hackathon Demo Flow (2 Minutes)

1. Admin logs in → shows system overview + risk chart  
2. Teacher logs in → adds marks + attendance for student  
3. Teacher clicks “Predict Risk” → gets AI output + reasons  
4. Teacher assigns intervention (extra practice + parent meeting)  
5. Parent logs in → sees alert + teacher recommendation  
6. Student logs in → sees tips + improvement plan  

---

# ✅ MVP Success Checklist

- [ ] 4 logins working
- [ ] Teacher can add marks + attendance
- [ ] Prediction API works and saves history
- [ ] Student dashboard shows results
- [ ] Parent dashboard shows alert
- [ ] Intervention system working
- [ ] At least 2 charts working
- [ ] Seeded demo data

---

## End Note
This 30-hour MVP is designed to be **demo-ready, realistic, and hackathon-winning**.  
If you later want the full 10-week version, you can expand it into SHAP/LIME, WebSockets, Redis, and temporal tracking.
