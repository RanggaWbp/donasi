# 🥤 Traktir Es Teh — Halaman Donasi

Landing page donasi untuk proyek open source. HTML, CSS, dan JavaScript biasa —
**tanpa framework, tanpa build step, tanpa pelacak**. Buka `index.html` langsung di browser atau deploy ke GitHub Pages.

Demo: https://ranggawbp.github.io/donasi/

## ✨ Fitur

- **QRIS & PayPal statis** — gambar QR dibaca langsung dari `assets/`. Tidak ada generator QR, tidak ada nominal yang dipilih di halaman.
- **Multi-metode** — QRIS, PayPal, transfer bank (Bank Jago), e-wallet (ShopeePay & OVO).
- **Kontak** — Telegram (`CONFIG.telegram`) dan Facebook (`CONFIG.facebook`) sebagai tombol di hero; ketiganya plus XDA (`CONFIG.xda`) sebagai tautan di footer. Profil XDA juga muncul di bawah daftar proyek lewat kunci bahasa `xdaNote`.
- **12 bahasa** — id, en, ar, bn, de, es, fr, hi, ja, pt, ru, zh. Otomatis mengikuti bahasa browser, bisa diganti lewat menu di header, dan `dir="rtl"` diatur sendiri untuk Arab. Tersimpan di `localStorage`. Angka (jumlah commit) diformat pakai `Intl` per bahasa, dan bentuk jamak mengikuti aturan tiap bahasa.
- **Daftar proyek statis** — semua repo akun (termasuk fork, ditandai label `fork`) ada di `CONFIG.projects` dengan jumlah commit masing-masing, jadi halaman tetap jalan tanpa panggilan API. Tidak ada nomor telemetri yang diambil saat dibuka.
- **Dinding sponsor**, **FAQ**, dan **badge** — semuanya dapat dikonfigurasi.
- **Dark mode** (mengikuti sistem + toggle manual), **responsif** penuh, dan **aksesibel** (tab keyboard, ARIA, fokus terlihat).
- **Slot iklan opsional** — `<head>` sudah ada komentar penanda untuk Google AdSense.
- **Tanpa pelacak** — nol cookie, nol analitik, nol request pelacak.

## 🚀 Cara pakai

1. **Fork/clone** repo ini.
2. Buka `index.html`, ubah objek `CONFIG` di dalam `<script>` (bagian paling atas, ada komentar `UBAH BAGIAN INI SAJA`):
   - `bank`, `wallets` — nomor rekening dan e-wallet.
   - `paypalLink` — tautan `https://paypal.me/USERNAME`.
   - `qrisImage`, `paypalQrImage` — path gambar QR (lihat bagian QR di bawah).
   - `projects` — daftar proyek + jumlah commit. Tambahkan `fork: true` pada repo hasil fork.
   - `proof` — dua angka kecil di bawah tombol. `l` berisi **kunci bahasa**, bukan teks mentah.
   - `githubUser` — username GitHub Anda (untuk avatar).
3. Simpan, commit, push.

### Menempelkan gambar QR

| Gambar | Taruh di | Nama file | Dibaca oleh |
|---|---|---|---|
| QRIS | `assets/` | `qris.png` | tab **QRIS** |
| PayPal | `assets/` | `paypal-qr.png` | tab **PayPal** (opsional — kalau file dihapus, tab hanya menampilkan tombol) |

Langkahnya:

```
cp /path/qris-ganda-saya.png   donasi/assets/qris.png
cp /path/qr-paypal-saya.png    donasi/assets/paypal-qr.png
cd donasi && git add assets && git commit -m "chore: ganti gambar QR" && git push
```

Syarat gambar: PNG atau JPG, minimal 600×600 px, jangan ada bagian QR yang terpotong.
Kalau nama file diganti, sesuaikan `qrisImage` / `paypalQrImage` di `CONFIG`.

### Menambah bahasa

Tambah satu blok di `assets/i18n.js` dengan kunci kode bahasa, lalu daftarkan di `window.LANGS`:

```js
window.LANGS.push(["ko", "한국어", "ltr"]);
window.I18N.ko = { /* ...salin struktur dari `id`... */ };
```

Field ketiga `LANGS` adalah arah teks. Isi `"rtl"` untuk bahasa kanan-ke-kiri.

### Memperbarui jumlah commit proyek

Cara tercepat — hitung semua repo sekaligus dan bandingkan dengan yang ada di halaman:

```bash
for r in $(gh api 'users/RanggaWbp/repos?per_page=100' --paginate -q '.[].name'); do
  n=$(gh api "repos/RanggaWbp/$r/commits?per_page=1" -i 2>/dev/null \
      | grep -o 'page=[0-9]*' | tail -1 | cut -d= -f2)
  printf '%8s  %s\n' "${n:-1}" "$r"
done | sort -rn
```

Salin angkanya ke `commits` di `CONFIG.projects`. Untuk repo baru, tambahkan satu blok
`{ name, url, desc, commits }` — beri `fork: true` kalau repo itu hasil fork.

### Iklan (opsional)

`<head>` sudah berisi komentar penanda tempat menempelkan skrip Google AdSense setelah akun Anda disetujui. **Pertimbangkan risikonya** — halaman ini berisi data keuangan (nomor rekening, QRIS). Bercampurnya pelacak iklan dengan halaman pembayaran adalah risiko keamanan dan kepercayaan. Mendukung proyek secara langsung lebih etis daripada menjual perhatian pendukung.

## 🔒 Privasi

Tidak ada cookie, tidak ada analitik, tidak ada panggilan API. Satu-satunya request keluar adalah:

- `avatars.githubusercontent.com` — foto profil (satu gambar).
- `fonts.googleapis.com` / `fonts.gstatic.com` — untuk font (boleh dihapus jika Anda mau font sistem saja).
- `paypal.me` — hanya saat tombol PayPal diklik.

GitHub Pages sendiri mengumpulkan data telemetri standar platform mereka — di luar kode halaman ini.

Halaman ini **tidak** memanggil `api.github.com`; daftar proyek diisi manual di `CONFIG.projects`.

## 🎨 Kustomisasi lanjutan

- **Warna**: ubah variabel `:root` di `<style>` (`--brand`, `--mango`, dll).
- **FAQ & "Ke mana dukungan Anda pergi"**: ubah array `faq` dan `uses` di dalam tiap bahasa di `assets/i18n.js`.
- **Validasi bahasa**: `node check-i18n.js assets/i18n.js` (dari skill `static-site-i18n`) — gagal kalau ada kunci yang tidak sama antar bahasa.
- **Sponsor**: isi array `SPONSORS` — `{ login: "username-github", amount: 50000 }`. Avatar diambil dari GitHub.
- **Font**: ganti link Google Fonts di `<head>`.

## 📁 Struktur

```
donasi/
├── index.html          # halaman + konfigurasi (HTML + CSS + JS)
├── README.md
└── assets/
    ├── qris.png        # gambar QRIS statis  ← tempel di sini
    ├── paypal-qr.png   # gambar QR PayPal     ← tempel di sini
    ├── og.png          # preview saat dibagikan
    └── i18n.js         # 12 paket bahasa
```

## 📜 Lisensi

MIT — bebas dipakai, diubah, dan didistribusikan ulang.
