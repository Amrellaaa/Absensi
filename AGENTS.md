# Absensi Siswa — Agent Guide

## Project Structure
- **`backend/`** — FastAPI (Python 3.13), MySQL via SQLAlchemy raw SQL, no ORM models beyond `Attendance`
- **`frontend/`** — React 19 + Vite 8 + Tailwind CSS 4 + React Router 7 (JSX, no TypeScript)

## Commands
| Action | Command |
|--------|---------|
| Backend | `cd backend; pip install -r requirements.txt; uvicorn main:app --reload --host 127.0.0.1 --port 8000` |
| Frontend dev | `cd frontend; npm run dev` |
| Frontend build | `npm run build` |
| Lint | `npm run lint` (ESLint only, no typecheck) |

## Architecture
- **No auth/JWT** — Login is plain SQL matching; routes are unprotected. Student: NISN + NIS as pw. Admin: `admin_data` table. Guru: NIP + password.
- **No tests** exist in the repo.
- **Database**: MySQL `absen_nepul`, raw SQL queries throughout. Only `student_attendance_data` has a SQLAlchemy model (`backend/models/model.py`).
- **Two duplicate `/students/import` endpoints** — one in `routes/student_import.py` (validates columns), one in `routes/data.py` (no validation). Prefer `student_import`.
- **Two distance-calculation utilities** — `utils/location.py` (used by absensi) and `services/gps.py` (unused/redundant).

## GPS Attendance (absensi.py)
- School coords: `-6.2819598, 106.7226564`
- Max radius: 50m; max GPS accuracy: 18m (fake GPS detection)
- Check-in window: 06:45–07:45. Before 07:00 → hadir, 07:00–07:45 → telat, after → alfa
- Check-out: after class `jam_pulang` from `kelas` table

## Frontend Routes (App.jsx)
- Students: `/`, `/login`, `/home`, `/profile`, `/permission`, `/schedule`, `/history`, `/absen/:type`, `/notifications`
- Teachers: `/guru/login`, `/guru/home`, `/guru/profile`, `/guru/notifications`, `/guru/rekap`, `/guru/pengajuan`, `/guru/jadwal`
- Admin: `/admin/login`, `/admin/home`, `/admin/profile`, `/admin/notifications`, `/admin/rekap`, `/admin/jadwal`, `/admin/jadwal/edit/:id`, `/admin/data-guru`, `/admin/data-guru/edit/:nip`, `/admin/data-siswa`, `/admin/data-siswa/edit/:id`, `/admin/pengajuan`, `/admin/reset-password`, `/admin/history-absensi`
- **Duplicate route**: `/guru/notifications` registered twice in App.jsx lines 69 and 73.

## Key Gotchas
- Env: `VITE_API_BASE=http://127.0.0.1:8000` (frontend `.env`)
- DB: `mysql+pymysql://root:@localhost/absen_nepul` (backend `database.py`)
- CORS whitelist: only `localhost:5173` and `127.0.0.1:5173`
- WhatsApp: Fonnte API, hardcoded token in `utils/whatsapp.py`
- Face verify (`utils/face_verify.py`) requires `face_recognition` lib (not in requirements.txt)
- No `package-lock.json` changes needed unless adding deps; `node_modules/` is committed
- Permission form: POST to `/izin/submit`, GET from `/permission-forms/{nis}` (different path patterns)
- Admin notifications use `/admin/notifications` (check for `menunggu` or null status in `data_form_izin`)

## File Conventions
- Backend: raw SQL via `text()`, manual `commit`/`rollback`, some routes use `get_db` dependency, others open `SessionLocal()` directly
- Frontend: all API calls in `src/api.js`, each returns `{success, ...}` or `[]` on error
- Tailwind pastel color palette defined in `tailwind.config.js` under `colors.pastel`
