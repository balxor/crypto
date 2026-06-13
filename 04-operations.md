# Artikel 4 - Operations

# Mengoperasikan Freqtrade: Transisi ke Live, Manajemen Risiko, dan Maintenance

> Artikel keempat dari seri tutorial Freqtrade. Artikel ini membahas transisi dari dry-run ke live trading, manajemen modal dan position sizing, risk limit, monitoring melalui log dan FreqUI, alerting, prosedur menghentikan bot, maintenance dan update server, backup, incident response, serta troubleshooting pada saat bot berjalan live.

---

## Daftar Isi

1. [Pendahuluan](#pendahuluan)
2. [Peran Operations](#peran-operations)
3. [Dry-run dan Perbedaannya dengan Live](#dry-run-dan-perbedaannya-dengan-live)
4. [Transisi ke Live](#transisi-ke-live)
   * [Pre-live Checklist](#pre-live-checklist)
   * [Urutan Transisi](#urutan-transisi)
   * [Review Parameter Config](#review-parameter-config)
5. [Manajemen Risiko](#manajemen-risiko)
   * [Manajemen Modal](#manajemen-modal)
   * [Position Sizing](#position-sizing)
   * [Risk Limit Harian dan Mingguan](#risk-limit-harian-dan-mingguan)
6. [Monitoring](#monitoring)
   * [Komponen yang Dipantau](#komponen-yang-dipantau)
   * [Membaca Log](#membaca-log)
   * [Monitoring Melalui FreqUI](#monitoring-melalui-frequi)
   * [Alerting](#alerting)
7. [Menghentikan Bot](#menghentikan-bot)
   * [Kapan Bot Dihentikan](#kapan-bot-dihentikan)
   * [Kill Switch Plan](#kill-switch-plan)
8. [Maintenance](#maintenance)
   * [Maintenance Server](#maintenance-server)
   * [Update OS](#update-os)
   * [Update Freqtrade](#update-freqtrade)
   * [Backup Konfigurasi dan Strategy](#backup-konfigurasi-dan-strategy)
9. [Incident Response](#incident-response)
10. [Troubleshooting Saat Live](#troubleshooting-saat-live)
11. [Routine Review](#routine-review)
12. [Checklist Operasional](#checklist-operasional)
13. [Glossary](#glossary)
14. [Ringkasan](#ringkasan)
15. [Disclaimer](#disclaimer)

---

## Pendahuluan

Artikel ini adalah lanjutan dari Artikel 3 - Strategy. Artikel sebelumnya membahas penulisan dan pengujian strategy; artikel ini membahas pengoperasian bot pada fase mendekati live dan live.

Pada tahap ini, pertanyaan yang relevan bukan lagi sebatas apakah strategy profitable pada backtest, melainkan:

```text
Apakah bot dapat dijalankan dengan risiko terkontrol?
Apakah server stabil?
Apakah API key aman?
Apakah log dapat dipantau?
Apakah ada prosedur menghentikan bot?
Apakah modal dikelola dengan benar?
Apakah pengguna memiliki prosedur penanganan error?
```

Live trading menggunakan dana sungguhan. Kesalahan operasional pada fase ini berdampak langsung pada modal, sehingga prosedur operasional perlu disiapkan sebelum live.

### Prasyarat

Pembaca diasumsikan sudah menyelesaikan Artikel 1–3 dan memiliki strategy yang telah diuji melalui backtest dan dry-run.

### Tujuan

Setelah menyelesaikan artikel ini, pembaca diharapkan dapat:

* Mengelola transisi dari dry-run ke live secara bertahap
* Mengatur manajemen modal dan position sizing
* Menetapkan risk limit harian dan mingguan
* Memantau bot melalui log, FreqUI, dan alert
* Menjalankan prosedur penghentian bot (kill switch)
* Melakukan maintenance, update, dan backup server
* Menangani insiden operasional yang umum terjadi

---

## Peran Operations

Operations adalah disiplin menjalankan dan merawat sistem. Pada trading bot, operations adalah komponen inti, bukan pelengkap. Kualitas operations menentukan apakah strategy dapat dijalankan dengan risiko terkendali.

| Strategy | Operations | Hasil                                                       |
| -------- | ---------- | ----------------------------------------------------------- |
| Baik     | Buruk      | Bot dapat gagal karena config, server, API, atau monitoring |
| Buruk    | Baik       | Bot tetap merugi karena logika strategy lemah               |
| Baik     | Baik       | Risiko lebih terkontrol; profit tetap tidak dijamin         |
| Buruk    | Buruk      | Risiko tertinggi                                            |

Cakupan operations meliputi prosedur menjalankan dan menghentikan bot, pembatasan modal, pemantauan error, pelaksanaan update, penyimpanan backup, penanganan insiden, dan evaluasi strategy secara berkala. Seluruh aspek ini dibahas pada bagian-bagian berikut.

---

## Dry-run dan Perbedaannya dengan Live

Dry-run adalah mode simulasi yang menjalankan strategy pada kondisi market real-time tanpa mengirim order sungguhan ke exchange. Dry-run digunakan untuk memverifikasi:

* Bot berjalan tanpa error
* Strategy menghasilkan sinyal
* Konfigurasi valid
* Pairlist berfungsi
* FreqUI dapat diakses
* Log bersih dari error kritis
* Prosedur start, stop, dan restart bot dikuasai

Perbedaan dry-run dan live:

| Aspek             | Dry-run                  | Live                      |
| ----------------- | ------------------------ | ------------------------- |
| Order             | Simulasi                 | Order sungguhan           |
| Dana              | Simulasi                 | Dana sungguhan            |
| Slippage          | Tidak selalu akurat      | Nyata                     |
| Eksekusi exchange | Tidak sepenuhnya nyata   | Nyata                     |
| Tekanan psikologis | Rendah                  | Tinggi                    |
| Error API         | Dapat muncul             | Berdampak langsung        |
| Risiko finansial  | Tidak ada                | Langsung                  |

Dry-run perlu dijalankan dalam periode yang cukup panjang agar mencakup beberapa kondisi market sebelum live. Dry-run yang terlihat positif selama beberapa jam tidak cukup sebagai dasar untuk live.

---

## Transisi ke Live

### Pre-live Checklist

Selesaikan seluruh item wajib berikut sebelum mengubah `dry_run` menjadi `false`:

| Checklist                                       | Status           |
| ----------------------------------------------- | ---------------- |
| Strategy sudah dibaca dan dipahami              | Wajib            |
| Strategy sudah dibacktest                       | Wajib            |
| Strategy sudah diuji pada beberapa periode market | Wajib          |
| Strategy sudah dijalankan dalam dry-run         | Wajib            |
| Dry-run tidak menghasilkan error kritis         | Wajib            |
| API key tidak memiliki permission withdrawal    | Wajib            |
| Config live tidak disimpan di repository publik | Wajib            |
| Stake amount sesuai modal kecil                 | Wajib            |
| Max open trades dibatasi                        | Wajib            |
| Stoploss aktif                                  | Wajib            |
| Pairlist dipilih secara sadar                   | Wajib            |
| FreqUI dapat diakses secara aman                | Wajib            |
| Password FreqUI kuat                            | Wajib            |
| VPS sudah diamankan                             | Wajib            |
| Backup config dan strategy tersedia             | Wajib            |
| Prosedur menghentikan bot dipahami              | Wajib            |
| Kill switch plan sudah ditulis                  | Wajib            |
| IP whitelist aktif (jika exchange mendukung)    | Direkomendasikan |

Live trading tidak digunakan sebagai tempat menemukan kesalahan dasar. Selesaikan seluruh item wajib terlebih dahulu.

### Urutan Transisi

Transisi ke live dilakukan bertahap, satu perubahan pada satu waktu:

```text
Backtest
    ↓
Dry-run
    ↓
Dry-run dengan config mendekati live
    ↓
Live dengan modal sangat kecil
    ↓
Monitoring ketat
    ↓
Evaluasi
    ↓
Penambahan modal hanya setelah risiko dipahami
```

Tujuan live pertama adalah memverifikasi sistem bekerja pada kondisi nyata dengan risiko kecil, bukan menghasilkan profit besar. Modal kecil menjadikan kesalahan awal sebagai biaya belajar yang terkendali; modal besar membuat kesalahan kecil menjadi mahal.

### Review Parameter Config

Perubahan dari dry-run ke live secara teknis hanya mengubah satu parameter:

```json
"dry_run": false
```

Namun parameter ini tidak diubah tanpa meninjau ulang parameter terkait:

| Parameter         | Hal yang diperiksa                          |
| ----------------- | ------------------------------------------- |
| `stake_amount`    | Sesuai modal kecil, tidak terlalu besar     |
| `max_open_trades` | Dibatasi, tidak terlalu banyak              |
| `pair_whitelist`  | Berisi pair likuid                          |
| `stoploss`        | Aktif dan bernilai wajar                    |
| `trading_mode`    | `spot` untuk pemula                         |
| `api_server`      | Tidak diekspos langsung ke internet         |
| `exchange.key`    | API key benar                               |
| `exchange.secret` | Secret tidak bocor                          |
| `initial_state`   | `stopped` agar bot tidak langsung trading   |

---

## Manajemen Risiko

### Manajemen Modal

Manajemen modal adalah dasar operations. Tanpa manajemen modal, strategy yang baik tetap dapat menghabiskan akun. Prinsip yang digunakan:

* Gunakan hanya modal yang siap hilang
* Jangan gunakan dana darurat, dana pinjaman, atau dana operasional keluarga
* Jangan gunakan seluruh saldo exchange untuk bot
* Pisahkan modal bot dari modal lain
* Mulai dari ukuran kecil
* Tambah modal hanya setelah evaluasi

Contoh alokasi modal untuk live pertama:

| Total dana crypto | Modal bot awal | Catatan                            |
| ----------------: | -------------: | ---------------------------------- |
|        1.000 USDT |       100 USDT | 10% untuk testing live             |
|        5.000 USDT |       250 USDT | Tetap kecil meski dana lebih besar |
|       10.000 USDT |       500 USDT | Evaluasi bertahap                  |

Tabel di atas adalah ilustrasi edukasi, bukan rekomendasi finansial. Prinsipnya: live pertama menggunakan modal kecil, bukan seluruh modal sekaligus.

### Position Sizing

Position sizing menentukan ukuran setiap trade. Pada Freqtrade, ukuran posisi dipengaruhi beberapa parameter:

| Parameter                | Fungsi                           |
| ------------------------ | -------------------------------- |
| `stake_amount`           | Modal per trade                  |
| `max_open_trades`        | Jumlah posisi aktif maksimal     |
| `tradable_balance_ratio` | Porsi saldo yang boleh digunakan |
| Pairlist                 | Aset yang boleh diperdagangkan   |
| `stoploss`               | Batas kerugian per trade         |

Contoh konfigurasi:

```json
"stake_currency": "USDT",
"stake_amount": 20,
"max_open_trades": 3
```

Eksposur maksimal kasar dari konfigurasi tersebut:

```text
20 USDT × 3 posisi = 60 USDT
```

Dengan stoploss strategy −5%, potensi loss per posisi:

```text
20 USDT × 5% = 1 USDT
```

Jika ketiga posisi terkena stoploss bersamaan:

```text
1 USDT × 3 = 3 USDT
```

Perhitungan ini adalah estimasi dasar. Pada kondisi live, fee, slippage, spread, dan eksekusi order menghasilkan angka yang berbeda. Estimasi ini tetap berguna untuk memahami eksposur sebelum bot dijalankan.

Prinsip position sizing:

* Hitung risiko dari skenario salah, bukan dari target profit
* Hitung eksposur maksimal
* Hitung total loss jika seluruh posisi terkena stoploss
* Pastikan satu trade tidak dapat menghabiskan akun
* Pastikan satu pair tidak mendominasi eksposur

### Risk Limit Harian dan Mingguan

Selain stoploss per trade, tetapkan batas risiko harian dan mingguan secara tertulis sebelum live. Contoh aturan:

| Kondisi                                     | Aksi                            |
| ------------------------------------------- | ------------------------------- |
| Loss harian mencapai 2% dari modal bot      | Hentikan bot dan review         |
| Loss mingguan mencapai 5% dari modal bot    | Hentikan bot minimal beberapa hari |
| Tiga stoploss beruntun                      | Hentikan bot dan periksa market |
| Exchange error berulang                     | Hentikan bot                    |
| VPS tidak stabil                            | Hentikan bot                    |
| Perilaku strategy menyimpang dari backtest  | Hentikan bot                    |

Angka di atas adalah ilustrasi; setiap pengguna menetapkan batas sesuai profil risiko masing-masing. Batas yang ditulis sebelum live mencegah keputusan diambil berdasarkan emosi: saat profit muncul dorongan untuk memperbesar modal, saat loss muncul dorongan untuk membalas. Keduanya berisiko tanpa aturan tertulis.

---

## Monitoring

### Komponen yang Dipantau

Monitoring adalah pemantauan kondisi sistem secara rutin untuk memastikan bot berjalan normal. Komponen yang dipantau mencakup sisi trading (status bot, trade aktif, trade history, profit/loss, drawdown, exit reason) dan sisi infrastruktur (error log, API error, koneksi exchange, saldo exchange, CPU/RAM/disk VPS, status container, status reverse proxy, dan masa berlaku SSL).

Alat monitoring:

| Alat                | Fungsi                          |
| ------------------- | ------------------------------- |
| FreqUI              | Status bot dan trade            |
| Log Docker          | Detail proses bot               |
| Telegram            | Notifikasi                      |
| SSH                 | Pemeriksaan server              |
| `htop`              | Resource server                 |
| `df`                | Penggunaan storage              |
| `systemctl`         | Status service (mis. Apache)    |
| `docker compose ps` | Status container                |

Command pemeriksaan dasar:

```bash
# Status container
docker compose ps

# Log real-time
docker compose logs -f

# Resource server
htop
df -h
free -h
```

### Membaca Log

Log adalah sumber informasi pertama saat terjadi masalah.

```bash
cd ~/freqtrade

# Log real-time
docker compose logs -f

# 200 baris terakhir
docker compose logs --tail=200

# Filter error
docker compose logs | grep -i error

# Filter warning
docker compose logs | grep -i warning
```

Jenis log yang perlu diperhatikan:

| Log                | Arti umum                              |
| ------------------ | -------------------------------------- |
| Exchange Error     | Masalah komunikasi dengan exchange     |
| Network Error      | Masalah koneksi                        |
| Insufficient Funds | Saldo tidak cukup                      |
| Pair Not Available | Pair tidak tersedia di exchange        |
| Invalid API Key    | API key salah atau permission kurang   |
| Rate Limit         | Request ke exchange melebihi batas     |
| JSON Error         | Config tidak valid                     |
| Strategy Error     | Error pada file strategy               |
| Database Error     | Masalah pada database lokal            |

Warning yang muncul berulang perlu ditindaklanjuti. Satu warning tunggal mungkin tidak kritis, tetapi warning yang terus berulang mengindikasikan masalah operasional.

### Monitoring Melalui FreqUI

FreqUI menampilkan status bot, open trades, closed trades, profit/loss, pair aktif, chart, durasi trade, detail entry/exit, kontrol bot, dan sebagian informasi strategy.

Ketentuan keamanan FreqUI:

* Gunakan password yang kuat dan bukan kredensial default
* Akses melalui HTTPS
* Jangan ekspos API server langsung ke internet; gunakan reverse proxy
* Batasi akses jika memungkinkan
* Jangan membagikan screenshot yang memuat informasi sensitif

Jika FreqUI tidak dapat diakses, telusuri rantai komponen berikut secara berurutan:

```text
Browser → DNS → HTTPS/SSL → Apache Reverse Proxy → Freqtrade API → Docker Container
```

Periksa Freqtrade API lokal:

```bash
curl http://127.0.0.1:8080
```

Interpretasi: jika command gagal dari dalam server, masalah berada di Freqtrade API atau container. Jika command berhasil tetapi akses domain gagal, masalah berada di DNS, Apache, firewall, atau SSL.

### Alerting

Alerting memberitahukan kondisi bot tanpa perlu membuka dashboard secara terus-menerus. Freqtrade dapat dimonitor dan dikontrol melalui FreqUI atau Telegram. Notifikasi yang dapat dikirim: bot start/stop, entry order, exit order, profit/loss, error tertentu, status harian, dan trade summary.

Ketentuan penggunaan alert:

* Aktifkan hanya alert yang relevan
* Pastikan alert masuk ke akun yang aman
* Jangan memasukkan bot ke grup besar
* Jangan memberikan akses command Telegram ke pihak yang tidak dipercaya
* Jangan membagikan atau meng-commit token Telegram ke repository

Volume alert perlu seimbang: alert yang terlalu banyak cenderung diabaikan, sedangkan alert yang terlalu sedikit membuat masalah terlambat diketahui.

---

## Menghentikan Bot

### Kapan Bot Dihentikan

Prosedur menghentikan bot sama pentingnya dengan menjalankannya. Bot dihentikan pada kondisi berikut:

* Loss harian atau mingguan melewati batas
* Volatilitas market ekstrem
* Exchange mengalami gangguan
* API error berulang
* VPS tidak stabil
* Perilaku strategy menyimpang dari ekspektasi
* Stoploss terlalu sering terpicu
* Pair menjadi tidak likuid
* Ada berita besar yang membuat kondisi market tidak normal
* Config baru belum diuji
* Update bot belum diverifikasi
* Pengguna tidak dapat melakukan monitoring

Bot tidak harus selalu aktif. Menghentikan bot pada kondisi yang tidak jelas adalah keputusan operasional yang valid.

### Kill Switch Plan

Kill switch plan adalah prosedur menghentikan bot dengan cepat, yang harus dikuasai sebelum live.

Menghentikan container:

```bash
cd ~/freqtrade
docker compose down
```

Bot juga dapat dihentikan melalui FreqUI jika masih responsif. Jika FreqUI tidak responsif, gunakan SSH dan Docker.

| Situasi                 | Aksi                                    |
| ----------------------- | --------------------------------------- |
| Bot error berulang      | Stop container                          |
| Exchange API bermasalah | Stop bot                                |
| Strategy salah entry    | Stop bot dan review                     |
| VPS resource penuh      | Stop bot dan periksa server             |
| FreqUI tidak responsif  | Masuk SSH dan periksa Docker            |
| Config salah            | Stop bot, backup, perbaiki config       |
| Market ekstrem          | Stop bot jika risiko tidak terkontrol   |

Setelah menghentikan bot, lakukan review sebelum menjalankan ulang:

```text
Apa penyebabnya?
Apakah ada trade aktif?
Apakah ada open order di exchange?
Apakah saldo sesuai?
Apakah config berubah?
Apakah strategy error?
Apakah log menunjukkan masalah?
```

Jika terdapat open order, periksa langsung di exchange. Saat bot sedang error, status di dashboard bot belum tentu akurat.

---

## Maintenance

### Maintenance Server

VPS yang tidak dirawat dapat menjadi sumber masalah. Komponen yang dipantau: CPU, RAM, disk, stabilitas network, container, status Apache, SSL, update OS, ukuran log, backup, dan upaya login tidak sah.

```bash
# Resource
htop
free -h
df -h

# Penggunaan Docker
docker system df

# Status service
sudo systemctl status apache2
sudo ufw status

# Riwayat login dan auth log
last
sudo tail -n 100 /var/log/auth.log
```

Disk yang penuh menyebabkan bot bermasalah. Periksa penggunaan Docker dan bersihkan resource yang tidak terpakai dengan hati-hati:

```bash
docker system df
docker system prune
```

`docker system prune` menghapus container, network, dan image yang tidak terpakai. Jalankan hanya setelah memahami dampaknya dan memastikan backup tersedia.

### Update OS

```bash
sudo apt update
sudo apt upgrade -y

# Cek kebutuhan reboot
cat /var/run/reboot-required
```

Keberadaan file `/var/run/reboot-required` menandakan server perlu reboot.

Sebelum reboot: hentikan bot jika diperlukan, pastikan tidak ada operasi penting berjalan, catat trade aktif, dan pastikan SSH, Docker, serta Apache dapat start ulang.

```bash
sudo reboot
```

Setelah reboot, verifikasi sistem:

```bash
docker compose ps
sudo systemctl status apache2
docker compose logs --tail=100
```

Hindari update OS pada saat kondisi market sensitif sementara bot sedang menjalankan posisi.

### Update Freqtrade

Freqtrade diupdate untuk mengikuti perubahan exchange API, dependency, bugfix, dan fitur baru. Pola update pada Docker:

```bash
cd ~/freqtrade
docker compose pull
docker compose up -d
```

Verifikasi setelah update:

```bash
docker compose ps
docker compose logs --tail=200
```

Sebelum update: backup config, strategy, dan database bila diperlukan; baca changelog; pastikan tidak ada trade penting; siapkan waktu untuk rollback.

Setelah update: pastikan bot dapat start, FreqUI dapat diakses, strategy tidak error, dan log bersih dari error kritis. Untuk perubahan besar, uji melalui dry-run sebelum kembali ke live. Jika update menyebabkan error, hentikan bot, baca log, periksa changelog, dan rollback bila perlu.

### Backup Konfigurasi dan Strategy

File yang perlu di-backup:

| File atau folder              | Fungsi                          |
| ----------------------------- | ------------------------------- |
| `user_data/config.json`       | Config utama                    |
| `user_data/strategies/`       | File strategy                   |
| `user_data/data/`             | Data historis                   |
| `user_data/backtest_results/` | Hasil backtest                  |
| `user_data/logs/`             | Log                             |
| Database Freqtrade            | Riwayat trade dan data operasional |

Secret tidak diunggah ke repository publik. Untuk GitHub, gunakan file example dan `.gitignore`.

File yang aman diunggah:

```text
config.example.json
config.dryrun.example.json
```

File yang tidak diunggah:

```text
config.json
config-live.json
secrets.json
.env
*.sqlite
```

Contoh `.gitignore`:

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

Backup manual:

```bash
cd ~/freqtrade

tar -czf freqtrade-backup-$(date +%Y%m%d).tar.gz \
  user_data/config.json \
  user_data/strategies \
  user_data/backtest_results
```

Backup yang memuat API key disimpan di lokasi aman, bukan di repository atau lokasi publik.

---

## Incident Response

Incident response adalah prosedur penanganan masalah dengan tujuan membatasi kerusakan, mengidentifikasi penyebab, dan mencegah kejadian berulang. Berikut prosedur untuk beberapa insiden umum.

### API Key Error

Gejala:

```text
Invalid API key
Authentication failed
Permission denied
```

Prosedur:

1. Hentikan bot
2. Periksa permission API di exchange
3. Periksa IP whitelist
4. Periksa apakah API key baru diubah
5. Periksa config
6. Rotasi API key jika ada risiko bocor
7. Jalankan ulang bot setelah kondisi aman

### VPS Down

Prosedur:

1. Periksa panel provider VPS
2. Periksa apakah server merespons ping
3. Periksa apakah SSH dapat login
4. Periksa status Docker
5. Periksa status Apache
6. Periksa log setelah server hidup
7. Periksa open order langsung di exchange
8. Verifikasi kondisi bot sebelum mengasumsikannya berjalan normal

### Bot Membuka Trade di Luar Ekspektasi

Prosedur:

1. Hentikan bot
2. Periksa trade di FreqUI
3. Periksa trade di exchange
4. Baca log entry
5. Periksa code strategy
6. Periksa pair dan timeframe
7. Periksa apakah config berubah
8. Jalankan backtest ulang bila perlu
9. Jalankan dry-run sebelum kembali ke live

### Loss Beruntun

Prosedur:

1. Hentikan bot jika sudah melewati risk limit
2. Periksa kondisi market
3. Periksa exit reason
4. Identifikasi pair dengan loss terbesar
5. Periksa apakah stoploss terlalu ketat atau terlalu longgar
6. Periksa kesesuaian strategy dengan kondisi market saat ini
7. Jangan menaikkan modal
8. Jangan melakukan revenge trading

Setiap insiden dicatat dengan format berikut:

```text
Tanggal:
Waktu:
Strategy:
Pair:
Masalah:
Dampak:
Aksi:
Penyebab sementara:
Perbaikan:
Status:
```

---

## Troubleshooting Saat Live

### Bot Tidak Membuka Trade

Penyebab umum: strategy belum memberi sinyal, bot masih dalam state `stopped`, pairlist kosong, pair tidak tersedia, saldo tidak cukup, `stake_amount` di bawah minimum order, `max_open_trades` sudah terpenuhi, exchange menolak order, atau kondisi market tidak memenuhi aturan entry.

```bash
docker compose logs --tail=200
```

Periksa juga status bot, open trades, pairlist, dan log melalui FreqUI.

### Bot Membuka Terlalu Banyak Trade

Penyebab umum: `max_open_trades` terlalu besar, pair whitelist terlalu banyak, entry logic terlalu longgar, timeframe terlalu kecil, atau strategy kekurangan filter.

Solusi: turunkan `max_open_trades`, batasi pair whitelist, tambahkan filter pada strategy, tinjau entry logic, lalu backtest dan dry-run ulang.

### Stoploss Terlalu Sering Terpicu

Penyebab umum: entry terlalu agresif, stoploss terlalu ketat, market choppy, pair terlalu volatil, timeframe tidak sesuai, atau strategy membuka posisi melawan trend.

Solusi: periksa exit reason dan pair performance, evaluasi kondisi market, uji nilai stoploss berbeda pada backtest, tambahkan trend filter, kurangi pair volatil, dan hentikan bot bila terjadi loss beruntun.

### Profit Habis oleh Fee

Penyebab umum: target ROI terlalu kecil, frekuensi trading terlalu tinggi, spread terlalu besar, pair kurang likuid, timeframe terlalu kecil, atau fee exchange tidak diperhitungkan dalam strategy.

Solusi: periksa struktur fee exchange, gunakan pair lebih likuid, kurangi frekuensi trading, uji timeframe lebih besar, sesuaikan minimal ROI, dan hindari scalping sebelum memahami dampak fee dan slippage.

### FreqUI Tidak Dapat Diakses

Penyebab umum: container mati, API server mati, Apache error, SSL kedaluwarsa, masalah DNS, firewall memblokir, atau konfigurasi reverse proxy salah.

```bash
docker compose ps
curl http://127.0.0.1:8080
sudo systemctl status apache2
sudo apachectl configtest
sudo ufw status
```

### Disk VPS Penuh

Gejala: bot error, database error, log gagal ditulis, container gagal start, atau server lambat.

```bash
df -h
docker system df
```

Solusi: hapus log lama yang aman dihapus, backup lalu hapus file yang tidak diperlukan, bersihkan resource Docker yang tidak terpakai, dan perbesar storage bila perlu. Database tidak dihapus tanpa backup.

### Exchange API Rate Limit

Gejala:

```text
Rate limit exceeded
Too many requests
```

Penyebab umum: frekuensi request terlalu tinggi, terlalu banyak pair dipantau, beberapa instance bot berjalan bersamaan, pembatasan dari exchange, atau retry network berlebihan.

Solusi: kurangi jumlah pair, periksa config exchange, periksa jumlah instance bot, baca log error, dan jangan menjalankan banyak bot dengan API key yang sama.

---

## Routine Review

Operations berlanjut setelah bot berjalan. Review dilakukan secara berkala.

**Review harian:** status bot, trade aktif, profit/loss harian, error log, API error, saldo exchange, resource VPS, dan kondisi market.

**Review mingguan:** total trade, winrate, drawdown, pair performance, exit reason, jumlah stoploss, profit per pair, perilaku strategy, dampak fee, dan perbandingan dengan hasil backtest.

**Review bulanan:** kelayakan strategy, perubahan kondisi market, penyesuaian pairlist, keputusan modal (tetap, turun, atau naik), kecukupan resource VPS, kebutuhan update Freqtrade, status backup, dan pembaruan dokumentasi internal.

Catatan review menjadi dasar pengambilan keputusan. Tanpa review, keputusan operasional tidak memiliki dasar data.

---

## Checklist Operasional

### Harian

| Checklist                          | Status |
| ---------------------------------- | ------ |
| Bot berjalan normal                | Wajib  |
| FreqUI dapat diakses               | Wajib  |
| Tidak ada error kritis             | Wajib  |
| Trade aktif wajar                  | Wajib  |
| Loss harian di bawah limit         | Wajib  |
| Resource VPS normal                | Wajib  |
| Exchange tidak bermasalah          | Wajib  |

### Mingguan

| Checklist                          | Status           |
| ---------------------------------- | ---------------- |
| Pair performance direview          | Wajib            |
| Exit reason direview               | Wajib            |
| Drawdown direview                  | Wajib            |
| Jumlah stoploss direview           | Wajib            |
| Log error direview                 | Wajib            |
| Backup diperiksa                   | Wajib            |
| Update Freqtrade dipertimbangkan   | Direkomendasikan |

### Sebelum Update

| Checklist                  | Status           |
| -------------------------- | ---------------- |
| Backup config              | Wajib            |
| Backup strategy            | Wajib            |
| Changelog dibaca           | Wajib            |
| Trade aktif diperiksa      | Wajib            |
| Rollback plan disiapkan    | Direkomendasikan |
| Update saat market tenang  | Direkomendasikan |

### Sebelum Menaikkan Modal

| Checklist                                    | Status |
| -------------------------------------------- | ------ |
| Strategy stabil di dry-run                   | Wajib  |
| Live kecil sudah berjalan cukup lama         | Wajib  |
| Drawdown terkontrol                          | Wajib  |
| Tidak ada error operasional                  | Wajib  |
| Risk limit tertulis                          | Wajib  |
| Pengguna siap menerima loss                  | Wajib  |
| Modal tidak dinaikkan setelah satu profit besar | Wajib |

---

## Glossary

| Istilah           | Penjelasan                                                    |
| ----------------- | ------------------------------------------------------------- |
| Operations        | Disiplin menjalankan dan merawat sistem bot                   |
| Dry-run           | Simulasi trading tanpa order sungguhan                        |
| Live Trading      | Trading dengan order sungguhan di exchange                    |
| Position Sizing   | Pengaturan ukuran posisi per trade                            |
| Exposure          | Total nilai posisi yang sedang terbuka                        |
| Risk Limit        | Batas kerugian yang ditetapkan sebelum trading                |
| Drawdown          | Penurunan nilai akun dari titik tertinggi                     |
| Stoploss          | Batas keluar untuk membatasi kerugian                         |
| Kill Switch       | Prosedur menghentikan bot dengan cepat                        |
| Monitoring        | Pemantauan bot, server, log, dan trade                        |
| Alerting          | Notifikasi otomatis untuk kondisi penting                     |
| FreqUI            | Web interface untuk memantau dan mengontrol Freqtrade         |
| API Key           | Kredensial untuk menghubungkan bot ke exchange                |
| IP Whitelist      | Pembatasan akses API hanya dari IP tertentu                   |
| Reverse Proxy     | Komponen yang meneruskan request dari domain ke service lokal |
| SSL               | Enkripsi koneksi web melalui HTTPS                            |
| VPS               | Server virtual untuk menjalankan bot                          |
| Incident Response | Prosedur menangani masalah operasional                        |
| Backup            | Salinan file penting untuk pemulihan                          |
| Changelog         | Catatan perubahan versi software                              |
| Rollback          | Pengembalian sistem ke versi sebelumnya                       |

---

## Ringkasan

* Live trading adalah fase operations, bukan sekadar mengubah `dry_run` menjadi `false`; seluruh parameter terkait ditinjau ulang sebelum live.
* Dry-run digunakan untuk validasi tetapi tidak merepresentasikan kondisi live secara penuh; dijalankan dalam periode yang cukup panjang.
* Pre-live checklist diselesaikan sebelum bot menggunakan dana sungguhan.
* Manajemen modal dan position sizing diukur dari skenario kerugian, bukan dari target profit.
* Risk limit harian dan mingguan ditetapkan secara tertulis sebelum live untuk mencegah keputusan berbasis emosi.
* Monitoring mencakup sisi trading dan infrastruktur, melalui log, FreqUI, dan alert yang volumenya seimbang.
* Prosedur menghentikan bot (kill switch) dikuasai sebelum live; menghentikan bot adalah keputusan operasional yang valid.
* Server memerlukan maintenance, update OS, update Freqtrade, dan backup berkala dengan secret yang dijaga di luar repository publik.
* Insiden ditangani dengan prosedur tertulis dan dicatat untuk mencegah pengulangan.
* Strategy direview secara harian, mingguan, dan bulanan berdasarkan data, bukan asumsi.

---

## Disclaimer

Artikel ini ditulis untuk tujuan edukasi. Crypto trading memiliki risiko tinggi. Bot trading tidak menjamin profit. Live trading dapat menyebabkan kehilangan dana sungguhan.

Contoh command, checklist, dan parameter dalam artikel ini perlu disesuaikan dengan kondisi masing-masing pengguna. Jangan menjalankan bot live sebelum memahami strategy, config, API key, risiko exchange, keamanan server, stoploss, position sizing, dan prosedur menghentikan bot. Jangan menggunakan dana kebutuhan hidup, dana darurat, dana pinjaman, atau dana operasional keluarga untuk trading.

Setiap keputusan teknis dan trading adalah tanggung jawab masing-masing pengguna.

---

2026 - Kenshin Himura
