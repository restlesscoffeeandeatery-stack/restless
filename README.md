# Restless Hub (PWA)

Cara pasang di GitHub Pages:
1. Buat repo baru (atau folder di repo existing), lalu upload **SEMUA isi folder ini APA ADANYA**
   ke root repo tersebut — index.html, manifest.json, sw.js, dan ketiga file icon-*.png
   (semuanya sudah rata di satu level, tidak ada subfolder lagi, supaya tidak ke-flatten
   saat upload lewat GitHub web).
2. Aktifkan GitHub Pages: Settings > Pages > Deploy from branch > pilih branch & folder root.
3. Buka URL Pages-nya di HP > menu browser > "Add to Home Screen" / "Install app".

Kalau tombol install tidak muncul di Android:
- Pastikan ketiga file icon (`icon-192.png`, `icon-512.png`, `icon-maskable-512.png`) benar-benar
  ada di root repo (cek langsung: buka `https://<url-pages>/icon-192.png`, harus tampil gambar,
  bukan 404).
- Clear data situs itu di Chrome dulu (biar service worker lama yang sempat cache error tidak
  kepakai), baru buka ulang.
- Tunggu beberapa detik setelah halaman kebuka sebelum cek menu titik tiga — Chrome butuh waktu
  untuk selesai evaluasi manifest.

Catatan:
- PWA (install + offline shell) hanya jalan di atas HTTPS — GitHub Pages sudah HTTPS otomatis.
- Service worker hanya nge-cache halaman hub-nya sendiri, bukan konten miniapp di dalam iframe —
  jadi Rekap Keuangan, Stock, dll tetap selalu ambil data terbaru.

## Kode akses (PIN)

- Buka pertama kali di device manapun -> diminta **buat PIN 4 digit** (isi 2x untuk konfirmasi).
- Semua link program dienkripsi (AES-GCM) memakai kunci turunan dari PIN itu, lalu baru
  disimpan di localStorage. File index.html ini sendiri **tidak menyimpan link apapun dalam
  bentuk plain text** — jadi kalau ada yang buka "view source" tanpa tahu PIN, tidak akan
  kelihatan link Rekap Keuangan/Stock/dll sama sekali, yang ada cuma ciphertext.
- Setelah PIN dibuat, tambahkan program lewat tombol "+ Tambah Link" seperti biasa — tersimpan
  otomatis terenkripsi.
- PIN diminta ulang setiap kali halaman di-reload/dibuka lagi (session tidak bertahan setelah
  refresh, ini disengaja). Ada juga tombol 🔒 di pojok kanan atas untuk mengunci manual kapan saja.
- **PIN berbeda per device/browser** — karena semuanya tersimpan lokal, install di HP lain berarti
  setup PIN + tambah link dari nol lagi di situ (tidak sinkron otomatis).
- **Kalau lupa PIN, data link TIDAK BISA dipulihkan** (karena memang terenkripsi, bukan cuma
  disembunyikan). Satu-satunya jalan adalah tombol "Lupa PIN? Reset semua data hub" di layar
  kunci, yang menghapus vault dan mulai dari PIN baru + link kosong.
