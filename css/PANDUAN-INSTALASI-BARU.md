# PANDUAN INSTALASI BARU: Apk Sekolah Mantap v4.7
Untuk memasang aplikasi sebagai **aplikasi baru** (proyek Apps Script, database, dan situs GitHub Pages sendiri). Data lama tidak ikut dan tidak terpengaruh.

**Cara kerja:** data tersimpan di Google Sheets; `Kode.gs` berjalan di Google Apps Script sebagai server; tampilan (HTML/CSS/JS) dipasang di GitHub Pages.

## Yang disiapkan
- Akun Google (disarankan akun khusus sekolah) dan akun GitHub (gratis).
- `Kode.gs` (berkas `Kode_-_Copy.gs`, ganti namanya menjadi `Kode.gs`).
- Folder tampilan hasil ekstrak `pembaruan-v4.7.zip` ditambah `js/config.js` Anda.

Struktur folder tampilan yang benar (`index.html` harus di akar):
```
index.html
css/style.css
js/config.js  app.js  games.js  tools.js  modules.js  pages.js
js/kenaikan-kelas.js  v41.js  nilai-grid.js  bais.js  games4.js  v43.js  v45.js
```
Berkas `ai.js` sudah tidak dipakai; jangan diunggah.

---
## TAHAP 1: Pasang server (Google Apps Script)
1. Buka https://script.google.com → **Proyek baru**. Beri nama, mis. "Apk Sekolah Mantap".
2. Hapus isi `Code.gs`, tempel seluruh isi `Kode.gs`, ganti nama file menjadi **Kode**, lalu simpan (Ctrl+S).
3. Pilih fungsi **`setupAppEnvironment`** → **Run**.
4. Saat Google meminta izin (Drive, Sheets, Gmail, Docs): **Tinjau izin** → pilih akun → *Lanjutan* → *Buka (tidak aman)* → **Izinkan**. Ini normal untuk skrip buatan sendiri.
5. Tunggu 1–2 menit sampai log menulis **"Setup selesai"** (lihat di menu *Eksekusi*). Proses ini membuat database Google Sheets baru, folder Drive, sekitar 60 sheet, akun Admin, dan akun demo.
6. Jalankan **`setupTurbo`** sekali (pemanas server tiap 5 menit; bisa dimatikan dengan `matikanKeepWarm`).
7. **Deploy → Deployment baru → ikon roda gigi → Aplikasi web**:
   - *Jalankan sebagai*: **Saya**
   - *Yang memiliki akses*: **Siapa saja**
   - Klik **Deploy**, lalu **salin URL** yang berakhiran `/exec`.

## TAHAP 2: Isi alamat server di tampilan
1. Buka `js/config.js` dengan Notepad.
2. Isi `GAS_URL` dengan URL `/exec` tadi, tetap di dalam tanda kutip:
   `const GAS_URL = 'https://script.google.com/macros/s/XXXX/exec';`
3. Simpan.

## TAHAP 3: Pasang tampilan di GitHub Pages
1. Di https://github.com buat **repository baru** (mis. `apk-sekolah`), tipe **Public**.
2. Cara termudah: **Add file → Upload files**, seret **isi** folder tampilan (bukan foldernya), lalu **Commit changes**.
   Cara perintah:
   ```
   cd folder-tampilan
   git init
   git add .
   git commit -m "Apk Sekolah Mantap v4.7"
   git branch -M main
   git remote add origin https://github.com/USERNAME/apk-sekolah.git
   git push -u origin main
   ```
   (password = Personal Access Token GitHub)
3. **Settings → Pages** → *Source*: **Deploy from a branch**, *Branch*: `main` / `(root)` → **Save**. Centang **Enforce HTTPS**.
4. Tunggu 1–2 menit; alamat situs muncul: `https://USERNAME.github.io/apk-sekolah/`.

HTTPS wajib agar kamera (scan QR dan presensi wajah) dapat dipakai.

## TAHAP 4: Masuk pertama kali dan pengaturan awal
1. Buka alamat situs → klik **Masuk** di landing page → login Admin: username **`ujangilham`**, password awal **`ujangilham`**.
2. **Segera ganti password**: *Administrasi & Sistem → Pengaturan → Ganti Password* (minimal 8 karakter).
3. **Pengaturan → Identitas Sekolah**: isi nama sekolah, alamat, kota, nama dan NIP Kepala Sekolah, NSS, NPSN, baris kop surat, logo, dan URL situs (GitHub Pages).
4. **Data Master → Tahun Ajaran**: pastikan tahun ajaran aktif benar.
5. Isi data inti berurutan: **Guru** (impor CSV tersedia) → **Kelas** (isi Wali Kelas) → **Siswa** (isi *No. HP Orang Tua*) → **Mata Pelajaran**.
6. **Pengguna & Hak Akses**: buat akun guru/staf (peran dan kolom *Kelas*). Untuk banyak guru sekaligus, gunakan tombol **Akun Guru Input Nilai**.
7. Admin: **Data Siswa → Sinkronkan Akun Orang Tua** (sekali) agar akun orang tua dibuat dari nomor HP (username = password awal = nomor HP).
8. Akun **demo** (`demo` / `demo1234`) hanya bisa melihat. Nonaktifkan di menu Pengguna bila tidak dipakai.

Nama mapel penting untuk hak akses nilai: mengandung "agama", "PAI", atau "budi pekerti" dikenali sebagai PAI; "PJOK", "jasmani", "olahraga", atau "penjas" sebagai PJOK.

## TAHAP 5: Opsional
- **WhatsApp otomatis:** daftar di fonnte.com, sambungkan nomor sekolah, lalu di Pengaturan pilih "Aktif" dan tempel token. Tanpa itu, wali kelas memakai tombol "Kirim" setelah presensi.
- **Soal AI untuk Kuis Bais:** Game → Kuis Bais → *Atur soal AI* (khusus Admin), tempel Gemini API key. Tanpa itu, game memakai bank soal bawaan.
- **Kartu pelajar:** menu Kartu Pelajar & QR Massal, cetak A4 skala 100%.
- **Backup rutin:** menu Backup & Hapus Data → Buat Backup Sekarang.

## Uji cepat (5 menit)
1. Login Admin → Siswa → **Impor CSV** (`contoh/siswa-dummy-1A.csv` bila tersedia di paket Anda).
2. Akademik → **Scan QR Presensi** → tandai status → Simpan.
3. Akademik → **Laporan Kehadiran** → pilih periode dan kelas → Cetak.
4. Keluar, login sebagai `demo`: tombol Tambah/Hapus tidak muncul.
5. Cek Guru Pintar: menu Generator RPP/Modul Ajar, Generator Soal (AI), dan Generator LKPD tidak ada (memang dihapus di v4.7).

## Setelah mengubah Kode.gs
**Deploy → Kelola deployment → pensil → Versi baru → Deploy** (URL `/exec` tetap). Bila ada perubahan struktur data, jalankan `setupAppEnvironment` dulu.

## Masalah umum
| Gejala | Solusi |
|---|---|
| "GAS_URL belum diisi" | Isi `js/config.js`, unggah ulang, Ctrl+Shift+R |
| "Sheet … belum ada" | Jalankan ulang `setupAppEnvironment`, lalu Deploy Versi baru |
| Halaman 404 di GitHub Pages | `index.html` harus di akar repository; tunggu 1–2 menit |
| Kamera ditolak | Ikon gembok di address bar → Izinkan kamera → muat ulang |
| Login gagal / "Tidak dapat terhubung" | Akses deployment harus **Siapa saja** dan URL berakhiran `/exec` |
| Tombol Masuk tidak membuka kotak login | Pastikan `js/app.js` v4.7 terunggah, lalu Ctrl+Shift+R |
| Perubahan tidak terlihat | Ctrl+Shift+R |
