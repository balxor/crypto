# Crypto Trading for Gamers with Freqtrade

> Seri tutorial edukasi untuk mempelajari crypto trading dan bot trading menggunakan Freqtrade secara sistematis, dari konsep dasar hingga operasional bot di VPS.

## Tentang Repository Ini

Repository ini berisi seri tutorial tentang dasar crypto trading, setup Freqtrade, pengembangan strategy, backtesting, dry-run, dan operasional bot.

Target pembaca adalah pengguna dengan latar belakang gaming yang terbiasa membaca sistem, statistik, ekonomi virtual, build, probabilitas, dan resource management, tetapi belum familiar dengan crypto market dan risiko trading. Seri ini menekankan pemahaman dan pengendalian risiko, bukan profit cepat. Alur pembelajarannya:

* Memahami market sebelum trading
* Memahami risiko sebelum menggunakan bot
* Memahami strategy sebelum live
* Memahami backtesting sebelum mengandalkan hasilnya
* Memahami operations sebelum menjalankan bot dengan dana sungguhan

---

## Target Pembaca

Sesuai untuk:

* Pengguna baru yang ingin mempelajari crypto secara terstruktur
* Pemula yang ingin memahami trading bot secara teknis
* Pengguna Linux pemula yang ingin belajar setup bot di VPS
* Pembaca yang ingin mempelajari Freqtrade dari dasar
* Pembaca yang ingin memahami backtesting, dry-run, dan risk management

Tidak sesuai untuk:

* Pencari sinyal beli/jual atau rekomendasi coin
* Pencari strategi profit instan
* Pengguna yang ingin menjalankan bot tanpa memahami risiko
* Pengguna yang ingin setup leverage agresif
* Pengguna yang ingin live trading tanpa testing

---

## Prinsip Seri

1. **Edukasi.** Bot tidak dijalankan live sebelum pembaca memahami market, exchange, API key, config, strategy, backtest, dan risiko.
2. **Dry-run.** Setiap strategy diuji dalam mode dry-run sebelum menggunakan dana sungguhan.
3. **Backtest.** Backtest menunjukkan performa pada data historis; kondisi live dapat berbeda.
4. **Security.** API key, VPS, SSH, firewall, reverse proxy, SSL, dan backup adalah bagian dari operasional bot.
5. **Risk management.** Strategy yang baik tetap dapat gagal jika position sizing, stoploss, dan eksposur tidak dikontrol.
6. **Bot menjalankan aturan.** Bot menjalankan strategy persis seperti yang dikonfigurasi; strategy yang buruk akan dijalankan secara konsisten sebagai strategy buruk.

---

## Struktur Seri

| Artikel   | Fokus      | Deskripsi singkat                                              |
| --------- | ---------- | ------------------------------------------------------------- |
| Artikel 1 | Foundation | Konsep dasar crypto market, trading, dan candlestick          |
| Artikel 2 | Setup      | Setup VPS, instalasi Freqtrade, konfigurasi, dan dry-run      |
| Artikel 3 | Strategy   | Indikator, penulisan strategy, backtesting, dan hyperopt      |
| Artikel 4 | Operations | Transisi ke live, risk management, monitoring, dan maintenance |
| Glossary  | Referensi  | Daftar istilah yang digunakan di seluruh seri                 |

### Artikel 1 — Foundation

Dasar crypto trading untuk pembaca baru:

* Cryptocurrency, blockchain, coin, token, wallet, dan stablecoin
* Exchange dan API key
* Mekanisme pasar crypto, market 24/7, pair, order book, spread, fee, likuiditas
* Definisi trading dan jenisnya: spot, margin, dan futures
* Fungsi dan batasan trading bot
* Pengenalan Freqtrade
* Struktur candlestick dan cara membaca chart dasar

Tujuan: membangun pemahaman dasar sebelum masuk ke instalasi bot.

### Artikel 2 — Setup

Setup teknis dari nol hingga bot berjalan dalam mode dry-run:

* Persiapan exchange dan pembuatan API key dengan permission minimal
* Penyewaan dan pengamanan VPS (SSH, user non-root, firewall)
* Setup Ubuntu Server
* Instalasi Freqtrade menggunakan Docker
* Struktur folder dan konfigurasi `config.json`
* FreqUI, Apache reverse proxy, dan SSL Let's Encrypt
* Menjalankan dry-run dan troubleshooting instalasi

Tujuan: menyiapkan environment Freqtrade yang bersih, aman, dan siap untuk eksperimen.

### Artikel 3 — Strategy

Penulisan dan evaluasi strategy:

* Data candle, DataFrame, dan OHLCV
* Indikator teknikal: SMA, EMA, RSI, dan volume
* Struktur strategy Freqtrade dan tiga contoh strategy bertingkat
* Konfigurasi ROI, stoploss, dan trailing stoploss
* Backtesting dan pembacaan hasil secara kritis
* Iterasi strategy, hyperparameter optimization, dan overfitting
* Penggunaan strategy komunitas sebagai referensi

Tujuan: memahami cara strategy dibuat, diuji, dievaluasi, dan diperbaiki.

### Artikel 4 — Operations

Pengoperasian bot setelah strategy diuji:

* Transisi dari dry-run ke live dan pre-live checklist
* Manajemen modal dan position sizing
* Risk limit harian dan mingguan
* Monitoring melalui log, FreqUI, dan alerting
* Prosedur menghentikan bot (kill switch)
* Maintenance server, update OS dan Freqtrade, serta backup
* Incident response dan troubleshooting saat live

Tujuan: menjalankan bot live dengan monitoring, maintenance, disiplin, dan prosedur penghentian yang jelas.

---

## Workflow Belajar

Urutan yang disarankan:

```text
Foundation
    ↓
Setup VPS dan Freqtrade
    ↓
Dry-run
    ↓
Menulis strategy sederhana
    ↓
Backtesting
    ↓
Evaluasi hasil
    ↓
Dry-run ulang
    ↓
Pre-live checklist
    ↓
Live dengan modal kecil
    ↓
Monitoring dan maintenance
```

Setiap tahap diselesaikan sebelum lanjut ke tahap berikutnya. Tahap instalasi tidak dilewati langsung menuju live trading.

---

## Hasil yang Diharapkan

Setelah menyelesaikan seri ini, pembaca diharapkan dapat:

* Memahami konsep dasar crypto market dan perbedaan spot, margin, serta futures
* Memahami fungsi dan risiko trading bot
* Melakukan setup Freqtrade di VPS dan menjalankan dry-run
* Membaca struktur dasar strategy Freqtrade
* Menjalankan backtesting dan membaca hasilnya secara kritis
* Mengenali risiko overfitting
* Melakukan monitoring bot dan menyusun pre-live checklist
* Menjaga konfigurasi dan API key tetap aman

---

## Batasan Seri

Seri ini tidak menyediakan:

* Jaminan profit atau strategi anti-rugi
* Sinyal beli/jual atau rekomendasi coin
* Financial advice
* Setup leverage atau strategi futures agresif
* Bot auto-profit atau klaim passive income tanpa risiko

Seluruh contoh strategy dalam seri ini ditujukan untuk edukasi.

---

## Keamanan Repository

File berikut tidak diunggah ke repository publik:

* API key dan API secret
* Password VPS dan private key SSH
* Token Telegram
* Database production
* File konfigurasi live yang berisi secret
* Informasi akun exchange

Gunakan file example untuk repository:

```text
config.example.json
config.dryrun.example.json
```

Jangan unggah file berisi secret:

```text
config.json
config-live.json
secrets.json
.env
id_rsa
```

Pastikan file sensitif tercantum dalam `.gitignore`:

```gitignore
# Freqtrade sensitive config
config.json
config-live.json
secrets.json
.env

# Database
*.sqlite
*.sqlite3
tradesv3.sqlite

# Logs
*.log
logs/

# SSH keys
id_rsa
id_rsa.pub
*.pem

# Python cache
__pycache__/
*.pyc

# OS files
.DS_Store
Thumbs.db
```

---

## Referensi

Rujukan resmi yang relevan:

* Freqtrade Documentation — https://www.freqtrade.io/en/stable/
* Freqtrade GitHub Repository — https://github.com/freqtrade/freqtrade
* Strategy Customization — https://www.freqtrade.io/en/stable/strategy-customization/
* Backtesting — https://www.freqtrade.io/en/stable/backtesting/
* Configuration — https://www.freqtrade.io/en/stable/configuration/
* FreqUI — https://www.freqtrade.io/en/stable/freq-ui/

Dokumentasi resmi adalah sumber acuan utama karena fitur, command, dukungan exchange, dan best practice dapat berubah antar versi.

---

## Disclaimer

Repository ini ditulis untuk tujuan edukasi.

Crypto trading memiliki risiko tinggi. Harga aset crypto dapat bergerak cepat dan ekstrem. Bot trading tidak menjamin profit. Backtesting tidak menjamin hasil di masa depan. Dry-run tidak sepenuhnya merepresentasikan kondisi live market. Jangan menggunakan dana kebutuhan hidup, dana darurat, dana pinjaman, atau dana operasional keluarga untuk trading.

Setiap keputusan trading adalah tanggung jawab masing-masing pengguna. Materi ini bukan financial advice.

---

2026 - Kenshin Himura
