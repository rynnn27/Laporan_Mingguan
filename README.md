# Aplikasi Laporan Kinerja Kepala Tata Usaha — SMK Plus Pelita Nusantara

Aplikasi web sederhana: **Google Sheets** sebagai database, **Google Apps Script** sebagai backend/API, **GitHub Pages** sebagai tampilan (frontend).

## Struktur

- `Code.gs` → ditempel ke Apps Script pada Google Sheet Anda (yang formatnya sama seperti file upload: sheet *Petunjuk*, *Laporan Mingguan*, *Rekap Tahunan*, *Program & Tugas*).
- `index.html` → di-push ke repo GitHub, diaktifkan sebagai GitHub Pages.

Catatan: sheet **Rekap Tahunan** di Google Sheets tidak perlu diisi manual lagi — aplikasi menghitungnya otomatis dari data di **Laporan Mingguan** setiap kali dibuka.

## Langkah 1 — Pasang Backend (Apps Script)

1. Buka file Excel yang sudah diupload sebagai **Google Sheet** (upload ke Google Drive → buka dengan Google Sheets, atau `File > Save as Google Sheets` jika sudah dibuka).
2. Pastikan nama sheet **persis**: `Petunjuk`, `Laporan Mingguan`, `Rekap Tahunan`, `Program & Tugas`.
3. Menu **Extensions > Apps Script**.
4. Hapus kode default, tempel seluruh isi `Code.gs`.
5. Klik ikon disket untuk menyimpan.
6. Klik **Deploy > New deployment**.
   - Klik ikon gerigi, pilih tipe **Web app**.
   - Description: bebas, misal "API Laporan TU".
   - Execute as: **Me**.
   - Who has access: **Anyone**.
   - Klik **Deploy**.
7. Google akan minta izin akses (Authorize access) — pilih akun Anda, klik **Advanced > Go to (nama project) (unsafe)** lalu **Allow**. Ini normal karena scriptnya milik Anda sendiri.
8. Salin **Web app URL** yang muncul (diakhiri `/exec`). Simpan URL ini.

> Setiap kali Anda mengubah isi `Code.gs`, lakukan **Deploy > Manage deployments > Edit (ikon pensil) > New version > Deploy** supaya perubahan aktif di URL yang sama.

## Langkah 2 — Pasang Frontend (GitHub Pages)

1. Buat repository baru di GitHub, misalnya `laporan-tu-pelita-nusantara`.
2. Upload file `index.html` ke repo tersebut (lewat web GitHub: **Add file > Upload files**, atau via git).
3. Buka **Settings > Pages** pada repo tersebut.
4. Pada **Source**, pilih branch `main` dan folder `/root`, klik **Save**.
5. Tunggu 1–2 menit, GitHub akan memberi URL seperti:
   `https://<username-anda>.github.io/laporan-tu-pelita-nusantara/`

## Langkah 3 — Hubungkan Frontend ke Backend

1. Buka URL GitHub Pages Anda.
2. Di bagian atas halaman ("URL Apps Script"), tempel URL `/exec` dari Langkah 1.
3. Klik **Simpan**. Data akan otomatis dimuat dari Google Sheet Anda.

URL ini disimpan di browser (localStorage) sehingga hanya perlu diisi sekali per perangkat/browser.

## Jika muncul "Sheet ... tidak ditemukan"

Artinya nama tab di Google Sheet Anda tidak persis sama dengan yang dicari kode (`Petunjuk`, `Laporan Mingguan`, `Rekap Tahunan`, `Program & Tugas`). Kode terbaru sudah otomatis toleran terhadap beda spasi/huruf besar-kecil, tapi kalau masih gagal:

1. Buka Apps Script (Extensions > Apps Script).
2. Pilih fungsi `cekNamaSheet` di dropdown atas, klik **Run**.
3. Buka **Lihat > Log Eksekusi** — akan tampil daftar nama sheet yang sebenarnya ada.
4. Samakan nama di kode (`SHEET_MINGGUAN`, `SHEET_PROGRAM`, `SHEET_PETUNJUK` di bagian atas `Code.gs`) dengan nama tab yang asli, atau ganti nama tab di sheet.
5. Setelah mengubah `Code.gs`, jangan lupa **Deploy > Manage deployments > Edit > New version > Deploy** supaya perubahan aktif.

## Upload Bukti/Dokumen

Kolom **Bukti/Dokumen** kini punya dua cara:
- **Tempel link manual** — misalnya link file yang sudah ada di Google Drive.
- **Unggah foto langsung** — pilih file (foto/PDF) lalu klik **Unggah**. File otomatis tersimpan ke folder **"Bukti Laporan TU"** di Google Drive akun Anda, dan link-nya otomatis terisi di kolom.

## Fitur

- **Laporan Mingguan** — tambah, edit, hapus laporan kegiatan mingguan; pilih status (Selesai/Proses/Tertunda/Tidak Dilaksanakan).
- **Rekap Tahunan** — otomatis terhitung per bulan (Juli–Juni sesuai Tahun Ajaran di sheet Petunjuk), termasuk persentase capaian dan ringkasan kendala/tindak lanjut.
- **Program & Tugas** — kelola daftar tugas pokok per bidang sebagai referensi.

## Keamanan & Batasan

- Karena "Who has access: Anyone" pada Web App, siapa pun yang tahu URL `/exec` bisa membaca/menulis data lewat API ini. Untuk pemakaian pribadi ini cukup aman selama URL tidak disebar, tapi bukan proteksi login sesungguhnya.
- Jika ke depan perlu multi-user dengan login, beri tahu saya — bisa ditambahkan verifikasi token/PIN sederhana di Apps Script.
- Google Apps Script Web App punya batas kuota (jumlah request per hari) pada akun gratis — untuk pemakaian satu orang, ini jauh di bawah batas.
