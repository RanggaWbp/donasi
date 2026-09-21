# ☕ Traktir Kopi — Halaman Donasi

Landing page donasi untuk proyek open source. HTML, CSS, dan JavaScript biasa —
**tanpa framework, tanpa build step, tanpa pelacak**. Buka `index.html` langsung di browser atau deploy ke GitHub Pages.

Demo: https://ranggawbp.github.io/donasi/

## ✨ Fitur

- **QRIS dinamis** — QR di-generate langsung di browser (library QRCode.js di `assets/`), nominal yang Anda pilih langsung tertanam di QR. Tanpa request ke server pihak ketiga. Mode gambar statis juga didukung.
- **Multi-metode** — QRIS, PayPal (konversi Rupiah → Dollar otomatis), transfer bank, e-wallet, plus tautan ke Saweria/Trakteer/Ko-fi.
- **Analogi kopi** — cangkir SVG yang terisi mengikuti nominal yang dipilih (`sekitar N gelas kopi`).
- **List proyek live** — diambil dari GitHub API, diurutkan berdasarkan jumlah bintang.
- **Dinding sponsor**, **FAQ**, dan **badge** — semuanya dapat dikonfigurasi.
- **Dark mode** (mengikuti sistem + toggle manual), **responsif** penuh, dan **aksesibel** (tab keyboard, ARIA, fokus terlihat).
- **Slot iklan opsional** — `<head>` sudah ada komentar penanda untuk Google AdSense.
- **Panel pemilik** — buka `/#admin` untuk mengunggah QR dan melihat pratinjau secara lokal (hanya tersimpan di browser Anda).
- **Tanpa pelacak** — nol cookie, nol analitik, nol request pelacakan.

## 🚀 Cara pakai

1. **Fork/clone** repo ini.
2. Buka `index.html`, ubah objek `CONFIG` di dalam `<script>` (bagian paling atas, ada komentar `UBAH BAGIAN INI SAJA`):
   - `name`, `headline`, `lead` — identitas dan ucapan Anda.
   - `bank`, `wallets`, `links` — detail rekening dan platform Anda.
   - `paypalUser` — username dari `paypal.me/username-anda`.
   - QRIS: isi `qrisPayload` dengan URL atau penyedia QRIS Anda.
   - `githubUser` — username GitHub Anda (untuk avatar & list proyek).
   - `proof` dan `uses` — angka dan teks bagian "Ke mana dukungan Anda pergi".
   - `qrisMode: "dynamic"` (default) atau `"image"`.
3. Simpan, commit, push.

### QRIS

| Mode | Cara |
|---|---|
| `"dynamic"` (default) | QR di-generate di browser. Isi diatur via `qrisPayload` (default: URL repo ini). Ganti nilai ini dengan target QRIS Anda. **Nominal dipilih tidak dikirim ke mana pun** — aplikasi bank/e-wallet Anda yang menanyakan nominal saat scan. |
| `"image"` | Set `qrisMode: "image"`, lalu timpa `assets/qris.png` dengan gambar QRIS Anda (600×600 px atau lebih). |

**Catatan teknis:** payload QRIS dinamis hanya membawa URL/target. Aplikasi penerima QRIS (bank/e-wallet) yang menanyakan nominal saat Anda scan — itulah mengapa tidak ada pelacak dan kenapa halaman ini tidak perlu server. Untuk QRIS statis, pakai mode `"image"`.

### PayPal

Set `paypalUser` dengan username PayPal.Me Anda. Tombol akan membuka `paypal.me/username/AMOUNT` dengan jumlah yang setara dengan nominal Rupiah yang dipilih (konversi kasar via `idrPerUsd`, default 16.000). Jika sudah punya gambar QR PayPal, simpan ke `assets/paypal-qr.png` — gambar akan otomatis dimuat di tab PayPal.

### Iklan (opsional)

`<head>` sudah berisi komentar penanda tempat menempelkan skrip Google AdSense setelah akun Anda disetujui. **Pertimbangkan risikonya** — halaman ini berisi data keuangan (nomor rekening, QRIS). Bercampurnya pelacak iklan dengan halaman pembayaran adalah risiko keamanan dan kepercayaan. Mendukung proyek secara langsung lebih etis daripada menjual perhatian pendukung.

## 🔒 Privasi

Tidak ada cookie, tidak ada analitik, tidak ada request ke server iklan. Satu-satunya request keluar adalah:

- `api.github.com` — untuk avatar dan list proyek (API publik GitHub).
- `fonts.googleapis.com` / `fonts.gstatic.com` — untuk font (boleh dihapus jika Anda mau font sistem saja).
- `paypal.me` — hanya saat tombol PayPal diklik.

GitHub Pages sendiri mengumpulkan data telemetri standar platform mereka — di luar kode halaman ini.

## 🎨 Kustomisasi lanjutan

- **Warna**: ubah variabel `:root` di `<style>` (`--brand`, `--mango`, dll).
- **FAQ**: tambah/ubah objek di array `FAQ`.
- **Sponsor**: isi array `SPONSORS` — `{ login: "username-github", amount: 50000 }`. Avatar diambil dari GitHub.
- **Font**: ganti link Google Fonts di `<head>`.
- **Presets nominal & harga kopi**: `presets`, `defaultAmount`, `coffeePrice`.

## 📁 Struktur

```
donasi/
├── index.html          # seluruh halaman (HTML + CSS + JS)
├── README.md
└── assets/
    ├── qris.png        # QRIS mode gambar (opsional)
    ├── paypal-qr.png   # QR PayPal (opsional)
    ├── og.png          # preview saat dibagikan
    └── qrcode.min.js   # library QRCode.js (lokal, tanpa CDN)
```

## 📜 Lisensi

MIT — bebas dipakai, diubah, dan didistribusikan ulang.
