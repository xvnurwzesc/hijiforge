# HijiForge

Generator kredensial offline. Satu file HTML, tanpa dependensi, berjalan sepenuhnya di browser.

## Tentang nama

"Hiji" dari bahasa Sunda berarti satu. "Forge" berarti menempa. "Fuse" adalah tempat di mana username dan password yang ditempa di tempat berbeda dilebur jadi satu kredensial utuh. Satu file, satu tempat, satu hasil.

## Fitur

### HijiUser
- Username acak, panjang 6–50 karakter
- Pilihan huruf kecil, huruf besar, dan angka kustom
- Opsi exclude ascender (`b d f h i j k l t`) dan descender (`g j p q y`)
- Opsi unik dalam satu batch
- Minimal jumlah karakter per tipe

### HijiPass
- Password acak, panjang 8–50 karakter
- Pilihan huruf kecil, huruf besar, angka, simbol
- Opsi exclude karakter mirip (`0 O 1 l I |`)
- Opsi wajib ada satu karakter dari tiap tipe aktif
- Rating kekuatan berdasarkan entropi

### HijiFuse
- Gabungkan username dan password jadi satu kredensial per platform
- Mode Existing: pakai hasil dari HijiUser dan HijiPass
- Mode Randomize: buat baru langsung, bisa pakai username manual, atau tanpa username/password
- Metadata: nama platform, nama depan/belakang, email, telepon
- Copy, regenerate, atau hapus per field
- Filter berdasarkan nomor urut atau teks
- Mode update: muat file backup, pilih field yang mau diupdate, preview sebelum diterapkan, edit manual per field dengan regenerate yang mengikuti format
- Indikator perubahan per field

### Umum
- Dark mode (ikut sistem atau manual)
- Copy dengan auto-clear 60 detik
- Export CSV / TXT
- Print PDF
- Auto-refresh setelah 15 menit tidak aktif
- Navigasi keyboard dan ARIA

## Cara pakai

Buka `index.html` di browser modern. Tidak perlu instalasi, tidak perlu internet.

Atau akses lewat GitHub Pages: https://xvnurwzesc.github.io/hijiforge/

## Backup

File backup berakhiran `.hf` berisi fused credentials yang dienkripsi dengan AES-GCM 256-bit dan kunci yang diturunkan lewat PBKDF2. Tujuannya agar isi file tidak bisa dibaca langsung lewat text editor.

Backup ini tidak meminta passphrase dari pengguna. Kunci enkripsi bersifat tetap di dalam source code. Siapa pun yang membaca source dapat mendekripsi file backup. Simpan file backup `.hf` ditempat yang aman.

## Keamanan

- Semua pembuatan kredensial dilakukan lokal memakai `crypto.getRandomValues` dan `crypto.subtle`
- Tidak ada koneksi keluar (CSP `connect-src 'none'`)
- `localStorage` hanya menyimpan preferensi UI dan input form
- `sessionStorage` hanya untuk sesi update yang sedang berjalan
- Password hasil generate tidak disimpan permanen
- Clipboard otomatis dikosongkan 60 detik setelah copy
- Halaman otomatis refresh setelah 15 menit tidak aktif untuk melindungi kredensial yang tampil di layar

## Catatan desain

Aplikasi ini sengaja dibuat sederhana. Tidak banyak peringatan, tidak banyak konfirmasi. Kalau sebuah proses gagal atau tombol tidak merespons, biasanya karena ada input yang tidak sesuai aturan.
