# HijiForge

Generator kredensial offline. Satu file HTML, tanpa dependensi, berjalan sepenuhnya di browser.

## Tentang nama

"Hiji" dari bahasa Sunda berarti satu. "Forge" berarti menempa. "Fuse" adalah tempat di mana username dan password yang ditempa di tempat berbeda dilebur jadi satu kredensial utuh. Satu file, satu tempat, satu hasil.

## Fitur

### Username
- Username acak, panjang 6–50 karakter
- Pilihan huruf kecil, huruf besar, dan angka kustom
- Opsi exclude ascender (`b d f h i j k l t`) dan descender (`g j p q y`)
- Opsi unik dalam satu batch
- Minimal jumlah karakter per tipe
- Regenerate per item tanpa mengubah item lain

### Password
- Password acak, panjang 8–50 karakter
- Pilihan huruf kecil, huruf besar, angka, simbol
- Opsi exclude karakter mirip (`0 O 1 l I |`)
- Opsi wajib ada satu karakter dari tiap tipe aktif
- Rating kekuatan berdasarkan entropi (indikator warna, tanpa label teks)

### Fuse
- Gabungkan username dan password jadi satu kredensial per platform
- Mode Existing: pakai hasil dari tab Username dan Password. Tombol berlabel "Combine". Tidak ada tombol regenerate per field, karena sumbernya mirror dari tab lain.
- Mode Randomize: buat baru langsung, bisa pakai username manual, atau tanpa username/password. Tombol berlabel "Generate Fused". Konfirmasi muncul jika sudah ada hasil di layar.
- Metadata: nama platform, nama depan/belakang, email, telepon
- Copy, regenerate (mode Randomize), atau hapus per field
- Filter berdasarkan nomor urut atau teks
- Indikator perubahan per field (hanya saat mode update)

### Update
Sub-navigasi dua mode:

- **Backup**: simpan seluruh fused credentials ke file `.hf` terenkripsi. Password diminta saat membuat backup.
- **Update**: muat file `.hf`, pilih field yang mau diupdate (Metadata, Email, Telephone, Username, Password, dan label extra jika ada), preview sebelum diterapkan, dan edit manual per field. Password file diminta saat memuat; jika salah, muncul border merah dan bisa retry tanpa memilih ulang file.

Tombol "Generate New Credentials" muncul hanya jika ada field (username atau password) yang tidak di-lock sesuai checkbox di preview.

### Extra
Tambahkan data tambahan per fused credential (misal label PIN dengan tipe Number, atau catatan dengan tipe Text).

- Pilih target number, tipe, label, dan value lalu klik Apply
- "Backup Selected" membackup hanya credential target, dan otomatis meng-apply form jika label dan value terisi
- Data extra muncul di kartu fuse, ikut ke backup, export CSV/TXT, dan print PDF
- Data extra bisa diedit di mode update (via tombol sliders)

### Umum
- Dark mode (ikut sistem atau manual)
- Header auto-hide saat scroll ke bawah (tampil kembali saat scroll ke atas atau mendekati atas halaman)
- Copy dengan auto-clear 60 detik
- Export CSV / TXT
- Print PDF
- Auto-refresh setelah 15 menit tidak aktif
- Navigasi keyboard dan ARIA

## Cara pakai

Buka `index.html` di browser modern. Tidak perlu instalasi, tidak perlu internet.

Atau akses lewat GitHub Pages: https://xvnurwzesc.github.io/hijiforge/

## Backup

File backup berakhiran `.hf` berisi fused credentials yang dienkripsi dengan AES-GCM 256-bit dan kunci yang diturunkan lewat PBKDF2-SHA256 600.000 iterasi.

Password backup diminta dari pengguna saat membuat maupun saat memuat file. Tanpa password yang benar, isi file tidak bisa dibaca.

File backup versi lama (format v1, sebelum ada password pengguna) masih bisa dimuat untuk kompatibilitas, tapi tidak lagi dibuat oleh versi ini.

Salt (16 byte) dan IV (12 byte) divalidasi saat memuat untuk menolak file yang rusak atau tidak sesuai format. Iterasi PBKDF2 dibatasi maksimum 2.000.000 untuk mencegah file berbahaya membuat browser hang.

Simpan file backup `.hf` di tempat yang aman.

## Keamanan

- Semua pembuatan kredensial dilakukan lokal memakai `crypto.getRandomValues` dan `crypto.subtle`
- Tidak ada koneksi keluar (CSP `connect-src 'none'`)
- `localStorage` hanya menyimpan preferensi UI dan input form
- `sessionStorage` hanya untuk sesi update yang sedang berjalan dan preview backup yang belum di-load
- Password hasil generate tidak disimpan permanen
- Clipboard otomatis dikosongkan 60 detik setelah copy
- Halaman otomatis refresh setelah 15 menit tidak aktif untuk melindungi kredensial yang tampil di layar

## Catatan desain

Aplikasi ini sengaja dibuat sederhana. Tidak banyak peringatan, tidak banyak konfirmasi. Kalau sebuah proses gagal atau tombol tidak merespons, biasanya karena ada input yang tidak sesuai aturan.

Pengecualian yang memang disengaja: konfirmasi tetap muncul untuk aksi destruktif (hapus credential, reset data, cancel mode update) dan untuk generate di mode Randomize saat sudah ada hasil di layar. Field yang invalid diberi border merah (misal email tidak valid atau password backup salah).
