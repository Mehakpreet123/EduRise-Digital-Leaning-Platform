# 🎓 EduRise – Digital Learning Platform

EduRise is a modern **Android-based digital learning platform** designed to connect **schools, teachers, and students** under one unified system.  
It provides separate dashboards for **Admin**, **Teacher**, and **Student** users, ensuring seamless communication, learning management, and data tracking — all powered by **Firebase Firestore**.

---

## 🚀 Features

### 👩‍💼 **Admin Dashboard**
- Register schools, manage staff and students.  
- Create and assign classes, subjects, and timetables.  
- Upload content, manage announcements, and monitor analytics.  

### 👨‍🏫 **Teacher Dashboard**
- Manage assigned classes and subjects.  
- Upload study materials (PDFs, videos, quizzes, etc.).  
- Mark attendance and assess student progress.  
- Send notifications or announcements to students.  

### 👨‍🎓 **Student Dashboard**
- View daily timetable, upcoming classes, and announcements.  
- Access digital learning content uploaded by teachers.  
- Track personal learning progress and quiz scores.  
- Receive notifications and messages from teachers/admins.  

---

## 🧱 Tech Stack

| Component | Technology Used |
|------------|-----------------|
| **Frontend (App)** | Android (Kotlin) |
| **Backend Database** | Firebase Firestore |
| **Authentication** | Firebase Auth (Email/Password) |
| **Storage** | Firebase Storage |
| **UI Design** | XML Layouts with Material Design |
| **IDE** | Android Studio (Gradle-based project) |

---

## 🗂️ Firestore Database Schema (EduRise – DATABASE SCHEMA)

### 🏫 **schools**
| Column Name | Type | Description |
|--------------|------|-------------|
| `school_id` | TEXT (PK) | Unique ID (UUID) for each school |
| `school_name` | TEXT | Name of the school |
| `school_code` | TEXT (UNIQUE) | Unique code for school login |
| `address` | TEXT | School address |
| `created_at` | TIMESTAMP | Registration date |

---

### 👩‍🏫 **staff (teachers + admins)**
| Column Name | Type | Description |
|--------------|------|-------------|
| `staff_id` | TEXT (PK) | Unique staff ID |
| `school_id` | TEXT (FK) | References schools.school_id |
| `name` | TEXT | Full name |
| `email` | TEXT (UNIQUE) | Firebase Auth email |
| `password_hash` | TEXT | Encrypted password |
| `role` | TEXT | ENUM: `teacher` / `admin` |
| `phone` | TEXT | Contact number |
| `language_preference` | TEXT | en / pa / hi … |
| `created_at` | TIMESTAMP | Registration date |

---

### 👨‍🎓 **students**
| Column Name | Type | Description |
|--------------|------|-------------|
| `student_id` | TEXT (PK) | Unique student ID |
| `name` | TEXT | Full name |
| `email` | TEXT (UNIQUE) | Firebase Auth email |
| `password_hash` | TEXT | Encrypted password |
| `class_id` | TEXT (FK) | References classes.class_id |
| `section` | TEXT | Class section |
| `phone` | TEXT | Contact number |
| `language_preference` | TEXT | en / pa / hi … |
| `created_at` | TIMESTAMP | Registration date |

---

### 🏫 **classes**
| Column Name | Type | Description |
|--------------|------|-------------|
| `class_id` | TEXT (PK) | Unique class ID |
| `school_id` | TEXT (FK) | References schools.school_id |
| `class_name` | TEXT | Example: “Grade 8” |
| `section_count` | INTEGER | Number of sections (optional) |

---

### 📚 **subjects**
| Column Name | Type | Description |
|--------------|------|-------------|
| `subject_id` | TEXT (PK) | Unique ID |
| `school_id` | TEXT (FK) | References schools.school_id |
| `subject_name` | TEXT | Example: “Mathematics” |

---

### 🧑‍🏫 **classAssignments**
| Column Name | Type | Description |
|--------------|------|-------------|
| `assignment_id` | TEXT (PK) | Unique ID |
| `class_id` | TEXT (FK) | References classes.class_id |
| `section` | TEXT | Class section |
| `subject_id` | TEXT (FK) | References subjects.subject_id |
| `staff_id` | TEXT (FK) | References staff.staff_id |
| `assigned_at` | TIMESTAMP | Assignment date |

---

### ⏰ **timetables**
| Column Name | Type | Description |
|--------------|------|-------------|
| `timetable_id` | TEXT (PK) | Unique ID |
| `school_id` | TEXT (FK) | References schools.school_id |
| `class_id` | TEXT (FK) | References classes.class_id |
| `section` | TEXT | Class section |
| `subject_id` | TEXT (FK) | References subjects.subject_id |
| `staff_id` | TEXT (FK) | References staff.staff_id |
| `day_of_week` | TEXT | Monday, Tuesday, etc. |
| `start_time` | TIME | Start time |
| `end_time` | TIME | End time |
| `lecture_type` | TEXT | Regular / Extra / Lab |
| `room` | TEXT | Room number / location |

---

### 📘 **content**
| Column Name | Type | Description |
|--------------|------|-------------|
| `content_id` | TEXT (PK) | Unique ID |
| `school_id` | TEXT (FK) | References schools.school_id |
| `class_id` | TEXT (FK) | References classes.class_id |
| `section` | TEXT | Class section |
| `subject_id` | TEXT (FK) | References subjects.subject_id |
| `staff_id` | TEXT (FK) | Uploaded by teacher |
| `title` | TEXT | Content title |
| `type` | TEXT | ENUM: pdf / video / quiz / ppt |
| `file_url` | TEXT | Firebase Storage URL |
| `language` | TEXT | Content language |
| `created_at` | TIMESTAMP | Upload time |

---

### 📈 **progress**
| Column Name | Type | Description |
|--------------|------|-------------|
| `progress_id` | TEXT (PK) | Unique ID |
| `student_id` | TEXT (FK) | References students.student_id |
| `content_id` | TEXT (FK) | References content.content_id |
| `status` | TEXT | completed / in_progress / not_started |
| `score` | FLOAT | Quiz score or progress |
| `updated_at` | TIMESTAMP | Last sync time |

---

### 📝 **attendance**
| Column Name | Type | Description |
|--------------|------|-------------|
| `attendance_id` | TEXT (PK) | Unique ID (UUID) |
| `class_id` | TEXT (FK) | References classes.class_id |
| `section` | TEXT | Section |
| `date` | DATE | Attendance date |
| `marked_by` | TEXT (FK) | References staff.staff_id |
| `attendance_data` | JSON | `{ "stu_id":"001", "name":"John Doe", "status":"present" }` |

---

### 📊 **analytics** *(optional)*
| Column Name | Type | Description |
|--------------|------|-------------|
| `analytics_id` | TEXT (PK) | Unique ID |
| `school_id` | TEXT (FK) | References schools.school_id |
| `metric_name` | TEXT | Example: "Attendance Rate" |
| `metric_value` | FLOAT | Calculated metric |
| `period` | TEXT | e.g., "2025-10", "weekly" |
| `generated_at` | TIMESTAMP | Last updated |

---

## 🧩 App Architecture

- **Firebase Firestore** for real-time data synchronization.  
- **Fragments** used for each module within dashboards.  
- **Navigation Component** for smooth screen transitions.


