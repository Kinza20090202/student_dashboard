# Student Dashboard Backend API

RESTful API untuk sistem **Student Dashboard** yang dibangun menggunakan **Node-RED** dan **PostgreSQL**. Layanan ini menyediakan endpoint CRUD untuk manajemen data siswa dan kelas (dengan relasi foreign key `class_id`), pengaturan aplikasi (`app_settings` dengan logika upsert), serta agregasi statistik ringkasan dashboard (`GET /api/v1/dashboard/summary`).

---

## Tech Stack

- **API & Workflow Engine:** Node-RED (Docker)
- **Relational Database:** PostgreSQL 17
- **API Testing:** Postman
- **Version Control:** Git & GitHub

---

## Struktur Folder Repository

```text
student.dashboard.backend/
├── database/
│   └── schema.sql
├── flows/
│   └── student-dashboard-flows.json
├── .gitignore
└── README.md
Skema Database1. classesMenyimpan data kelas dan tahun ajaran.id (SERIAL PRIMARY KEY)code (VARCHAR(50) UNIQUE NOT NULL)name (VARCHAR(100) NOT NULL)homeroom_teacher (VARCHAR(120))academic_year (VARCHAR(30))created_at (TIMESTAMP DEFAULT CURRENT_TIMESTAMP)updated_at (TIMESTAMP DEFAULT CURRENT_TIMESTAMP)2. studentsMenyimpan data siswa yang berelasi ke tabel classes.id (SERIAL PRIMARY KEY)nis (VARCHAR(50) UNIQUE NOT NULL)full_name (VARCHAR(150) NOT NULL)class_id (INTEGER REFERENCES classes(id) ON DELETE SET NULL)class_name (VARCHAR(100))email (VARCHAR(150) UNIQUE)status (VARCHAR(30) DEFAULT 'active')created_at (TIMESTAMP DEFAULT CURRENT_TIMESTAMP)updated_at (TIMESTAMP DEFAULT CURRENT_TIMESTAMP)3. app_settingsMenyimpan konfigurasi aplikasi berbasis key-value (upsert berdasarkan setting_key).id (SERIAL PRIMARY KEY)setting_key (VARCHAR(100) UNIQUE NOT NULL)setting_value (TEXT NOT NULL)description (TEXT)created_at (TIMESTAMP DEFAULT CURRENT_TIMESTAMP)updated_at (TIMESTAMP DEFAULT CURRENT_TIMESTAMP)Daftar Endpoint APIBase URL: http://localhost:1880/api/v11. StudentsMethodEndpointQuery / PayloadDeskripsiGET/students?class_id=, ?className=, ?status=, ?search=Mengambil daftar siswa beserta informasi kelas (JOIN classes).GET/students/:idPath :idMengambil detail data siswa berdasarkan id.POST/studentsBody JSON (nis, full_name, classId / className, email, status)Menambahkan data siswa baru.PUT/studentsBody JSON (id atau nis, field yang diubah)Memperbarui data siswa berdasarkan id atau nis.PUT/students/:idPath :id + Body JSONMemperbarui data siswa berdasarkan parameter :id.DELETE/students/:idPath :idMenghapus data siswa berdasarkan :id.2. ClassesMethodEndpointQuery / PayloadDeskripsiGET/classes-Mengambil seluruh daftar kelas.GET/classes/:idPath :idMengambil detail kelas berdasarkan :id.POST/classesBody JSON (code, name, homeroom_teacher, academic_year)Menambahkan data kelas baru.PUT/classes/:idPath :id + Body JSONMemperbarui data kelas berdasarkan :id.DELETE/classes/:idPath :idMenghapus data kelas berdasarkan :id.3. App SettingsMethodEndpointQuery / PayloadDeskripsiGET/app-settings-Mengambil seluruh konfigurasi aplikasi.PUT/app-settingsBody JSON (settingKey, settingValue, description)Menyimpan atau memperbarui konfigurasi (upsert).4. Dashboard SummaryMethodEndpointQuery / PayloadDeskripsiGET/dashboard/summary?academicYear=Mengambil statistik ringkasan, studentsPerClass, recentStudents, availableAcademicYears, dan appSettings.Spesifikasi Request & Response1. GET /api/v1/dashboard/summary?academicYear=2026/2027Mengembalikan agregasi data utama untuk tampilan dashboard (200 OK):JSON{
  "success": true,
  "message": "Dashboard summary berhasil diambil.",
  "data": {
    "academicYear": "2026/2027",
    "stats": {
      "totalStudents": 48,
      "activeStudents": 45,
      "inactiveStudents": 3,
      "totalClasses": 4
    },
    "studentsPerClass": [
      {
        "classId": 1,
        "className": "XI RPL 1",
        "academicYear": "2026/2027",
        "studentCount": 12
      }
    ],
    "recentStudents": [
      {
        "id": 10,
        "nis": "24250010",
        "full_name": "Kinza Abrisam",
        "class_id": 1,
        "class_name": "XI RPL 1",
        "status": "active",
        "created_at": "2026-11-20T08:00:00.000Z"
      }
    ],
    "availableAcademicYears": [
      "2026/2027",
      "2025/2026"
    ],
    "appSettings": {
      "school_name": "SMK Telkom Malang",
      "semester": "Ganjil"
    }
  }
}
2. POST /api/v1/studentsContoh body request (201 Created):JSON{
  "nis": "24250011",
  "full_name": "Alya Rahma",
  "classId": 1,
  "email": "alya.rahma@example.com",
  "status": "active"
}
Contoh response sukses:JSON{
  "success": true,
  "message": "Data siswa berhasil ditambahkan.",
  "data": {
    "id": 11,
    "nis": "24250011",
    "full_name": "Alya Rahma",
    "class_id": 1,
    "class_name": "XI RPL 1",
    "email": "alya.rahma@example.com",
    "status": "active"
  }
}
3. PUT /api/v1/app-settingsContoh body request upsert (200 OK):JSON{
  "settingKey": "active_academic_year",
  "settingValue": "2026/2027",
  "description": "Tahun ajaran aktif pada dashboard"
}