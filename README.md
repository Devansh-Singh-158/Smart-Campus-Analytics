# InnoVista Smart Campus Analytics: Student Success Platform

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Framework-Flask_3.x-lightgrey.svg)](https://palletsprojects.com/p/flask/)
[![Database](https://img.shields.io/badge/Database-SQLite3-green.svg)](https://www.sqlite.org/)
[![Charts](https://img.shields.io/badge/Visualizations-Chart.js_v4.4-orange.svg)](https://www.chartjs.org/)
[![Tests](https://img.shields.io/badge/Tests-266%2F266_Passing_(100%25)-brightgreen.svg)](file:///c:/Users/n8869/Desktop/INNOVISTA)
[![License](https://img.shields.io/badge/License-MIT-purple.svg)](file:///c:/Users/n8869/Desktop/INNOVISTA)

A comprehensive, production-grade **Student Success and Early Intervention Platform** engineered for university leadership, department heads, academic advisors, and placement coordinators. The system harmonizes **7 independent campus data streams** into transparent composite performance scores, predictive risk alerts, and actionable behavioral interventions.

---

## 📑 Table of Contents
1. [Executive Summary](#-executive-summary)
2. [Key Architecture & Data Integration](#-key-architecture--data-integration)
3. [Demo Staff Credentials](#-demo-staff-credentials)
4. [Quickstart & Setup Guide](#-quickstart--setup-guide)
5. [Application Directory & Page Walkthrough](#-application-directory--page-walkthrough)
6. [Scoring Formula & Risk Tiers](#-scoring-formula--risk-tiers)
7. [Behavioral Cohort Segments](#-behavioral-cohort-segments)
8. [Data Cleaning & ETL Pipeline](#-data-cleaning--etl-pipeline)
9. [RESTful JSON API Reference](#-restful-json-api-reference)
10. [Presentation Deck & Demo Script](#-presentation-deck--demo-script)
11. [Automated Test Battery](#-automated-test-battery)
12. [Project File Structure](#-project-file-structure)

---

## 🌟 Executive Summary

Higher education institutions face critical challenges with late-stage academic failures and career unreadiness. Student data is traditionally fragmented across isolated departmental silos—academic registrar databases, biometric attendance turnstiles, learning management systems (LMS), and placement office trackers.

**Smart Campus Analytics** transforms reactive counseling into proactive early intervention:
1. **Unifies 7 Campus Streams:** Integrates Academic Marks, Biometrics, LMS, Placement, Skills, Extracurriculars, and Feedback into a single SQLite database (`data/campus.db`).
2. **Computes Transparent 8-Indicator Success Scores (0–100):** Weighted multi-dimensional index paired with **Academic Readiness** and **Placement Readiness** subscores.
3. **Triages Risk in Real Time:** High Risk (*"Needs Attention"*), Medium Risk, and Low Risk classifications.
4. **Segments Cohorts into 6 Actionable Clusters:** Prescribes tailored institutional interventions (e.g. mock coding clinics, remedial tutoring, honors fellowships).
5. **Provides Enterprise Role-Based Access Control:** Admin vs. Faculty View-Only modes with mandatory pre-entry security warnings, live recalculation, and an immutable audit ledger.
6. **Built for Any Device:** Fully responsive layout tested on laptop (1366x768, 1440x900) and tablet (768px, 1024px) viewports, complete with 15-minute inactivity session expiration and synthetic data disclaimers.

---

## 🏗️ Key Architecture & Data Integration

```
  ┌────────────────────────────────────────────────────────────────────────┐
  │                   7 RAW DATA SILOS (/data/raw/*.csv)                   │
  │ Academic │ Attendance │ LMS │ Engagement │ Placement │ Skills │Feedback│
  └───────────────────────────────────┬────────────────────────────────────┘
                                      │
                                      ▼
  ┌────────────────────────────────────────────────────────────────────────┐
  │             AUTOMATED 5-STEP ETL & DATA CLEANING ENGINE                │
  │   1. UID Normalization   │   2. Duplicate Drop   │   3. Median Impute  │
  │   4. Domain Clamping     │   5. Type Enforcement │   6. Audit Logging  │
  └───────────────────────────────────┬────────────────────────────────────┘
                                      │
                                      ▼
  ┌────────────────────────────────────────────────────────────────────────┐
  │                 RELATIONAL SQLITE DATABASE (campus.db)                 │
  │   • users    • batches    • students    • details    • audit_log       │
  └───────────────────────────────────┬────────────────────────────────────┘
                                      │
                                      ▼
  ┌────────────────────────────────────────────────────────────────────────┐
  │              SCORING & EXPLAINABILITY ENGINE (scoring.py)               │
  │   • Success Score (0-100)    • Academic Subscore   • Placement Subscore│
  │   • High/Med/Low Risk Flags  • 6 Behavioral Cohorts • "Why Flagged"    │
  └───────────────────────────────────┬────────────────────────────────────┘
                                      │
                                      ▼
  ┌────────────────────────────────────────────────────────────────────────┐
  │                FLASK APPLICATION & REST APIS (app.py)                  │
  │      • Session Auth (15m timeout)    • Role-based Access (Admin/Faculty│
  │      • Live Recalculation Engine     • Chart.js Visualizations         │
  └────────────────────────────────────────────────────────────────────────┘
```

---

## 🔑 Demo Staff Credentials

| Username | Password | Role | Permitted Actions |
|:---|:---|:---|:---|
| **`admin`** | **`admin123`** | **Admin** | **Full Read & Write Access:** Add new students, inline metric editing, CSV upload & cleaning, cohort re-scoring, and complete audit log inspection. |
| **`faculty1`** | **`faculty123`** | **Faculty** | **Read-Only Access:** Inspect cohort analytics, drill down into student records, review at-risk lists, and examine data quality reports. Mutation actions are blocked (HTTP 403). |

---

## 🚀 Quickstart & Setup Guide

### 1. Prerequisites
- **Python 3.10+**
- **pip** package installer

### 2. Dependency Installation
No Node.js or frontend build steps are required. Install the lightweight Python stack:
```powershell
pip install flask pandas numpy werkzeug
```

### 3. Initialize Synthetic Data & Database Pipeline
The repository includes a seeded random data generator producing 180 realistic students across 3 batches (`CSE 2023-27`, `ECE 2023-27`, `CSE 2024-28`):
```powershell
# Step 1: Generate 7 correlated CSVs with deliberate anomalies
python generate_data.py

# Step 2: Execute data cleaning pipeline and build SQLite database
python clean_and_merge.py

# Step 3: Compute normalized scores, subscores, segments, and explainability
python scoring.py
```

### 4. Launch the Web Application
```powershell
python app.py
```
Open your web browser and navigate to: **[http://127.0.0.1:5000](http://127.0.0.1:5000)**

---

## 📱 Application Directory & Page Walkthrough

### 1. Authentication Portal (`/login`)
- Clean split-screen brand hero showcasing the InnoVista emblem and login card.
- Bcrypt password hash verification and session management.
- Password visibility toggle (`👁️` / `🙈`).
- Unauthenticated requests to any page or API are immediately rejected (`302 Redirect` or `401 Unauthorized`).
- Persistent synthetic data disclaimer footer.

### 2. Cohort Selector (`/select-batch`)
- Card grid representing the active university batches:
  - **CSE 2023-27** (60 students • Computer Science & Engineering)
  - **ECE 2023-27** (60 students • Electronics & Communication)
  - **CSE 2024-28** (60 students • Computer Science & Engineering)
- Shows active student count, class average Success Score, and overall attendance.
- Keyboard accessible (`Enter` / `Space` selection).
- Active cohort is retained in session and switchable anytime via the top header pill.

### 3. Executive Batch Overview (`/overview`)
- **3 Summary KPI Stat Cards:** Total Students, Class Average Success Score, and Students Needing Attention count.
- **"How is this calculated?" Modal:** Interactive dialog explaining indicator weights, formulas, and normalization rules.
- **Students Requiring Attention Box:** Scrollable roster sorted lowest score first, with primary concern badges (`Low attendance`, `Multiple backlogs`, `Low marks`, `Placement risk`), and a 1-click **Download as CSV** button.
- **Key Trend Visualizations (Chart.js):**
  1. *Score Distribution by Risk Bands* (Low `<50`, Medium `50-64`, High `65+`).
  2. *Average Attendance Trend by Subject* with 75% institutional benchmark line.
- **3 Flow Navigation Cards:** Quick links to STUDENTS LIST, AT-RISK & SEGMENTS, and DATA MANAGEMENT.

### 4. Students Directory (`/students-view`)
- Searchable and filterable master table of all cohort students with avatar initials, UID, Name, CGPA, Attendance %, Backlogs, Success Score, and colored Risk Flags (🟢 Low, 🟡 Medium, 🔴 High).
- Real-time search by student name or UID substring.
- Risk filter (All, High, Medium, Low, Needs Attention).
- Cohort segment dropdown filter.
- Sortable table headers (click any column header to toggle ascending/descending).
- 1-click row click navigation to the student detail profile.

### 5. Student 360° Detail & Explainability (`/students-view/<uid>`)
- **Profile Header:** Student initials, Name, UID, active batch, cohort segment badge, Success Score, Academic subscore, Placement subscore, and Attendance compliance badge.
- **"Why Flagged" Diagnostic Panel:** Plain-English root causes detailing exact point deficits below cohort averages and actionable faculty playbooks.
- **Score Breakdown Chart (Chart.js):** Horizontal bar chart comparing student performance against cohort averages across all 8 indicators.
- **7 Integrated Data Source Cards:**
  1. *Academic Performance:* CGPA, Internals (/40), Exams (/60), Backlogs, and individual subject marks table.
  2. *Attendance Tracking:* Overall %, Subject Average %, Biometric compliance status, and subject attendance table.
  3. *LMS Engagement:* Login frequency, assignment completion %, resources downloaded, forum posts.
  4. *Extracurricular Activities:* Events attended, clubs joined, hackathons, certifications, workshops.
  5. *Placement Readiness:* Aptitude score, coding challenge score, mock interview rating, communication score, recruitment drives attended, offers received.
  6. *Skills Assessment:* Technical scores (Python, SQL, Problem Solving) & Soft Skills (Communication, Teamwork, Leadership).
  7. *Student & Faculty Feedback:* Satisfaction ratings (1–5 scale) across courses, faculty, infrastructure, and overall experience.

### 6. At-Risk Rosters & Behavioral Segments (`/at-risk-view`)
- **Side-by-Side Dual Risk Rosters:**
  - *Academic Risk List:* Flagged for attendance `<65%`, backlogs $\ge 3$, or low exam marks, showing Academic Sub-score and diagnostic reason.
  - *Placement Risk List:* Flagged for low aptitude, coding challenges, or mock interviews, showing Placement Sub-score and diagnostic reason.
- **Chart.js Segment Distribution:** Doughnut chart illustrating student shares across 6 behavioral profiles.
- **6 Behavioral Cohort Segment Cards:** Displays segment name, count pill, description, highlighted recommended intervention, and a **"View Students"** button opening an interactive roster dialog with search.

### 7. Data Management & Quality Control (`/data-management-view`)
- **Pre-Entry Security Warning Modal:**
  - Mandatory confirmation before modifying data: *"You are about to modify student records. Changes affect scores and risk flags for the whole batch and are logged. Continue?"*
  - Admin proceeds with full edit access; Faculty receives view-only mode notice.
- **Editable Students Table:** In-table editing of CGPA, marks, attendance, backlogs, LMS, and placement scores.
- **Add Student Form:** Validated inputs with boundary guidance hints.
- **CSV Upload & 5-Step Cleaning Pipeline:** Uploads new student batches, strips whitespace, drops duplicates, imputes missing numbers, clamps out-of-bounds metrics, recalculates scores, and logs fixes.
- **Data Quality Report:** Historical ledger of all anomalies detected, fixed, and imputed.
- **Administrative Audit Log:** Immutable audit trail capturing timestamp, staff username, action type (`ADD_STUDENT`, `EDIT_STUDENT`, `CSV_IMPORT`), target UID, and before-and-after change diffs (`attendance_overall: 58.0% → 76.0%`).

### 8. Phase 9 Enhancements
- **Synthetic Data Disclaimer Footer:** Small persistent footer on every page: *"Demo uses synthetic data generated for this prototype."*
- **15-Minute Inactivity Auto-Logout:** Dual client-side timer and server-side `before_request` timestamp check automatically logs out inactive staff after 15 minutes, returning them to `/login` with an informational flash alert.
- **Responsive Layout:** Tailored CSS media queries and flexbox/grid containers ensuring full usability across laptops (1366x768, 1440x900) and tablets (768px, 1024px).

---

## 📐 Scoring Formula & Risk Tiers

The composite **Success Score (0–100)** is calculated as a weighted sum of normalized indicators:

$$\text{Success Score} = \sum_{i=1}^{8} \left( \frac{\text{Weight}_i \times \text{Normalized Score}_i}{100} \right)$$

### 8 Indicator Weights & Normalization Matrix

| # | Indicator Domain | Weight | Native Scale | Normalized Scale (0–100) | Description |
|:---:|:---|:---:|:---|:---|:---|
| **1** | **Examinations** | **15%** | 0 – 60 marks | `(Exam Avg / 60) × 100` | Major semester external examination scores. |
| **2** | **Internal Assessments** | **15%** | 0 – 40 marks | `(Internal Avg / 40) × 100` | Continuous internal evaluations and quizzes. |
| **3** | **Attendance Tracking** | **15%** | 0 – 100% | Directly `Attendance %` | Physical lecture and laboratory session presence. |
| **4** | **Placement Readiness** | **20%** | Tests & Drives | `80% Test Avg + 20% Drives` | Aptitude speed, coding tests, mock interviews, drives attended. |
| **5** | **LMS Engagement** | **10%** | Diverse metrics | `40% Assg + 30% Logins + 15% Downloads + 15% Posts` | Digital engagement on university portal. |
| **6** | **Skills Assessment** | **10%** | 0 – 100 scores | Mean of Python, SQL, Problem Solving, Soft Skills | Hands-on laboratory coding and communication skills. |
| **7** | **Extracurricular Activities**| **8%** | Count (0–30) | `(Total Activities / 30) × 100` | Clubs, hackathons, workshops, certifications. |
| **8** | **Campus Feedback** | **7%** | 1.0 – 5.0 rating | `(Average Rating / 5.0) × 100` | Course satisfaction, faculty evaluation, campus experience. |

### Domain Sub-Scores
- **Academic Readiness (0–100):** Combines Exams (15%), Internals (15%), Attendance (15%), and LMS (10%) scaled over 55 points.
- **Placement Readiness (0–100):** Combines Placement (20%), Skills (10%), and Extracurriculars (8%) scaled over 38 points.

### Objective Risk Classification Rules
```
Score < 50.0  OR  Attendance < 65.0%  OR  Active Backlogs >= 3
                /                             \
          YES  /                               \  NO
              ▼                                 ▼
      ┌───────────────┐                  Score >= 65.0 ?
      │   HIGH RISK   │                    /        \
      │Needs Attention│              YES  /          \  NO
      └───────────────┘                  ▼            ▼
                                  ┌───────────┐  ┌────────────┐
                                  │ LOW RISK  │  │MEDIUM RISK │
                                  └───────────┘  └────────────┘
```

*For complete mathematical proofs and explainability algorithms, consult [SCORING.md](SCORING.md).*

---

## 🧩 Behavioral Cohort Segments

| Cohort Segment | Icon | Definition & Diagnostic Criteria | Prescribed Institutional Intervention |
|:---|:---:|:---|:---|
| **High Performers** | 🌟 | Excelling across academic grades (CGPA $\ge 8.0$), coding challenges, and campus activities. | Nominate for honors research projects, fellowship applications, and peer study tutoring leadership. |
| **Strong Academics but Low Placement Readiness** | 📘 | High classroom grades (CGPA $\ge 6.0$), but lagging in aptitude test speed, coding challenges, or mock interviews. | Enroll in targeted technical interview bootcamps, resume clinics, and weekly mock coding challenges. |
| **Placement Ready but Weak Academics** | 💻 | Strong practical coding skills and interview confidence, but at academic risk due to low internals, exam marks, or backlogs. | Schedule remedial tutoring sessions, previous exam question reviews, and backlog clearing milestones. |
| **Disengaged** | ⚠️ | Critically low attendance ($< 60\%$), neglected LMS activity, and multiple active backlogs. | Immediate 1-on-1 pastoral counseling, parent-mentor academic conference, and formal attendance warning notice. |
| **Needs Overall Support** | 🚨 | Facing comprehensive difficulties across both classroom academic assessments and placement readiness benchmarks. | Design an Individualized Student Support Plan (ISSP) pairing faculty remedial sessions with peer study buddies. |
| **Steady** | ⚖️ | Stable cohort students comfortably meeting departmental benchmarks without acute deficits. | Provide regular semester milestone checkpoints and encourage participation in upcoming hackathons and clubs. |

---

## 🧹 Data Cleaning & ETL Pipeline

The ETL pipeline in `clean_and_merge.py` applies 5 deterministic cleaning rules before merging records into SQLite:
1. **UID Normalization:** Removes internal and outer whitespace, converts letters to uppercase (e.g. `' cse 2023005 '` $\rightarrow$ `'CSE2023005'`).
2. **Duplicate Removal:** Identifies and drops exact duplicate rows, retaining the latest record.
3. **Out-of-Range Clamping:** Clamps metric boundaries (CGPA `[0.0, 10.0]`, Internals `[0.0, 40.0]`, Exams `[0.0, 60.0]`, Attendance `[0.0, 100.0]`).
4. **Missing Value Imputation:** Imputes missing numbers using the column median across the cohort.
5. **Type Enforcement:** Casts count features (backlogs, hackathons, clubs, certifications) to integers.
6. **Data Quality Ledger:** Logs every applied fix with affected counts into the `data_quality_report` table.

---

## 🌐 RESTful JSON API Reference

Every route requires session authentication except `/api/auth/login`. Unauthenticated calls receive `HTTP 401 Unauthorized`. Mutation endpoints (`POST`, `PUT`) require Admin privileges (`HTTP 403 Forbidden` for Faculty).

| Method | Endpoint | Access | Description |
|:---|:---|:---|:---|
| `POST` | `/api/auth/login` | Public | Authenticates credentials (`username`, `password`) and sets session. |
| `POST` | `/api/auth/logout` | Authenticated | Clears user session. |
| `GET` | `/api/auth/me` | Authenticated | Returns currently authenticated user profile and active batch. |
| `GET` | `/api/batches` | Authenticated | Lists all academic batches with student counts and average KPIs. |
| `GET` | `/api/batch/summary` | Authenticated | Returns batch summary metrics and attention roster with concern tags. |
| `GET` | `/api/batch/attention-csv` | Authenticated | Downloads attention roster as CSV sorted lowest score first. |
| `GET` | `/api/students` | Authenticated | Search and filter students (`search`, `risk`, `segment`, `sort_by`, `sort_order`). |
| `GET` | `/api/students/<uid>` | Authenticated | Full 360° profile with 7 data sources, reasons, and normalized contributions. |
| `POST` | `/api/students` | **Admin Only** | Adds a new student record; automatically recalculates scores and logs audit. |
| `PUT` | `/api/students/<uid>` | **Admin Only** | Modifies student metrics; validates boundaries, recalculates, and logs diffs. |
| `POST` | `/api/upload-csv` | **Admin Only** | Ingests CSV file through automated cleaning pipeline and updates database. |
| `GET` | `/api/data-management/template-csv` | Authenticated | Downloads sample CSV template with standard student headers. |
| `GET` | `/api/at-risk` | Authenticated | Returns academic risk list, placement risk list, and segment rosters. |
| `GET` | `/api/trends` | Authenticated | Returns Chart.js distributions (risk bands, attendance trends, skills radar). |
| `GET` | `/api/data-quality` | Authenticated | Returns summary of cleaning fixes and all report entries. |
| `GET` | `/api/audit-logs` | Authenticated | Returns historical modification events with user, action, and field diffs. |

---

## 🎤 Presentation Deck & Demo Script

A structured **5-slide pitch deck outline** and an exact **3-minute live demonstration script** aligned with competition judging criteria are available in [PRESENTATION.md](PRESENTATION.md):
- **Slide 1:** Problem Understanding & Institutional Value
- **Slide 2:** Multi-Source Data Integration & Cleaning Pipeline
- **Slide 3:** Explainable Success Score, Sub-Scores & Behavioral Cohorts
- **Slide 4:** Interactive Dashboard & Decision-Support UX
- **Slide 5:** Administrative Control, Auditability & Enterprise Scalability
- **3-Minute Demo Script:** Timed walkthrough covering Login, Overview, Student Drill-Down, At-Risk Segments, and Data Management.

---

## 🧪 Automated Test Battery

The project includes **7 test suites** containing **266+ automated tests** verifying every phase:

```powershell
# Run the complete test suite
python test_phase9_e2e.py
python test_phase8_data_management.py
python test_phase7_at_risk.py
python test_phase6_students.py
python test_phase5_overview.py
python test_frontend_shell.py
python test_backend.py
```

```
======================================================================
ALL TEST SUITES PASSED: 266 / 266 (100% SUCCESS)
Database integrity verified (180 student records maintained across 3 batches).
======================================================================
```

---

## 📁 Project File Structure

```
INNOVISTA/
│
├── app.py                         # Production Flask application & RESTful API routes
├── generate_data.py               # Seeded synthetic data generator (7 raw CSVs, 180 students)
├── clean_and_merge.py             # 5-step ETL cleaning pipeline & SQLite relational schema
├── scoring.py                     # 8-indicator scoring, subscores, segments & explainability
│
├── SCORING.md                     # Complete scoring framework documentation
├── PRESENTATION.md                # 5-slide pitch deck outline & 3-minute demo script
├── README.md                      # Comprehensive project guide & setup instructions
│
├── data/
│   ├── campus.db                  # Production SQLite database (users, students, details, audit)
│   └── raw/                       # 7 generated raw CSV files
│       ├── academic.csv
│       ├── attendance.csv
│       ├── lms.csv
│       ├── engagement.csv
│       ├── placement.csv
│       ├── skills.csv
│       └── feedback.csv
│
├── templates/
│   ├── base.html                  # Shared layout: header, cohort badge, logout, footer
│   ├── login.html                 # Page 1: Authentication portal
│   ├── select_batch.html          # Page 2: Cohort selector grid
│   ├── overview.html              # Page 3: Executive batch overview & Chart.js trends
│   ├── students_list.html         # Page 4: Searchable, filterable students directory
│   ├── student_detail.html        # Page 5: 360° profile, why-flagged & 7 data sources
│   ├── at_risk.html               # Page 6: Dual risk rosters & behavioral cohort cards
│   └── data_management.html       # Page 7: Pre-entry modal, edit table, CSV upload, audit log
│
├── static/
│   ├── css/
│   │   └── style.css              # Universal design system (Inter font, responsive, cards)
│   └── js/
│       ├── chart.min.js           # Locally bundled Chart.js v4.4.1 (offline-ready)
│       └── main.js                # Core JS: password toggle, batch submit, 15m auto-logout
│
└── test_*.py                      # 7 Automated test suites (266+ tests)
    ├── test_phase9_e2e.py
    ├── test_phase8_data_management.py
    ├── test_phase7_at_risk.py
    ├── test_phase6_students.py
    ├── test_phase5_overview.py
    ├── test_frontend_shell.py
    └── test_backend.py
```

---

*InnoVista Smart Campus Analytics • Developed with Python Flask, SQLite, and Chart.js.*
