# 🎓 ENSAO Grading System

A lightweight, PHP/MySQL web application for managing and consulting student grades at **ENSA Oujda**. The system provides two role-based dashboards — one for **teachers** (grade entry & class statistics) and one for **students** (grade consultation & ranking).

> ⚠️ **Status:** Academic / demo project. Authentication is not yet wired to the database and queries are not parameterized — see [Known Issues & Roadmap](#-known-issues--roadmap) before any real deployment.

---

## ✨ Features

### 👨‍🏫 Teacher (`Prof/`)
- Dashboard with profile, taught classes, module count, and student count
- Filter students by academic year / semester / module
- Class statistics: pass rate, min/max/average grade, pass/fail counts
- Grade entry form (`notes.php`) — Contrôle Continu, TP, Projet, Examen, Note Finale
- Auto-calculation of the final grade when not explicitly entered:
  `CC×0.3 + TP×0.2 + Projet×0.25 + Examen×0.25`

### 🎓 Student (`Etudiant/`)
- Dashboard with profile, semester, course count, and overall average
- Filter grades by academic year / semester
- Per-module grade breakdown with color-coded badges (excellent / good / average / poor / pending)
- Class ranking (position out of total students for the selected period)

### 🔐 Login (`Login/`)
- Shared login page for both roles (static UI, ENSAO-branded)

---

## 🗂️ Project Structure

```
Grading-System-main/
├── Login/
│   ├── login.html          # Login page (UI only)
│   ├── style.css
│   └── image/
├── Etudiant/
│   ├── dashboard.php       # Student grades & ranking
│   ├── style.css
│   └── image/
├── Prof/
│   ├── dashboard.php       # Teacher overview & class stats
│   ├── notes.php           # Grade entry / editing
│   ├── style.css
│   └── image/
└── Database Setup/
    └── database_setup.sql  # Schema + sample data
```

---

## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Backend | PHP (`mysqli`) |
| Database | MySQL / MariaDB |
| Frontend | HTML5, CSS3, Bootstrap 5, Font Awesome, Remix Icon |
| Fonts | Google Fonts (League Spartan, Montserrat) |

---

## 🗃️ Database Schema

The database `ensao_grades` consists of 4 tables and 1 view (defined in `Database Setup/database_setup.sql`):

| Table | Description |
|---|---|
| `teachers` | Teacher ID, full name, email |
| `students` | Student ID (matricule), full name, email, program |
| `modules` | Module code and name |
| `grades` | Grades per student/teacher/year/semester/module (CC, TP, Projet, Examen, Note Finale) |
| `grade_details` (view) | Joined, human-readable view of grades with student/teacher/module names |

The script also seeds sample teachers, students, modules, and a few grades for testing.

---

## 🚀 Getting Started

### Prerequisites
- PHP 7.4+ with the `mysqli` extension
- MySQL / MariaDB
- A local web server (XAMPP, WAMP, MAMP, or `php -S`)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/Grading-System.git
   cd Grading-System
   ```

2. **Create the database**
   ```bash
   mysql -u root -p < "Database Setup/database_setup.sql"
   ```
   This creates the `ensao_grades` database, its tables, and inserts sample data.

3. **Configure the database connection**
   Each PHP file currently connects with:
   ```php
   $host = 'localhost';
   $dbname = 'ensao_grades';
   $username = 'root';
   $password = '';
   ```
   Update these values in `Prof/dashboard.php`, `Prof/notes.php`, and `Etudiant/dashboard.php` to match your local MySQL setup.

4. **Serve the project**
   ```bash
   php -S localhost:8000
   ```
   Or place the folder inside your `htdocs` / `www` directory if using XAMPP/WAMP.

5. **Open the app**
   - Login page: `http://localhost:8000/Login/login.html`
   - Teacher dashboard: `http://localhost:8000/Prof/dashboard.php`
   - Student dashboard: `http://localhost:8000/Etudiant/dashboard.php`

---

## 🧑‍💻 Usage Notes

- The login page currently redirects directly to the **Prof** dashboard regardless of the credentials entered — there is no real authentication flow yet.
- The **student** dashboard defaults to `student_id = GI2023457`; pass a different `?student_id=` in the URL to view another student (e.g. `dashboard.php?student_id=GI2023456`).
- The **teacher** dashboard/notes pages default to `teacher_id = ENS-2025-423`.

Sample accounts from the seed data:

| Type | ID | Name |
|---|---|---|
| Teacher | `ENS-2025-423` | Prof. Mohammed Ouadoud |
| Student | `GI2023456` | Younes Bouazzaoui |
| Student | `GI2023457` | Ilias Amrani |

---

## ⚠️ Known Issues & Roadmap

This project is a functional prototype. The following points should be addressed before any production-like use:

- [ ] **No real authentication** — `login.html` has no backend logic; add a PHP login endpoint with hashed passwords and session-based access control.
- [ ] **SQL injection risk** — queries build SQL strings via direct variable interpolation (`$_GET`/`$_POST`) instead of prepared statements. Migrate to `mysqli`/PDO prepared statements.
- [ ] **Hardcoded DB credentials** in each PHP file — move to a shared config file / environment variables.
- [ ] **No role-based access control** — dashboards are accessible by directly navigating to the URL and passing any `student_id`/`teacher_id`.
- [ ] **No password hashing / session validation** on protected pages.
- [ ] Add CSRF protection on the grade-submission form in `notes.php`.

---

## 👥 Contributors

Developed as a student project — MGSI, ENSA Oujda.

## 📄 License

Academic project — for educational purposes.
