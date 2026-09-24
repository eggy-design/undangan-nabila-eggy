# Panduan Pakai Template Undangan Digital (Versi Full Fitur)

File utama: **undangan.html** — satu file, HTML + CSS + JS jadi satu.

Setiap fitur opsional di dalam kode ditandai dengan komentar:
```
FITUR: NAMA FITUR (mulai)
... kode ...
FITUR: NAMA FITUR (selesai)
```
Tandanya sama persis di beberapa tempat: bagian HTML (`<body>`), bagian CSS (`<style>`), dan bagian JS (`<script>`) — kalau fitur itu punya bagian di sana. **Cara hapus fitur apa pun: cari nama fiturnya (Ctrl+F), lalu hapus semua blok yang diapit "(mulai)" sampai "(selesai)" dengan nama itu, di semua bagian tempat nama itu muncul.**

Bagian yang **tidak** bertanda FITUR (Cover dasar, Pembuka, Mempelai, Acara, Penutup) adalah kerangka inti — sebaiknya jangan dihapus, cukup diedit isinya.

---

## 1. Cara ganti data dasar
Sama seperti sebelumnya — cari dan ganti langsung di teks: nama mempelai, tanggal, alamat, foto, dll. Semua teks yang perlu diganti sudah dalam bahasa manusia biasa (bukan kode aneh), tinggal cari dan timpa.

## 2. Konfigurasi cepat (kumpul di satu tempat)
Di paling atas tag `<script>`, ada blok **KONFIGURASI** — isi variabel-variabel ini sesuai acaramu:

| Variabel | Untuk fitur | Isi dengan |
|---|---|---|
| `TANGGAL_ACARA` | Countdown, Simpan Kalender | `"2026-12-12T08:00:00"` |
| `TANGGAL_SELESAI` | Simpan Kalender | Waktu acara selesai |
| `JUDUL_ACARA` | Simpan Kalender | Judul acara |
| `LOKASI_ACARA` | Simpan Kalender | Alamat lengkap |
| `COVER_MODE` | Mode Cover | `"photo"` / `"video"` / `"slide"` |
| `UCAPAN_SHEET_CSV_URL` | Wall of Love | Link CSV Google Sheet (langkah 5) |

---

## 3. Ganti Foto, Lagu, Video, Background

### Ganti Foto
**Ada di:** foto cover, 2 foto mempelai, 6 foto galeri, 3 foto slide (kalau pakai `COVER_MODE = "slide"`).

**Cara A — pakai link online (praktis, tidak perlu upload file bareng HTML):**
1. Upload foto ke Google Drive, Imgur, atau Postimages.
2. Ambil **link langsung ke gambar** (harus berakhiran `.jpg`/`.png`, bukan link halaman preview/viewer). Imgur/Postimages biasanya sudah memberi link format ini langsung; link Google Drive biasa perlu diubah dulu formatnya, jadi Imgur/Postimages lebih gampang.
3. Tempel link itu menggantikan isi `src="..."` pada tag `<img>` yang mau diganti.

**Cara B — file lokal (offline, ditaruh sefolder):**
1. Siapkan file foto, misal `foto-raka.jpg`, taruh di folder yang sama dengan `undangan.html`.
2. Ganti `src="https://..."` jadi `src="foto-raka.jpg"`.
3. Kalau nanti di-upload ke hosting (Netlify/GitHub Pages/Vercel), foto ini harus ikut di-upload dalam folder yang sama.

Titik yang perlu diganti di kode:
- Cover: `<div id="cover-bg-photo"><img src="...">`
- Mempelai: 2 `<img>` di dalam `.mempelai`
- Galeri: 6 `<img>` di dalam `.galeri-grid` (boleh tambah/kurang jumlahnya)
- Slide cover: 3 `<img>` di dalam `<div id="cover-bg-slide">`

### Ganti Lagu (Musik Latar)
Hanya ada 1 cara realistis: **file lokal**, karena butuh format audio, bukan gambar.
1. Siapkan file musik format `.mp3` (pastikan punya hak pakai lagunya — lagu berlisensi/hak cipta sebaiknya dihindari kalau undangan mau disebar publik).
2. Rename file itu jadi `musik.mp3`, taruh di folder yang sama dengan `undangan.html`. Atau, kalau mau nama file lain, cari baris `<source src="musik.mp3" type="audio/mpeg">` dan ganti `musik.mp3` sesuai nama filemu.
3. Selesai — otomatis diputar begitu tombol "Buka Undangan" diklik, dan bisa di-pause/play lewat tombol bulat di pojok kanan bawah.

*Alternatif:* cari musik bebas royalti di YouTube Audio Library atau Pixabay Music, download mp3-nya, lalu ikuti langkah di atas.

### Ganti Video
Ada 2 jenis video, caranya beda:

**a) Video cover** (kalau `COVER_MODE = "video"`) — wajib file lokal, karena video background butuh loading cepat & auto-loop tanpa iklan:
1. Siapkan file `.mp4`, idealnya di bawah 10MB biar tidak berat, taruh sefolder.
2. Ganti `src="video-cover.mp4"` di dalam `<video>` pada `<div id="cover-bg-video">`.

**b) Video Pre-Wedding & Live Streaming** — wajib pakai YouTube (karena keduanya di-embed via `<iframe>`, bukan file langsung):
1. Upload video ke YouTube (bisa disetel "Unlisted" biar tidak muncul di pencarian publik, tapi tetap bisa dibuka lewat undangan).
2. Ambil ID video dari link, contoh: `youtube.com/watch?v=ABC123XYZ` → ID-nya `ABC123XYZ`.
3. Ganti bagian `GANTI_DENGAN_ID_VIDEO` (Pre-Wedding) atau `GANTI_DENGAN_ID_VIDEO_LIVE` (Live) di dalam `src="https://www.youtube.com/embed/..."`.

### Ganti Background
Ada 2 makna "background" di template ini:

**a) Background cover** (halaman pertama sebelum tombol "Buka Undangan"):
- Ikuti langkah **Ganti Foto** di atas untuk `cover-bg-photo`, atau **Ganti Video** untuk `cover-bg-video`, atau ganti 3 foto di `cover-bg-slide` — atur `COVER_MODE` sesuai yang mau dipakai.

**b) Warna background halaman isi** (bukan foto, tapi warna dasar seperti krem yang dipakai sekarang):
1. Cari bagian paling atas `<style>`, ada blok `:root { ... }`.
2. Ganti nilai `--clr-bg: #FAF6EF;` ke kode warna hex lain sesuai selera (misal `#FFFFFF` untuk putih, `#F5F0FA` untuk lavender muda).
3. Warna lain yang bisa ikut disesuaikan di blok yang sama: `--clr-forest` (warna judul/aksen utama), `--clr-gold` (warna aksen emas), `--clr-ink` (warna teks).

---

## 4. Panduan per fitur baru

### Mode Cover (Foto / Video / Slide 3 Foto)
- Ganti nilai `COVER_MODE` ke `"photo"`, `"video"`, atau `"slide"`.
- **Foto**: ganti `src` di `<div id="cover-bg-photo">`.
- **Video**: taruh file video (mp4, idealnya di bawah 10MB biar cepat loading) di folder yang sama, ganti `src="video-cover.mp4"`. Video otomatis dibisukan (browser selalu blokir video bersuara yang autoplay).
- **Slide**: ganti 3 `src` di dalam `<div id="cover-bg-slide">`, otomatis bergantian tiap 4 detik.
- **Hapus fitur ini** (balik ke 1 foto statis biasa): hapus blok `FITUR: MODE COVER` di HTML+CSS+JS, lalu di CSS `#cover` tambahkan `background:url('foto.jpg') center/cover;`.

### Monogram (Inisial ala Logo)
- Ganti teks `R&S` di `<div class="monogram">`.
- **Hapus**: hapus blok `FITUR: MONOGRAM` di HTML dan CSS.

### Turut Mengundang
- Ganti teks di dalam `<div class="turut-list">`. Fitur generik ini juga bisa dipakai untuk acara lain yang melibatkan banyak orang (misalnya upacara adat dengan beberapa peserta) — tinggal tulis nama-namanya di sini.
- **Hapus**: hapus blok `FITUR: TURUT MENGUNDANG` di HTML dan CSS.

### Countdown
- Otomatis mengikuti `TANGGAL_ACARA`.
- **Hapus** (contoh kasusmu): hapus blok `FITUR: COUNTDOWN` di HTML, CSS, dan JS.

### Catatan Khusus (dress code / protokol)
- Ganti teks di `<div class="detail-card">` bagian "Catatan" (di dalam section Acara).
- **Hapus**: hapus blok `FITUR: CATATAN KHUSUS`.

### Sesi Acara / Duplikat Undangan
Bukan fitur terpisah — caranya: copy seluruh blok `<div class="detail-card">...Resepsi...</div>` di section Acara, tempel di bawahnya, ganti judul jadi "Sesi 2" dan waktunya. Untuk undangan yang beda per sesi, tinggal buat 2 file (`undangan-sesi1.html`, `undangan-sesi2.html`) dari template yang sama.

### Rundown Acara
- Ganti/tambah baris `<div class="rundown-item">` sesuai jadwal.
- **Hapus**: hapus blok `FITUR: RUNDOWN ACARA` di HTML dan CSS.

### Love Story
- Ganti/tambah `<div class="story-item">` sesuai kisahmu.
- **Hapus** (contoh kasusmu): hapus blok `FITUR: LOVE STORY` di HTML dan CSS.

### Video Pre-Wedding
- Upload video ke YouTube (bisa "Unlisted" biar tidak muncul di pencarian publik), ambil ID video dari link (`youtube.com/watch?v=INI_ID_NYA`), ganti `GANTI_DENGAN_ID_VIDEO` di `<iframe>`.
- **Hapus**: hapus blok `FITUR: VIDEO PRE-WEDDING` di HTML dan CSS.

### Galeri
- Ganti/tambah/kurangi `<img>` di dalam `.galeri-grid`. Jumlah foto bebas, tidak dibatasi kuota — tinggal tambah baris `<img src="...">` sebanyak yang mau.
- **Hapus**: hapus seluruh blok `FITUR: GALERI` di HTML dan CSS.

### Wall of Love (Buku Tamu Publik) — lanjutan, opsional
Menampilkan ucapan tamu langsung di halaman undangan (bukan cuma tersimpan di form).
1. Buka Google Sheet hasil RSVP/Ucapan → menu **File → Bagikan → Publikasikan ke web**.
2. Pilih sheet yang berisi jawaban ucapan, format **CSV**, klik Publikasikan.
3. Salin link yang muncul, tempel ke `UCAPAN_SHEET_CSV_URL` di bagian KONFIGURASI.
4. Cek urutan kolom di sheet-mu (biasanya: Timestamp, Nama, Ucapan). Sesuaikan `KOLOM_NAMA_INDEX` dan `KOLOM_UCAPAN_INDEX` kalau urutannya beda (0 = kolom pertama).
- **Hapus**: kosongkan `UCAPAN_SHEET_CSV_URL` (fitur otomatis nonaktif dan menampilkan pesan default), atau hapus penuh blok `FITUR: WALL OF LOVE` di HTML, CSS, JS.

### RSVP
- Sama seperti sebelumnya: buat Google Form, ambil link embed, tempel di `src` iframe section RSVP.
- "Laporan RSVP Google Sheet" dari daftar fiturmu itu **otomatis gratis** — setiap Google Form punya tab "Tanggapan" yang bisa dibuka sebagai Google Sheet, tidak perlu kode tambahan.
- **Hapus**: hapus blok `FITUR: RSVP`.

### Ucapan & Doa
- Sama seperti sebelumnya, form terpisah atau digabung dengan RSVP.
- **Hapus**: hapus blok `FITUR: UCAPAN & DOA`.

### Angpao Digital
- Ganti nomor rekening/e-wallet dan nama pemilik di dalam `.angpao-card`. Tambah kartu baru dengan copy blok `.angpao-card`.
- Tombol "Salin" pakai clipboard browser — **hanya berfungsi kalau file sudah di-hosting online (HTTPS)**, tidak jalan kalau dibuka langsung dari file di komputer.
- Ini murni menampilkan info rekening, bukan sistem pembayaran — tamu tetap transfer manual lewat aplikasi bank/e-wallet masing-masing.
- **Hapus**: hapus blok `FITUR: ANGPAO DIGITAL` di HTML, CSS, JS.

### Kontak & Media Sosial
- Ganti link `href` di 3 `<a class="kontak-item">` (Instagram, WhatsApp, Facebook). Untuk WhatsApp pakai format `https://wa.me/62812xxxxxxx` (nomor tanpa angka 0 di depan, pakai kode negara 62).
- **Hapus**: hapus blok `FITUR: KONTAK & MEDIA SOSIAL` di HTML dan CSS.

### QR Code Undangan
- Otomatis membuat QR dari link halaman itu sendiri, pakai layanan gratis `api.qrserver.com` (tidak perlu setup apa pun).
- **Hapus**: hapus blok `FITUR: QR CODE UNDANGAN` di HTML, CSS, JS.

### Live Streaming
- Section ini default **disembunyikan** (`display:none`). Mendekati acara, buat Live di YouTube, ambil ID video, ganti `GANTI_DENGAN_ID_VIDEO_LIVE`, lalu hapus `style="display:none;"` di tag `<section id="live-section">` supaya muncul.
- **Hapus total**: hapus blok `FITUR: LIVE STREAMING`.

### Musik Latar
- Sama seperti sebelumnya, taruh file `musik.mp3` di folder yang sama.
- **Hapus**: hapus blok `FITUR: MUSIK LATAR` di HTML, CSS, JS.

---

## 5. Fitur dari daftarmu yang TIDAK dimasukkan (dan kenapa)
Supaya jujur dan tidak menjanjikan sesuatu yang tidak jalan:

- **Ganti Desain Undangan (mis. dari "Naraya" ke "Sanaya")** — di jasa berbayar ini artinya pindah ke template visual lain yang sudah jadi. Template ini cuma 1 desain; kamu bisa custom warna & font lewat variabel `:root` di paling atas CSS, tapi untuk ganti tata letak/gaya visual sepenuhnya perlu dibuatkan desain baru dari nol.
- **Tambah kuota foto +3/+5/+10** — ini konsep paket berbayar di jasa lain. Di template ini kamu bebas menambah foto sebanyak apa pun di Galeri, tanpa batas, tanpa biaya.
- **Paket Full Fitur** — bukan fitur teknis, cuma nama paket harga. Di sini semua fitur sudah tersedia sekaligus secara modular, tinggal pilih mana yang dipakai.
- **Custom Inisial gaya logo tertentu** — sudah ada versi sederhananya (fitur Monogram, teks dalam lingkaran). Untuk gaya logo yang lebih rumit/artistik butuh dibuatkan sebagai gambar (misalnya lewat desainer atau AI image generator), lalu tinggal dipasang sebagai gambar biasa menggantikan monogram teks.
- **Tambah peserta mepandes** — istilah spesifik acara adat Bali (upacara potong gigi). Digeneralisasi jadi fitur Turut Mengundang yang bisa dipakai untuk mencantumkan peserta/nama tambahan di acara apa pun.

---

## 6. Upload supaya dapat link (gratis)
Tetap sama seperti sebelumnya — pilih salah satu:
- **Netlify Drop**: app.netlify.com/drop — drag & drop folder, langsung dapat link.
- **GitHub Pages**: upload ke repo, rename jadi `index.html`, aktifkan di Settings → Pages.
- **Vercel**: vercel.com, import folder/drag & drop.

## 7. Jadikan template untuk dipakai berulang
1. Simpan satu salinan `undangan.html` versi "kosongan" sebagai master template — jangan diedit langsung.
2. Tiap ada acara baru: copy master → ganti data & pilih fitur yang dipakai (hapus yang tidak perlu sesuai daftar di atas) → upload.

## 8. Checklist sebelum dibagikan ke tamu
- [ ] Nama, tanggal, lokasi sudah benar
- [ ] Fitur yang tidak dipakai sudah dihapus bersih (HTML+CSS+JS, cek tidak ada tag yang "menggantung")
- [ ] Semua foto/video tampil (buka file-nya sendiri di browser dulu)
- [ ] Link Google Maps mengarah ke lokasi yang benar
- [ ] Form RSVP/Ucapan bisa diisi dan masuk ke Google Sheet
- [ ] Kalau pakai Wall of Love: cek ucapan tampil setelah publish sheet
- [ ] Kalau pakai Angpao: coba tombol "Salin" setelah file di-hosting online
- [ ] Dicoba buka dari HP, termasuk scroll di cover sebelum klik "Buka Undangan" (harus tidak bisa scroll)
- [ ] Link final dites di mode private/incognito
