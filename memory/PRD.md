# Rahaza Travel ERP — PRD / Catatan Lanjutan (E1)

## Problem statement asli
Lanjutkan development repo https://github.com/pandeyoga/travelhost. Cek penyimpanan foto galeri/media, error,
gambar tidak terload, stabilitas sistem. (Sesi ini fokus ke bug yang dilaporkan user via screenshot.)

## Arsitektur
FastAPI (backend/, routers + services) · React CRA + shadcn (frontend/) · MongoDB. ERP di /app/*, situs publik di /.
Deploy: Caddy (deploy/render_caddyfile.sh) — ERP bisa di subdomain terpisah (ERP_DOMAIN → `redir / /app/login`).
Seed demo: `python scripts/seed_data.py` (owner/ops/marketing/driver @demo.local, pass demo12345).

## Yang dikerjakan 12 Sep 2026
1. Setup repo di /app, deps terpasang (emergentintegrations/litellm pin konflik → di-skip dari install, tidak dipakai runtime).
2. CMS → Halaman: dari 3 → 9 halaman (home, about, contact, fleet, destinations, packages, promo, blog, trip-calculator);
   6 halaman baru punya section `page_hero` via komponen `CmsPageHero`.
3. Pratinjau CMS menampilkan ERP (bukan situs) saat ERP di subdomain: iframe kini pakai base URL situs publik
   (Pengaturan Situs → `site_url`, fallback env PUBLIC_SITE_URL) + postMessage cross-origin via `?pbOrigin=`.
4. Manajemen User: peran `marketing_admin` tampil di dropdown; edit user, reset sandi, nonaktifkan, hapus
   (DELETE /api/users/{id} dgn guard diri sendiri & owner terakhir).
5. Master Driver: field Status & Rating manual dihapus (status otomatis dari trip). Tambah tautan akun login
   (drivers.user_id): pilih akun driver yang ada / buat akun baru; kolom "Akun Login"; GET /api/drivers/accounts.
6. Master Armada: dropdown status operasional diganti "Status Unit" (Aktif/Nonaktif); on_trip/maintenance otomatis
   dan ditolak backend bila dikirim dari master data. Verifikasi sinkronisasi trip/maintenance/publik OK.
Testing: test_reports/iteration_3.json & iteration_4.json — semua lulus.

## Backlog / P1
- Audit penyimpanan foto Media Library (permintaan awal user): cek media_store.py (disk lokal), URL gambar, orphan.
- Rating driver: belum ada sumber data (ulasan per driver) — field disembunyikan; bisa dihitung dari testimoni/trip.
- FAQ/CTA per halaman daftar (fleet/destinations) belum bisa di-override dari builder (hanya hero).
