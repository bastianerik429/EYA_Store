# EYA — Web Top Up Game

Tugas kelompok mata kuliah **Pemrograman Web** dengan tema e-commerce: EYA top up
game **Mobile Legends**, **PUBG Mobile**, dan **Roblox**.

Alur pengguna:

```
Beranda → Detail game (pilih 1 dari 3 paket) → Isi data akun → Pembayaran → Top up berhasil
```

> Semua transaksi hanya **simulasi** untuk keperluan tugas kuliah.

---

## 1. Batasan teknis yang dipenuhi

| Ketentuan | Implementasi |
|---|---|
| Hanya HTML5 dan CSS3 | Seluruh halaman `.html` + 4 file CSS di folder `css/` |
| Boleh Bootstrap 5.x | Bootstrap **5.3.3** (CSS) lewat CDN, ditambah Bootstrap Icons (CSS) |
| Dilarang JavaScript | **Tidak ada file/kode JS buatan sendiri.** Satu-satunya JS adalah `bootstrap.bundle.min.js` untuk komponen bawaan Bootstrap (menu lipat/navbar dan FAQ accordion) |
| Responsive (HP, tablet, desktop) | Grid Bootstrap + media queries di `css/responsive.css` |

## 2. Pembagian tugas anggota

| Anggota | Nama /  | Tanggung jawab | Halaman & file yang dikerjakan |
|---|---|---|---|
| 1 | **Erik** /    | Halaman Utama & Header/Footer Global | `index.html`, `cara-topup.html`, `sukses.html`, `css/global.css` (header, footer, tombol, design token) |
| 2 | **Azkia** /   | Halaman Detail/Katalog & Sistem Layout CSS | `katalog.html`, `detail-mlbb.html`, `detail-pubg.html`, `detail-roblox.html`, `css/style.css` (layout & komponen) |
| 3 | **Azriel** /  | Halaman Form/Kontak & Responsivitas Media Queries/Bootstrap | `form-mlbb.html`, `form-pubg.html`, `form-roblox.html`, `pembayaran.html`, `kontak.html`, `css/form.css`, `css/responsive.css` |

Setiap anggota mengerjakan **lebih dari 2 halaman** (Erik 3, Azkia 4, Azriel 5).
Setiap file HTML memuat komentar `Penanggung jawab:` di bagian `<head>`.

## 3. Struktur folder

```
EYA-game/
├── index.html            Beranda
├── cara-topup.html       Panduan + FAQ
├── katalog.html          Daftar game + tabel semua paket
├── detail-mlbb.html      Pilih 3 paket Mobile Legends
├── detail-pubg.html      Pilih 3 paket PUBG Mobile
├── detail-roblox.html    Pilih 3 paket Roblox
├── form-mlbb.html        Isi data akun (User ID + Zone ID)
├── form-pubg.html        Isi data akun (ID Karakter)
├── form-roblox.html      Isi data akun (Username)
├── pembayaran.html       Pilih metode pembayaran
├── sukses.html           Top up berhasil
├── kontak.html           Form kontak
├── css/
│   ├── global.css        Token desain, navbar, footer, tombol   (Erik)
│   ├── style.css         Layout & komponen katalog              (Azkia)
│   ├── form.css          Form, stepper, ringkasan pesanan       (Azriel)
│   └── responsive.css    Media queries                          (Azriel)
└── README.md
```

## 4. Cara menjalankan

1. Clone repository ini.
2. Buka `index.html` di browser (klik dua kali). Tidak perlu server.
3. Butuh koneksi internet untuk memuat Bootstrap, ikon, dan font dari CDN.

Bisa juga dipublikasikan lewat **GitHub Pages**: *Settings → Pages → Deploy from a branch → `main` / root*.

## 5. Penjelasan kode penting

Bagian ini bisa dijadikan bahan laporan. Tiap anggota sebaiknya menambahkan penjelasan
dengan bahasanya sendiri untuk file yang ia kerjakan.

### 5.1 Membawa paket yang dipilih antar halaman tanpa JavaScript (Azriel & Azkia)

Tanpa JS, halaman HTML tidak bisa membaca data dari halaman sebelumnya. Kami memakai
tiga fitur HTML/CSS murni:

1. **Hash di URL.** Tombol paket di halaman detail mengarah ke `form-mlbb.html#mlbb-172`.
2. **Selector `:target`.** Elemen kosong `<span id="mlbb-172" class="flow-anchor">` menjadi `:target`
   saat URL memuat `#mlbb-172`. Selector `#mlbb-172:target ~ .row .pick-mlbb-172` lalu
   menampilkan ringkasan dan tombol yang sesuai. Tanda `~` berarti "elemen `.row` yang
   berada sesudah anchor". Lihat komentar di `css/form.css`.
3. **Atribut `formaction`.** Tombol *Lanjut ke pembayaran* memakai
   `formaction="pembayaran.html#mlbb-172"`, sehingga hash ikut terbawa ke halaman berikutnya.

### 5.2 Form dan validasi HTML5 (Azriel)

- Tipe input `email`, `tel`, `text` dengan atribut `required`, `pattern`, `minlength`, `inputmode`.
- `method="get"` supaya form berjalan di situs statis (tanpa server).
- Umpan balik warna merah/hijau memakai CSS `:not(:placeholder-shown):invalid` dan `:valid`.
- Pilihan metode pembayaran adalah `input type="radio"` (kelas Bootstrap `btn-check`) yang
  labelnya di-style menjadi kartu.

### 5.3 Responsivitas (Azriel)

- **Mobile-first**: gaya dasar untuk HP, lalu `@media (min-width: 576px / 768px / 992px / 1200px)`.
- Grid Bootstrap: `col-md-4`, `col-lg-7`, `order-1 order-lg-2`, dan sebagainya.
- Tabel harga dibungkus `.table-responsive` agar bisa digeser di layar kecil.
- Ringkasan pesanan `position: sticky` hanya di desktop (`>= 992px`).

### 5.4 Sistem layout & komponen (Azkia)

- Ritme jarak konsisten lewat kelas `.section` dan `.section-band`.
- Kartu voucher dengan garis putus-putus dan takik tiket memakai pseudo-element
  `::before` / `::after` dan `clip-path`.
- Warna tiap game diatur lewat custom property (`--game`, `--game-soft`, `--game-on`)
  yang diset per kelas `.game-mlbb`, `.game-pubg`, `.game-roblox`.

### 5.5 Header, footer, dan design token (Erik)

- Semua warna, radius, bayangan, dan font disimpan sebagai CSS custom property di `:root`
  (`css/global.css`), sehingga tema bisa diubah dari satu tempat.
- Navbar memakai komponen Bootstrap (`navbar-expand-lg`, `navbar-toggler`) dan menandai
  halaman aktif dengan `class="active"` + `aria-current="page"`.
- Karena tanpa JS/server tidak ada fitur *include*, header dan footer ditulis ulang di
  setiap halaman dengan isi yang sama.

## 6. Keterbatasan (karena aturan tanpa JavaScript)

- Isi form (mis. ID akun) dikirim lewat query string di URL, tetapi **tidak ditampilkan**
  di halaman berikutnya karena membacanya butuh JS. Paket yang dipilih tetap tampil
  berkat teknik `:target` di atas.
- Nomor pesanan, tanggal, dan harga di halaman sukses adalah contoh statis.
- Form kontak memakai `mailto:` sehingga membuka aplikasi email pengguna.

## 7. Panduan kontribusi Git

Supaya kontribusi tiap anggota terlihat di riwayat commit:

```bash
git checkout -b fitur/Erik-beranda      # satu branch per anggota/fitur
git add index.html css/global.css
git commit -m "feat(beranda): tambah hero dan papan harga"
git push origin fitur/Erik-beranda      # lalu buka Pull Request ke main
```

Contoh pesan commit: `feat(katalog): tambah tabel paket`, `fix(form): perbaiki pattern Zone ID`,
`style(responsive): rapikan stepper di layar HP`.

---

Dibuat oleh: Erik, Azkia, Azriel — Tugas Kelompok Pemrograman Web.
