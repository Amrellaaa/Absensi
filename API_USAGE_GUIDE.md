# 📚 Referensi Fungsi API Frontend

Dokumen ini menunjukkan cara menggunakan semua fungsi API yang sudah disediakan di `src/api.js`

---

## 🔐 **1. Authentication**

### `loginUser(nisn, password)`
Login user (siswa, guru, atau admin)

```javascript
import { loginUser } from './api.js';

async function handleLogin() {
  const result = await loginUser('1234567890', 'password123');
  
  if (result.success) {
    console.log('Role:', result.role); // admin, guru, siswa
    console.log('Name:', result.name);
    localStorage.setItem('userRole', result.role);
  } else {
    alert(result.message);
  }
}
```

---

## 👥 **2. Data Siswa**

### `getAllStudents()`
Ambil daftar semua siswa

```javascript
import { getAllStudents } from './api.js';

async function loadStudents() {
  const students = await getAllStudents();
  // Returns: [{ nis, nisn, nama_siswa, id_kelas, ... }]
  console.log(students);
}
```

### `getStudentByNis(nis)`
Ambil data siswa spesifik

```javascript
import { getStudentByNis } from './api.js';

async function loadStudent() {
  const student = await getStudentByNis('1234567890');
  // Returns: { nis, nisn, nama_siswa, id_kelas, wali_kelas, alamat, no_wa_siswa, no_wa_ortu, tanggal_lahir }
  console.log('Nama:', student.nama_siswa);
}
```

---

## 👨‍🏫 **3. Data Guru**

### `getAllTeachers()`
Ambil daftar semua guru

```javascript
import { getAllTeachers } from './api.js';

async function loadTeachers() {
  const teachers = await getAllTeachers();
  // Returns: [{ NIP, Nama, Tanggal_lahir, Alamat, Guru_Pengampu, Nomor_Telepon }]
  teachers.forEach(guru => {
    console.log(guru.Nama, '-', guru.Guru_Pengampu);
  });
}
```

---

## 📚 **4. Data Kelas**

### `getKelas()`
Ambil daftar semua kelas

```javascript
import { getKelas } from './api.js';

async function loadClasses() {
  const classes = await getKelas();
  // Returns: [{ id, nama_kelas, wali_kelas, jam_pulang }]
  classes.forEach(kelas => {
    console.log(kelas.nama_kelas, 'Wali:', kelas.wali_kelas);
  });
}
```

---

## 📅 **5. Jadwal Pelajaran**

### `getAllSchedules()`
Ambil semua jadwal pelajaran

```javascript
import { getAllSchedules } from './api.js';

async function loadAllSchedules() {
  const schedules = await getAllSchedules();
  // Returns: [{ id_mapel, mapel, jam, hari, guru, kelas }]
  console.log(schedules);
}
```

### `getSchedulesByClass(className)`
Ambil jadwal untuk kelas spesifik

```javascript
import { getSchedulesByClass } from './api.js';

async function loadClassSchedule() {
  const schedules = await getSchedulesByClass('10-1');
  // Returns: [{ id_mapel, mapel, jam, hari, guru, kelas }]
  schedules.forEach(jadwal => {
    console.log(`${jadwal.hari} - ${jadwal.jam}: ${jadwal.mapel} (${jadwal.guru})`);
  });
}
```

---

## ⏱️ **6. Absensi**

### `absenMasuk(nis, latitude, longitude, accuracy)`
Submit absensi masuk dengan lokasi GPS

```javascript
import { absenMasuk } from './api.js';

async function handleCheckIn() {
  // Dapatkan GPS location dari browser
  navigator.geolocation.getCurrentPosition(async (position) => {
    const { latitude, longitude, accuracy } = position.coords;
    
    const result = await absenMasuk('1234567890', latitude, longitude, accuracy);
    
    if (result.success) {
      console.log('Absen masuk berhasil!');
      console.log('Status:', result.data.status); // hadir, telat, atau alfa
      console.log('Jam:', result.data.jam_masuk);
    } else {
      alert('Error: ' + result.message);
    }
  });
}
```

### `absenKeluar(nis, latitude, longitude, accuracy)`
Submit absensi keluar dengan lokasi GPS

```javascript
import { absenKeluar } from './api.js';

async function handleCheckOut() {
  navigator.geolocation.getCurrentPosition(async (position) => {
    const { latitude, longitude, accuracy } = position.coords;
    
    const result = await absenKeluar('1234567890', latitude, longitude, accuracy);
    
    if (result.success) {
      console.log('Absen keluar berhasil!');
    } else {
      alert('Error: ' + result.message);
    }
  });
}
```

### `getStudentAttendance(nis)`
Ambil riwayat absensi siswa (30 hari terakhir)

```javascript
import { getStudentAttendance } from './api.js';

async function loadAttendanceHistory() {
  const attendance = await getStudentAttendance('1234567890');
  // Returns: [{ id_absen, nis, nama_siswa, tanggal, jam_masuk, jam_keluar, status_kehadiran, keterangan, latitude, longitude }]
  
  attendance.forEach(absen => {
    console.log(`${absen.tanggal} - ${absen.status_kehadiran}`);
  });
}
```

### `getTodayAttendance()`
Ambil data absensi hari ini untuk semua siswa

```javascript
import { getTodayAttendance } from './api.js';

async function loadTodayAttendance() {
  const today = await getTodayAttendance();
  // Returns: [{ id_absen, nis, nama_siswa, tanggal, jam_masuk, jam_keluar, status_kehadiran }]
  console.log(`Siswa hadir hari ini: ${today.length}`);
}
```

---

## 📋 **7. Admin Dashboard**

### `getAdminSummary()`
Ambil ringkasan data untuk dashboard admin

```javascript
import { getAdminSummary } from './api.js';

async function loadDashboard() {
  const summary = await getAdminSummary();
  // Returns: { total_students, attendance_today, total_classes }
  
  console.log('Total Siswa:', summary.total_students);
  console.log('Hadir Hari Ini:', summary.attendance_today);
  console.log('Total Kelas:', summary.total_classes);
}
```

---

## 📄 **8. Form Izin**

### `getPermissionForms(nis)`
Ambil daftar form izin siswa

```javascript
import { getPermissionForms } from './api.js';

async function loadPermissionForms() {
  const forms = await getPermissionForms('1234567890');
  // Returns: [{ nis, nama_siswa, tanggal, alasan, status, lampiran }]
  
  forms.forEach(form => {
    console.log(`${form.tanggal} - ${form.alasan} (${form.status})`);
  });
}
```

### `submitPermissionForm(nis, nama_siswa, tanggal, alasan, lampiran)`
Submit form izin baru

```javascript
import { submitPermissionForm } from './api.js';

async function submitForm() {
  const result = await submitPermissionForm(
    '1234567890',
    'Ahmad Rizky',
    '2026-05-13',
    'Sakit demam',
    'scan_surat_dokter.pdf' // optional
  );
  
  if (result.success) {
    console.log('Form izin berhasil diajukan!');
  } else {
    alert(result.message);
  }
}
```

---

## 📊 **9. Rekap Absensi**

### `getAttendanceReport(classId, startDate, endDate)`
Ambil rekap absensi kelas dalam periode tertentu

```javascript
import { getAttendanceReport } from './api.js';

async function loadAttendanceReport() {
  const report = await getAttendanceReport(
    1,  // class_id
    '2026-05-01',  // start_date (optional)
    '2026-05-31'   // end_date (optional)
  );
  
  // Returns: [{ nis, nama_siswa, hadir, telat, alfa, total }]
  report.forEach(siswa => {
    console.log(`${siswa.nama_siswa}: Hadir=${siswa.hadir}, Telat=${siswa.telat}, Alfa=${siswa.alfa}`);
  });
}
```

---

## 🔄 **10. Update Absensi**

### `updateAttendance(id_absen, jam_keluar)`
Update jam keluar absensi

```javascript
import { updateAttendance } from './api.js';

async function updateAttendanceRecord() {
  const result = await updateAttendance(
    7,  // id_absen
    '14:30:00'  // jam_keluar
  );
  
  if (result.success) {
    console.log('Absensi berhasil diupdate!');
  }
}
```

---

## 🎯 **Contoh Lengkap: Halaman Absen Siswa**

```javascript
import React, { useState, useEffect } from 'react';
import { absenMasuk, absenKeluar, getStudentAttendance, getStudentByNis } from '../api.js';

export function AbsenPage() {
  const [student, setStudent] = useState(null);
  const [attendance, setAttendance] = useState([]);
  const [loading, setLoading] = useState(false);

  const nis = '1234567890'; // Dari login

  useEffect(() => {
    loadData();
  }, []);

  async function loadData() {
    const siswa = await getStudentByNis(nis);
    setStudent(siswa);

    const absensi = await getStudentAttendance(nis);
    setAttendance(absensi);
  }

  async function handleAbsenMasuk() {
    setLoading(true);
    
    navigator.geolocation.getCurrentPosition(async (position) => {
      const { latitude, longitude, accuracy } = position.coords;
      const result = await absenMasuk(nis, latitude, longitude, accuracy);

      if (result.success) {
        alert(`Absen Masuk Berhasil! Status: ${result.data.status}`);
        loadData(); // Refresh data
      } else {
        alert(`Error: ${result.message}`);
      }
      
      setLoading(false);
    });
  }

  async function handleAbsenKeluar() {
    setLoading(true);
    
    navigator.geolocation.getCurrentPosition(async (position) => {
      const { latitude, longitude, accuracy } = position.coords;
      const result = await absenKeluar(nis, latitude, longitude, accuracy);

      if (result.success) {
        alert('Absen Keluar Berhasil!');
        loadData(); // Refresh data
      } else {
        alert(`Error: ${result.message}`);
      }
      
      setLoading(false);
    });
  }

  return (
    <div className="p-6">
      <h1>Halaman Absensi</h1>
      
      {student && <p>Nama: {student.nama_siswa}</p>}

      <button onClick={handleAbsenMasuk} disabled={loading}>
        {loading ? 'Loading...' : 'Absen Masuk'}
      </button>

      <button onClick={handleAbsenKeluar} disabled={loading}>
        {loading ? 'Loading...' : 'Absen Keluar'}
      </button>

      <h2>Riwayat Absensi</h2>
      <table>
        <thead>
          <tr>
            <th>Tanggal</th>
            <th>Jam Masuk</th>
            <th>Jam Keluar</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {attendance.map(absen => (
            <tr key={absen.id_absen}>
              <td>{absen.tanggal}</td>
              <td>{absen.jam_masuk}</td>
              <td>{absen.jam_keluar}</td>
              <td>{absen.status_kehadiran}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
```

---

## ✅ Checklist Implementasi

- [ ] Backend running di `http://127.0.0.1:8000`
- [ ] Frontend running di `http://127.0.0.1:5173`
- [ ] `.env` file sudah dikonfigurasi dengan `VITE_API_BASE`
- [ ] Bisa login dengan `nisn=admin, password=admin`
- [ ] Bisa fetch data dari `/students`, `/classes`, dll
- [ ] Absen masuk/keluar berfungsi dengan GPS
- [ ] Form izin bisa disubmit
- [ ] Dashboard admin menampilkan data

---

**Selamat! Semua API sudah siap digunakan di frontend Anda! 🎉**

