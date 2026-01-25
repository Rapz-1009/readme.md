# WhatsApp Bot Baileys

Bot WhatsApp berbasis **Node.js** menggunakan library **Baileys**. Fokus ke struktur rapi, modular, dan gampang dikembangin tanpa drama yang tidak perlu.

## Teknologi
- Node.js (LTS disarankan)
- JavaScript (ESM)
- @adiwajshing/baileys / @neoxr/baileys
- Axios / Fetch
- FFmpeg (opsional, media processing)

## Fitur
- Pairing WhatsApp (QR / Pairing Code)
- Command berbasis prefix
- Sistem plugin / handler
- Kirim teks, gambar, audio, video, sticker
- Group management
- Auto reconnect
- Error handler terpusat

## Struktur Proyek
. ├─ src/ │  ├─ plugins/        # Command / fitur bot │  ├─ lib/            # Helper & utilitas │  ├─ database.json   # Database lokal │  └─ index.js        # Entry point ├─ session/           # Session WhatsApp ├─ package.json └─ README.md
Salin kode

## Instalasi
```bash
git clone <repo-url>
cd nama-projek
npm install
Menjalankan Bot
Salin kode
Bash
node src/index.js
Pairing WhatsApp
Jalankan bot
Scan QR atau masukkan pairing code
Session otomatis tersimpan di folder session/
Contoh Command
Salin kode

.menu
.play judul lagu
.sticker
.ping
Konfigurasi
Pengaturan utama bisa diletakkan di index.js atau file config terpisah:
Nomor owner
Prefix command
Mode public / self
API key eksternal
Catatan
Jangan bagikan folder session
Gunakan Node.js versi stabil
Spam itu cepat, banned lebih cepat
