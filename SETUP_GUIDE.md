# 📋 Panduan Koneksi Frontend-Backend-Database

## 🎯 Ringkasan Integrasi

Sudah saya integrasikan frontend dengan backend dan database dengan lengkap:

### ✅ Yang Sudah Dilakukan:

1. **Backend (FastAPI)**
   - ✅ Membuat 16+ API endpoints untuk semua operasi database
   - ✅ Endpoint untuk CRUD siswa, guru, kelas, jadwal
   - ✅ Endpoint untuk absensi masuk/keluar
   - ✅ Endpoint untuk form izin
   - ✅ Endpoint untuk rekap absensi
   - ✅ CORS middleware sudah dikonfigurasi

2. **Frontend (React)**
   - ✅ Update `src/api.js` dengan 16+ fungsi fetch
   - ✅ Semua fungsi terhubung ke backend API
   - ✅ Error handling untuk setiap request
   - ✅ Support untuk environment variables

3. **Database**
   - ✅ Sudah terkoneksi ke MySQL `absen_nepul`
   - ✅ Semua tabel siap digunakan

---

## 🚀 Cara Menjalankan

### **STEP 1: Setup Backend**

```bash
cd d:\Kerja_raktek\backend

# Install dependencies
pip install -r requirements.txt

# Jalankan backend
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

**Output yang diharapkan:**
```
INFO:     Uvicorn running on http://127.0.0.1:8000
INFO:     Application startup complete
```

### **STEP 2: Setup Frontend**

```bash
cd d:\Kerja_raktek\frontend

# Install dependencies (jika belum)
npm install

# Jalankan development server
npm run dev
```

**Output yang diharapkan:**
```
VITE v5.x.x  ready in xxx ms

➜  Local:   http://127.0.0.1:5173/
```

### **STEP 3: Test Koneksi**

Buka browser dan kunjungi:
- **Backend Test**: `http://127.0.0.1:8000/test-db` → harus menunjukkan `{"db": "connected ✅"}`
- **Frontend**: `http://127.0.0.1:5173` → harus buka tanpa error

---

## 📡 API Endpoints yang Tersedia

### **Authentication**
- `POST /login` - Login user

### **Siswa**
- `GET /students` - Ambil semua siswa
- `GET /students/{nis}` - Ambil data siswa spesifik

### **Guru**
- `GET /teachers` - Ambil semua guru

### **Kelas**
- `GET /classes` - Ambil semua kelas

### **Jadwal Pelajaran**
- `GET /schedules` - Ambil semua jadwal
- `GET /schedules/class/{class_name}` - Ambil jadwal kelas spesifik

### **Absensi**
- `GET /attendance/{nis}` - Riwayat absensi siswa
- `GET /attendance/today` - Absensi hari ini
- `POST /absen/masuk` - Submit absensi masuk
- `POST /absen/keluar` - Submit absensi keluar
- `PUT /attendance/{id_absen}` - Update absensi

### **Form Izin**
- `GET /permission-forms/{nis}` - Ambil form izin siswa
- `POST /permission-forms` - Submit form izin baru

### **Rekap Absensi**
- `GET /attendance-report/class/{class_id}` - Rekap absensi kelas

---

## 💻 Contoh Penggunaan di React Component

```javascript
import { getStudentByNis, getStudentAttendance, getAllSchedules } from './api.js';

export function MyComponent() {
  const [student, setStudent] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // Ambil data siswa
      const siswa = await getStudentByNis('1234567890');
      setStudent(siswa);

      // Ambil absensi siswa
      const absensi = await getStudentAttendance('1234567890');
      console.log('Absensi:', absensi);

      // Ambil jadwal
      const jadwal = await getAllSchedules();
      console.log('Jadwal:', jadwal);
    }

    fetchData();
  }, []);

  return (
    <div>
      {student && <p>Nama: {student.nama_siswa}</p>}
    </div>
  );
}
```

---

## 🔧 Konfigurasi Environment

### **Frontend (.env)**
```
VITE_API_BASE=http://127.0.0.1:8000
```

### **Backend (database.py)**
```python
DATABASE_URL = "mysql+pymysql://root:@localhost/absen_nepul"
```

---

## ⚠️ Troubleshooting

### **Error: "Cannot connect to backend"**
- ✅ Pastikan backend sudah running di port 8000
- ✅ Check CORS di `main.py` sudah benar

### **Error: "No module named 'fastapi'"**
- ✅ Install requirements: `pip install -r requirements.txt`

### **Error: "Database connection failed"**
- ✅ Pastikan MySQL running
- ✅ Pastikan database `absen_nepul` sudah di-import
- ✅ Check username/password di `database.py`

### **Frontend tidak meload data**
- ✅ Buka DevTools → Console untuk melihat error
- ✅ Check API_BASE di `api.js` sudah benar
- ✅ Pastikan backend endpoint tersedia di `/attendance/today`, dll

---

## 📝 File-file yang Diubah/Dibuat

✅ `backend/main.py` - Update include_router
✅ `backend/routes/data.py` - **BARU** - API endpoints
✅ `backend/requirements.txt` - **BARU** - Dependencies
✅ `frontend/src/api.js` - Update dengan 16+ fungsi
✅ `frontend/.env` - Sudah ada konfigurasi

---

## 🎓 Struktur Lengkap Query yang Tersedia

Setiap endpoint mengembalikan response yang terstruktur:

```javascript
// Response sukses
{
  success: true,
  data: [...],
  message: "..."
}

// Response error
{
  success: false,
  message: "Error description"
}
```

---

## ✨ Next Steps (Opsional)

1. **Tambah Authentication Token** - Implementasi JWT
2. **Upload File** - Untuk lampiran form izin
3. **Real-time Updates** - Gunakan WebSocket
4. **Dashboard Statistics** - Chart & graph dengan recharts
5. **Optimasi Query** - Tambah pagination & filtering

---

**Status: ✅ SIAP DIGUNAKAN**

Semua endpoint sudah dikonfigurasi dan siap menerima request dari frontend!

