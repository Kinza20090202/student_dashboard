# Student Dashboard Backend API

RESTful API untuk sistem **Student Dashboard** yang dibangun menggunakan **Node-RED** dan **PostgreSQL**. Mendukung manajemen data siswa, kelas (dengan relasi foreign key `class_id`), pengaturan aplikasi (`app_settings`), serta statistik ringkasan dashboard.

---

## Tech Stack

- **Runtime / API Builder:** Node-RED (Docker)
- **Database:** PostgreSQL 17
- **API Testing:** Postman

---

## Struktur Database

1. **`classes`** — Menyimpan data kelas (`id`, `code`, `name`, `homeroom_teacher`, `academic_year`, `created_at`, `updated_at`).
2. **`students`** — Menyimpan data siswa (`id`, `nis`, `full_name`, `class_id`, `class_name`, `email`, `status`, `created_at`, `updated_at`) dengan foreign key `class_id` yang merujuk ke `classes(id)`.
3. **`app_settings`** — Menyimpan konfigurasi aplikasi (`id`, `setting_key`, `setting_value`, `description`, `created_at`, `updated_at`).

---

## Daftar Endpoint API

Base URL: `http://localhost:1880/api/v1`

### 1. Students
| Method | Endpoint | Deskripsi |
| :--- | :--- | :--- |
| `GET` | `/students` | Mengambil seluruh data siswa beserta info kelas (`JOIN classes`). Mendukung filter `?class_id=`, `?className=`, `?status=`, dan `?search=`. |
| `GET` | `/students/:id` | Mengambil detail data siswa berdasarkan `id`. |
| `POST` | `/students` | Menambahkan data siswa baru (mendukung input `classId` maupun `className`). |
| `PUT` | `/students` | Memperbarui data siswa berdasarkan `id` atau `nis`. |
| `PUT` | `/students/:id` | Memperbarui data siswa berdasarkan parameter `:id`. |
| `DELETE` | `/students/:id` | Menghapus data siswa berdasarkan `:id`. |

### 2. Classes
| Method | Endpoint | Deskripsi |
| :--- | :--- | :--- |
| `GET` | `/classes` | Mengambil seluruh daftar kelas. |
| `GET` | `/classes/:id` | Mengambil detail kelas berdasarkan `:id`. |
| `POST` | `/classes` | Menambahkan data kelas baru. |
| `PUT` | `/classes/:id` | Memperbarui data kelas berdasarkan `:id`. |
| `DELETE` | `/classes/:id` | Menghapus data kelas berdasarkan `:id`. |

### 3. App Settings
| Method | Endpoint | Deskripsi |
| :--- | :--- | :--- |
| `GET` | `/app-settings` | Mengambil seluruh konfigurasi aplikasi. |
| `PUT` | `/app-settings` | Menyimpan atau memperbarui konfigurasi berdasarkan `settingKey` (upsert). |

### 4. Dashboard Summary
| Method | Endpoint | Deskripsi |
| :--- | :--- | :--- |
| `GET` | `/dashboard/summary` | Mengambil ringkasan statistik siswa & kelas, `studentsPerClass`, `recentStudents`, `availableAcademicYears`, dan `appSettings`. Mendukung filter `?academicYear=`. |

---

## Standar Format Respons

**Respons Sukses (`200` / `201`):**
```json
{
  "success": true,
  "message": "Data berhasil diproses.",
  "data": {}
}