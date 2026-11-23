# Student Dashboard Backend API

RESTful API untuk sistem Student Dashboard menggunakan Node-RED dan PostgreSQL.

## Tech Stack
- Node-RED (Docker)
- PostgreSQL 17
- Postman

## Struktur Folder
- `database/schema.sql` — skema tabel `classes`, `students`, dan `app_settings`
- `flows/student-dashboard-flows.json` — flow Node-RED
- `README.md` — dokumentasi API

## Endpoints

Base URL: `http://localhost:1880/api/v1`

### Students
- `GET /students` — ambil daftar siswa (support filter `class_id`, `className`, `status`, `search`)
- `GET /students/:id` — ambil detail siswa berdasarkan `id`
- `POST /students` — tambah data siswa baru
- `PUT /students` — update data siswa berdasarkan `id` atau `nis`
- `PUT /students/:id` — update data siswa berdasarkan `:id`
- `DELETE /students/:id` — hapus data siswa berdasarkan `:id`

### Classes
- `GET /classes` — ambil daftar kelas
- `GET /classes/:id` — ambil detail kelas berdasarkan `:id`
- `POST /classes` — tambah data kelas baru
- `PUT /classes/:id` — update data kelas berdasarkan `:id`
- `DELETE /classes/:id` — hapus data kelas berdasarkan `:id`

### App Settings
- `GET /app-settings` — ambil konfigurasi aplikasi
- `PUT /app-settings` — tambah/update konfigurasi (`upsert`)

### Dashboard Summary
- `GET /dashboard/summary` — ambil statistik ringkasan, `studentsPerClass`, `recentStudents`, `availableAcademicYears`, dan `appSettings` (support filter `academicYear`)

## Cara Menjalankan
1. Jalankan PostgreSQL 17 dan buat database, lalu eksekusi `database/schema.sql`.
2. Jalankan Node-RED, import `flows/student-dashboard-flows.json`, sesuaikan koneksi PostgreSQL, lalu klik Deploy.
3. Uji endpoint menggunakan Postman ke `http://localhost:1880/api/v1`.
