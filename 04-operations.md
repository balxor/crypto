# Artikel 4 - Operations

# Dari Dry-run ke Live: Mengoperasikan Freqtrade dengan Aman

> Artikel ini membahas transisi dari dry-run ke live, manajemen modal, position sizing, monitoring bot, log, FreqUI, alerting, kapan harus menghentikan bot, maintenance server, update Freqtrade, backup, dan troubleshooting saat bot berjalan.

---

## Daftar Isi

1. [Tujuan Artikel](#tujuan-artikel)
2. [Mindset Operations](#mindset-operations)
3. [Dry-run](#dry-run)
4. [Pre-live Checklist](#pre-live-checklist)
5. [Transisi dari Dry-run ke Live](#transisi-dari-dry-run-ke-live)
6. [Manajemen Modal](#manajemen-modal)
7. [Position Sizing](#position-sizing)
8. [Risk Limit Harian dan Mingguan](#risk-limit-harian-dan-mingguan)
9. [Monitoring Bot](#monitoring-bot)
10. [Membaca Log](#membaca-log)
11. [Monitoring Melalui FreqUI](#monitoring-melalui-frequi)
12. [Alerting](#alerting)
13. [Kapan Harus Menghentikan Bot](#kapan-harus-menghentikan-bot)
14. [Kill Switch Plan](#kill-switch-plan)
15. [Maintenance Server](#maintenance-server)
16. [Update OS](#update-os)
17. [Update Freqtrade](#update-freqtrade)
18. [Backup Konfigurasi dan Strategy](#backup-konfigurasi-dan-strategy)
19. [Incident Response Ringan](#incident-response-ringan)
20. [Troubleshooting Umum Saat Live](#troubleshooting-umum-saat-live)
21. [Routine Review](#routine-review)
22. [Checklist Operasional](#checklist-operasional)
23. [Glossary](#glossary)
24. [Key Takeaways](#key-takeaways)
25. [Disclaimer](#disclaimer)

---

## Tujuan Artikel

Artikel ini adalah lanjutan dari Artikel 3 - Strategy. Jika artikel sebelumnya membahas cara menulis dan menguji strategy, artikel ini membahas cara mengoperasikan bot ketika sudah mendekati atau masuk fase live. Pada tahap ini, masalahnya bukan lagi hanya:

```text
Apakah strategy bisa profit di backtest?
```

Pertanyaan yang lebih penting adalah:

```text
Apakah bot bisa dijalankan dengan risiko yang terkontrol?
Apakah server stabil?
Apakah API key aman?
Apakah log bisa dipantau?
Apakah ada rencana berhenti?
Apakah modal dikelola dengan benar?
Apakah pengguna tahu apa yang harus dilakukan ketika terjadi error?
```

Dalam konteks gamer, ini adalah fase setelah build selesai diuji di training ground. Live trading bukan training dummy. Jika masuk tanpa persiapan, kerusakan tidak hanya terjadi pada karakter atau resource digital. Kerusakan terjadi pada uang sungguhan. Tujuan artikel ini adalah membantu pembaca memahami operasi bot secara disiplin:

* Mengelola Transisi dari Dry-run ke Live.
* Mengatur Modal dan Position Sizing.
* Memantau Bot Melalui Log, FreqUI, dan Alert.
* Menentukan Kapan Bot Harus Dihentikan.
* Melakukan Maintenance Server.
* Melakukan Backup Config dan Strategy.
* Menangani Error Umum Saat Live.
* Membuat Rutinitas Review.

---

## Mindset Operations

Operations adalah disiplin menjalankan sistem. Dalam trading bot, operations bukan bagian yang sifatnya hanya tambahan. Operations adalah bagian inti. Strategy yang baik tetap bisa bermasalah jika operations buruk. Contoh:

| Strategy | Operations | Risiko                                                     |
| -------- | ---------- | ---------------------------------------------------------- |
| Bagus    | Buruk      | Bot bisa gagal karena config, server, API, atau monitoring |
| Buruk    | Bagus      | Bot tetap bisa rugi karena logic strategy lemah            |
| Bagus    | Bagus      | Risiko lebih terkontrol, tetapi profit tetap tidak dijamin |
| Buruk    | Buruk      | Risiko paling tinggi                                       |

Operations mencakup:

* Cara Bot Dijalankan.
* Cara Bot Dihentikan.
* Cara Modal Dibatasi.
* Cara Error Dipantau.
* Cara Update Dilakukan.
* Cara Backup Disimpan.
* Cara Incident Ditangani.
* Cara Strategy Dievaluasi.

Gamer biasanya memahami konsep ini dengan baik. Di game, build kuat tidak cukup. Party tetap butuh role yang jelas, resource management, cooldown timing, mekanik boss, dan keputusan mundur. Trading bot juga sama. Strategy adalah build. Operations adalah cara build itu dijalankan dalam kondisi nyata.

---

## Dry-run

Dry-run adalah simulasi. Dry-run membantu melihat perilaku bot tanpa mengirim order sungguhan ke exchange. Dry-run berguna untuk:

* Melihat Apakah Bot Berjalan.
* Melihat Apakah Strategy Memberi Sinyal.
* Melihat Apakah Config Benar.
* Melihat Apakah Pairlist Berjalan.
* Melihat Apakah FreqUI Bisa Dipakai.
* Melihat Apakah Log Bersih dari Error.
* Melatih Cara Start, Stop, dan Restart Bot.

Namun, dry-run tidak sama dengan live. Perbedaan penting:

| Aspek             | Dry-run                | Live                    |
| ----------------- | ---------------------- | ----------------------- |
| Order             | Simulasi               | Order sungguhan         |
| Uang              | Simulasi               | Uang sungguhan          |
| Slippage          | Tidak selalu realistis | Nyata                   |
| Eksekusi exchange | Tidak sepenuhnya nyata | Nyata                   |
| Psikologi         | Lebih tenang           | Lebih berat             |
| Error API         | Bisa muncul            | Bisa berdampak langsung |
| Risiko finansial  | Tidak langsung         | Langsung                |

Dry-run adalah training ground. Live trading adalah arena sebenarnya. Jangan masuk live hanya karena dry-run terlihat hijau selama beberapa jam. Dry-run perlu berjalan cukup lama agar pembaca memahami pola bot dalam kondisi market yang berbeda.

---

## Pre-live Checklist

Sebelum mengubah `dry_run` menjadi `false`, lakukan checklist berikut.

| Checklist                                       | Status           |
| ----------------------------------------------- | ---------------- |
| Strategy Sudah Dibaca dan Dipahami              | Wajib            |
| Strategy Sudah Dibacktest                       | Wajib            |
| Strategy Sudah Diuji di Beberapa Periode Market | Wajib            |
| Strategy Sudah Dijalankan dalam Dry-run         | Wajib            |
| Dry-run Tidak Menghasilkan Error Kritis         | Wajib            |
| API Key Tidak Memiliki Permission Withdrawal    | Wajib            |
| IP Whitelist Aktif Jika Exchange Mendukung      | Direkomendasikan |
| Config Live Tidak Disimpan di Repository Publik | Wajib            |
| Stake Amount Sudah Sesuai Modal Kecil           | Wajib            |
| Max Open Trades Sudah Dibatasi                  | Wajib            |
| Stoploss Sudah Aktif                            | Wajib            |
| Pairlist Sudah Dipilih Secara Sadar             | Wajib            |
| FreqUI Bisa Diakses Aman                        | Wajib            |
| Password FreqUI Kuat                            | Wajib            |
| VPS Sudah Diamankan                             | Wajib            |
| Backup Config dan Strategy Sudah Ada            | Wajib            |
| Cara Stop Bot Sudah Dipahami                    | Wajib            |
| Kill Switch Plan Sudah Ditulis                  | Wajib            |

Jika ada item wajib yang belum terpenuhi, jangan lanjut live. Live trading tidak boleh menjadi tempat pertama untuk menemukan kesalahan dasar.

---

## Transisi dari Dry-run ke Live

Transisi ke live harus dilakukan perlahan. Jangan mengubah banyak hal sekaligus. Urutan yang lebih aman:

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
Naikkan modal hanya jika benar-benar paham risikonya
```

Perubahan utama dari dry-run ke live biasanya ada pada config. Contoh parameter dry-run:

```json
"dry_run": true,
"dry_run_wallet": 1000
```

Untuk live:

```json
"dry_run": false
```

Namun, perubahan ini tidak boleh dilakukan sendirian tanpa review parameter lain. Sebelum live, periksa kembali:

| Parameter         | Hal yang Diperiksa                 |
| ----------------- | ---------------------------------- |
| `stake_amount`    | Jangan terlalu besar               |
| `max_open_trades` | Jangan terlalu banyak              |
| `pair_whitelist`  | Gunakan pair likuid                |
| `stoploss`        | Harus aktif dan masuk akal         |
| `trading_mode`    | Gunakan spot untuk pemula          |
| `api_server`      | Jangan expose langsung ke internet |
| `exchange.key`    | Pastikan API key benar             |
| `exchange.secret` | Pastikan tidak bocor               |
| `initial_state`   | Lebih aman mulai dari `stopped`    |

Contoh prinsip awal:

```text
Live pertama bukan untuk mencari profit besar.
Live pertama untuk memastikan sistem bekerja dengan risiko kecil.
```

Jika menggunakan modal kecil, kerugian awal dapat menjadi biaya belajar yang terkontrol. Jika langsung menggunakan modal besar, kesalahan kecil bisa menjadi mahal.

---

## Manajemen Modal

Manajemen modal adalah dasar operations. Tanpa manajemen modal, strategy yang baik tetap bisa menghancurkan akun. Prinsip utama:

* Gunakan Modal yang Siap Rugi.
* Jangan Gunakan Dana Darurat.
* Jangan Gunakan Uang Pinjaman.
* Jangan Gunakan Uang Operasional Keluarga.
* Jangan Menggunakan Seluruh Saldo Exchange untuk Bot.
* Pisahkan Modal Trading Bot dari Modal Lain.
* Mulai dari Ukuran Kecil.
* Naikkan Modal Hanya Setelah Evaluasi.

Contoh pembagian modal:

| Total Dana Crypto | Modal Bot Awal | Catatan                            |
| ----------------: | -------------: | ---------------------------------- |
|        1.000 USDT |       100 USDT | 10% digunakan untuk testing live   |
|        5.000 USDT |       250 USDT | Mulai kecil walau dana lebih besar |
|       10.000 USDT |       500 USDT | Tetap perlu evaluasi bertahap      |

Tabel di atas hanya contoh edukasi, bukan rekomendasi finansial. Tujuannya menunjukkan prinsip bahwa live pertama harus kecil. Jangan membuat live pertama menjadi all-in test.

---

## Position Sizing

Position sizing adalah cara menentukan ukuran setiap trade. Dalam Freqtrade, ukuran posisi dapat dipengaruhi oleh beberapa parameter, antara lain:

| Parameter                | Fungsi                           |
| ------------------------ | -------------------------------- |
| `stake_amount`           | Modal per trade                  |
| `max_open_trades`        | Jumlah posisi aktif maksimal     |
| `tradable_balance_ratio` | Porsi saldo yang boleh digunakan |
| Pairlist                 | Aset yang boleh diperdagangkan   |
| Stoploss                 | Batas kerugian per trade         |

Contoh sederhana:

```json
"stake_currency": "USDT",
"stake_amount": 20,
"max_open_trades": 3
```

Dengan konfigurasi ini, exposure maksimal kasar adalah:

```text
20 USDT x 3 posisi = 60 USDT
```

Jika stoploss strategy adalah minus 5%, potensi loss per posisi sekitar:

```text
20 USDT x 5% = 1 USDT
```

Jika tiga posisi terkena stoploss:

```text
1 USDT x 3 = 3 USDT
```

Perhitungan ini masih sederhana. Dalam kondisi live, fee, slippage, spread, dan eksekusi dapat membuat hasil berbeda. Namun, pendekatan ini membantu memahami risiko sebelum bot berjalan. Prinsip position sizing:

* Jangan Mengukur Risiko dari Profit Impian.
* Ukur Risiko dari Skenario Salah.
* Hitung Exposure Maksimal.
* Hitung Risiko Jika Semua Posisi Kena Stoploss.
* Jangan Biarkan Satu Trade Menghancurkan Akun.
* Jangan Biarkan Satu Pair Mendominasi Risiko.

Dalam game, position sizing mirip pengaturan resource sebelum raid. Jangan membawa semua potion untuk satu percobaan jika belum tahu mekanik boss.

---

## Risk Limit Harian dan Mingguan

Selain stoploss per trade, pembaca perlu memiliki batas risiko harian dan mingguan. Contoh aturan manual:

| Kondisi                                    | Aksi                           |
| ------------------------------------------ | ------------------------------ |
| Loss Harian Mencapai 2% dari Modal Bot     | Stop Bot dan Review            |
| Loss Mingguan Mencapai 5% dari Modal Bot   | Stop Bot Minimal Beberapa Hari |
| Tiga Stoploss Beruntun                     | Stop Bot dan Cek Market        |
| Exchange Error Berulang                    | Stop Bot                       |
| VPS Tidak Stabil                           | Stop Bot                       |
| Strategy Berperilaku Tidak Sesuai Backtest | Stop Bot                       |

Angka di atas hanya contoh edukasi. Setiap pengguna harus menentukan batas sesuai profil risiko masing-masing. Yang penting adalah memiliki batas tertulis sebelum live. Tanpa batas tertulis, keputusan sering dikendalikan emosi. Ketika bot profit, pengguna ingin memperbesar modal. Ketika bot loss, pengguna ingin membalas. Keduanya berbahaya jika tidak ada aturan.

---

## Monitoring Bot

Monitoring adalah proses memantau kondisi bot secara rutin. Monitoring bukan berarti melihat chart setiap menit. Monitoring berarti memahami apakah sistem berjalan normal. Hal yang perlu dipantau:

* Status Bot.
* Trade Aktif.
* Trade History.
* Profit dan Loss.
* Drawdown.
* Exit Reason.
* Error Log.
* API Error.
* Koneksi Exchange.
* Saldo Exchange.
* CPU dan RAM VPS.
* Disk Space.
* Status Docker Container.
* Status Apache atau Reverse Proxy.
* SSL Certificate.
* Update OS dan Freqtrade.

Monitoring dapat dilakukan melalui:

| Alat              | Fungsi                          |
| ----------------- | ------------------------------- |
| FreqUI            | Melihat status bot dan trade    |
| Log Docker        | Melihat detail proses bot       |
| Telegram          | Menerima notifikasi             |
| SSH               | Memeriksa server                |
| htop              | Memantau resource server        |
| df                | Memantau storage                |
| systemctl         | Memantau service seperti Apache |
| docker compose ps | Memantau container              |

Command dasar:

```bash
docker compose ps
```

```bash
docker compose logs -f
```

```bash
htop
```

```bash
df -h
```

```bash
free -h
```

---

## Membaca Log

Log adalah combat log untuk bot. Jika terjadi masalah, log biasanya menjadi tempat pertama untuk mencari petunjuk. Command melihat log:

```bash
cd ~/freqtrade
docker compose logs -f
```

Melihat log terakhir saja:

```bash
docker compose logs --tail=200
```

Melihat log dan mencari error:

```bash
docker compose logs | grep -i error
```

Melihat warning:

```bash
docker compose logs | grep -i warning
```

Hal yang perlu diperhatikan dalam log:

| Log                | Arti Umum                              |
| ------------------ | -------------------------------------- |
| Exchange Error     | Ada masalah komunikasi dengan exchange |
| Network Error      | Ada masalah koneksi                    |
| Insufficient Funds | Saldo tidak cukup                      |
| Pair Not Available | Pair tidak tersedia di exchange        |
| Invalid API Key    | API key salah atau permission kurang   |
| Rate Limit         | Terlalu banyak request ke exchange     |
| JSON Error         | Config tidak valid                     |
| Strategy Error     | Ada error di file strategy             |
| Database Error     | Ada masalah database lokal             |

Jangan mengabaikan log yang berulang. Satu warning kecil mungkin tidak kritis. Tetapi warning yang terus muncul bisa menjadi tanda masalah operasional.

---

## Monitoring Melalui FreqUI

FreqUI membantu pengguna melihat kondisi bot melalui web interface. Hal yang bisa dipantau dari FreqUI:

* Status Bot.
* Open Trades.
* Closed Trades.
* Profit dan Loss.
* Pair yang Aktif.
* Chart.
* Trade Duration.
* Entry dan Exit.
* Control Bot.
* Beberapa Informasi Strategy.

FreqUI memudahkan monitoring, tetapi tetap perlu diamankan. Prinsip keamanan FreqUI:

* Gunakan Password Kuat.
* Gunakan HTTPS.
* Jangan Expose API Server Langsung ke Internet.
* Gunakan Reverse Proxy.
* Batasi Akses Jika Memungkinkan.
* Jangan Membagikan Screenshot yang Berisi Informasi Sensitif.
* Jangan Gunakan Username dan Password Default.

Jika FreqUI tidak bisa diakses, cek urutan berikut:

```text
Browser
    ↓
DNS
    ↓
HTTPS / SSL
    ↓
Apache Reverse Proxy
    ↓
Freqtrade API
    ↓
Docker Container
```

Command cek Freqtrade API lokal:

```bash
curl http://127.0.0.1:8080
```

Jika command ini gagal dari server, masalah ada di Freqtrade API atau container. Jika command ini berhasil tetapi domain gagal, masalah kemungkinan ada di DNS, Apache, firewall, atau SSL.

---

## Alerting

Alerting membantu pengguna mengetahui kondisi bot tanpa harus terus membuka dashboard. Freqtrade dapat dikontrol dan dimonitor melalui WebUI atau Telegram. Jika menggunakan Telegram, pastikan aksesnya aman. Hal yang bisa dikirim melalui alert:

* Bot Start.
* Bot Stop.
* Entry Order.
* Exit Order.
* Profit dan Loss.
* Error Tertentu.
* Status Harian.
* Trade Summary.

Prinsip penggunaan alert:

* Aktifkan Alert Penting.
* Jangan Terlalu Banyak Alert yang Tidak Berguna.
* Pastikan Alert Masuk ke Akun yang Aman.
* Jangan Masukkan Bot ke Grup Besar.
* Jangan Berikan Akses Command ke Orang yang Tidak Dipercaya.
* Jangan Bagikan Token Telegram.
* Jangan Commit Token Telegram ke GitHub.

Alert yang terlalu ramai akan diabaikan. Alert yang terlalu sedikit membuat masalah terlambat diketahui. Cari keseimbangan/balancing.

---

## Kapan Harus Menghentikan Bot

Mengetahui kapan harus menghentikan bot sama pentingnya dengan mengetahui kapan menjalankannya. Bot sebaiknya dihentikan jika:

* Loss Harian Melewati Batas.
* Loss Mingguan Melewati Batas.
* Market Mengalami Volatilitas Ekstrem.
* Exchange Mengalami Gangguan.
* API Error Berulang.
* VPS Tidak Stabil.
* Strategy Berperilaku Tidak Sesuai Ekspektasi.
* Stoploss Terlalu Sering Terpicu.
* Pair Tiba-Tiba Tidak Likuid.
* Ada Berita Besar yang Membuat Market Tidak Normal.
* Config Baru Belum Diuji.
* Update Bot Belum Diverifikasi.
* Pengguna Tidak Bisa Melakukan Monitoring.

Bot tidak harus selalu aktif. Tidak trading juga merupakan keputusan. Dalam game, tidak masuk raid ketika koneksi lag adalah keputusan yang sehat. Dalam trading, tidak menjalankan bot ketika kondisi tidak jelas juga keputusan yang sehat.

---

## Kill Switch Plan

Kill switch plan adalah rencana menghentikan bot dengan cepat. Setiap pengguna harus tahu cara mematikan bot sebelum bot live. Command stop container:

```bash
cd ~/freqtrade
docker compose down
```

Command pause atau stop melalui FreqUI juga dapat digunakan jika tersedia dan bot masih responsif. Namun, jika FreqUI tidak responsif, gunakan SSH dan Docker. Checklist kill switch:

| Situasi                 | Aksi                                  |
| ----------------------- | ------------------------------------- |
| Bot Error Berulang      | Stop Container                        |
| Exchange API Bermasalah | Stop Bot                              |
| Strategy Salah Entry    | Stop Bot dan Review                   |
| VPS Resource Penuh      | Stop Bot dan Cek Server               |
| FreqUI Tidak Responsif  | Masuk SSH dan Cek Docker              |
| Config Salah            | Stop Bot, Backup, Perbaiki Config     |
| Market Ekstrem          | Stop Bot Jika Risiko Tidak Terkontrol |

Setelah stop bot, jangan langsung start ulang. Lakukan review:

```text
Apa penyebabnya?
Apakah ada trade aktif?
Apakah exchange masih memiliki open order?
Apakah saldo sesuai?
Apakah config berubah?
Apakah strategy error?
Apakah log menunjukkan masalah?
```

Jika ada open order di exchange, cek langsung di exchange. Jangan hanya percaya dashboard bot jika sedang ada error.

---

## Maintenance Server

VPS perlu dirawat. Server yang tidak dirawat dapat menjadi sumber masalah. Hal yang perlu dipantau:

* CPU Usage.
* RAM Usage.
* Disk Usage.
* Network Stability.
* Docker Container.
* Apache Status.
* SSL Certificate.
* OS Update.
* Log Size.
* Backup.
* Unauthorized Login Attempt.

Command dasar:

```bash
htop
```

```bash
free -h
```

```bash
df -h
```

```bash
docker system df
```

```bash
sudo systemctl status apache2
```

```bash
sudo ufw status
```

Cek login SSH:

```bash
last
```

Cek auth log:

```bash
sudo tail -n 100 /var/log/auth.log
```

Jika disk penuh, bot bisa bermasalah. Cek penggunaan Docker:

```bash
docker system df
```

Bersihkan resource Docker yang tidak digunakan dengan hati-hati:

```bash
docker system prune
```

Jangan menjalankan command pembersihan tanpa memahami efeknya. Pastikan backup sudah tersedia sebelum melakukan maintenance besar.

---

## Update OS

Update OS penting untuk keamanan.

Command update:

```bash
sudo apt update
```

```bash
sudo apt upgrade -y
```

Cek apakah reboot diperlukan:

```bash
cat /var/run/reboot-required
```

Jika file tersebut ada, server membutuhkan reboot. Reboot server:

```bash
sudo reboot
```

Sebelum reboot:

* Stop Bot Jika Diperlukan.
* Pastikan Tidak Ada Operasi Penting.
* Catat Trade Aktif.
* Pastikan Bisa Login SSH Kembali.
* Pastikan Docker Container Bisa Start Ulang.
* Pastikan Apache Bisa Start Ulang.

Setelah reboot:

```bash
docker compose ps
```

```bash
sudo systemctl status apache2
```

```bash
docker compose logs --tail=100
```

Jangan update OS sembarangan ketika bot sedang berada dalam kondisi market sensitif.

---

## Update Freqtrade

Freqtrade perlu diupdate karena exchange API, dependency, bugfix, dan fitur dapat berubah. Jika menggunakan Docker, pola umum update:

```bash
cd ~/freqtrade
docker compose pull
docker compose up -d
```

Setelah update, cek container:

```bash
docker compose ps
```

Cek log:

```bash
docker compose logs --tail=200
```

Sebelum update Freqtrade:

* Backup Config.
* Backup Strategy.
* Backup Database Jika Dibutuhkan.
* Baca Changelog.
* Pastikan Tidak Ada Trade Penting.
* Siapkan Waktu untuk Rollback Jika Perlu.

Setelah update:

* Cek Bot Bisa Start.
* Cek FreqUI Bisa Diakses.
* Cek Strategy Tidak Error.
* Cek Log Tidak Memiliki Error Kritis.
* Jalankan Dry-run Jika Menguji Perubahan Besar.
* Jangan Langsung Mengubah Banyak Hal Sekaligus.

Jika update menyebabkan error, jangan panik. Stop bot, baca log, cek changelog, dan rollback jika perlu.

---

## Backup Konfigurasi dan Strategy

Backup adalah bagian wajib operations. File penting:

| File atau Folder              | Fungsi                                      |
| ----------------------------- | ------------------------------------------- |
| `user_data/config.json`       | Config utama                                |
| `user_data/strategies/`       | File strategy                               |
| `user_data/data/`             | Data historis                               |
| `user_data/backtest_results/` | Hasil backtest                              |
| `user_data/logs/`             | Log                                         |
| Database Freqtrade            | Riwayat trade dan data operasional tertentu |

Namun, jangan upload secret ke repository publik. Untuk GitHub, gunakan file example:

```text
config.example.json
config.dryrun.example.json
```

Jangan upload:

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

Contoh backup manual:

```bash
cd ~/freqtrade

tar -czf freqtrade-backup-$(date +%Y%m%d).tar.gz \
  user_data/config.json \
  user_data/strategies \
  user_data/backtest_results
```

Jika config berisi secret, simpan backup di tempat aman. Jangan menyimpan backup berisi API key di lokasi publik.

---

## Incident Response Ringan

Incident response adalah langkah ketika terjadi masalah. Tujuannya adalah membatasi kerusakan, memahami penyebab, dan mencegah kejadian berulang.

### Contoh Incident: API Key Error

Gejala:

```text
Invalid API key
Authentication failed
Permission denied
```

Langkah:

1. Stop Bot.
2. Cek Permission API di Exchange.
3. Cek IP Whitelist.
4. Cek Apakah API Key Baru Saja Diubah.
5. Cek Config.
6. Jangan Share API Key di Chat Publik.
7. Rotate API Key Jika Ada Risiko Bocor.
8. Start Bot Kembali dalam Kondisi Aman.

### Contoh Incident: VPS Down

Langkah:

1. Cek Panel Provider VPS.
2. Cek Apakah Server Bisa Ping.
3. Cek Apakah SSH Bisa Login.
4. Cek Status Docker.
5. Cek Status Apache.
6. Cek Log Setelah Server Hidup.
7. Cek Exchange Langsung untuk Open Order.
8. Jangan Asumsikan Bot Berjalan Normal Sebelum Diverifikasi.

### Contoh Incident: Bot Membuka Trade Tidak Sesuai Ekspektasi

Langkah:

1. Stop Bot.
2. Cek Trade di FreqUI.
3. Cek Trade di Exchange.
4. Baca Log Entry.
5. Cek Strategy Code.
6. Cek Pair dan Timeframe.
7. Cek Apakah Config Berubah.
8. Jalankan Backtest Ulang Jika Perlu.
9. Jalankan Dry-run Sebelum Live Kembali.

### Contoh Incident: Loss Beruntun

Langkah:

1. Stop Bot Jika Sudah Melewati Risk Limit.
2. Cek Market Condition.
3. Cek Exit Reason.
4. Cek Pair yang Paling Banyak Rugi.
5. Cek Apakah Stoploss Terlalu Ketat atau Terlalu Longgar.
6. Cek Apakah Strategy Cocok dengan Market Saat Ini.
7. Jangan Langsung Menaikkan Modal.
8. Jangan Revenge Trading.

Incident harus dicatat. Format catatan sederhana:

```text
Tanggal:
Waktu:
Strategy:
Pair:
Masalah:
Dampak:
Aksi:
Penyebab Sementara:
Perbaikan:
Status:
```

---

## Troubleshooting Umum Saat Live

### Bot Tidak Membuka Trade

Kemungkinan penyebab:

* Strategy Tidak Memberi Sinyal.
* Bot Masih Dalam State Stopped.
* Pairlist Kosong.
* Pair Tidak Tersedia.
* Saldo Tidak Cukup.
* Stake Amount Terlalu Kecil.
* Max Open Trades Sudah Tercapai.
* Exchange Menolak Order.
* Market Tidak Memenuhi Kondisi Entry.

Command cek:

```bash
docker compose logs --tail=200
```

Cek FreqUI:

```text
Status bot
Open trades
Pairlist
Logs
```

### Bot Membuka Trade Terlalu Banyak

Kemungkinan penyebab:

* `max_open_trades` Terlalu Besar.
* Pair Whitelist Terlalu Banyak.
* Entry Logic Terlalu Longgar.
* Timeframe Terlalu Kecil.
* Strategy Tidak Memiliki Filter Cukup.
* Stake Amount Terlalu Kecil Sehingga Banyak Posisi Terbuka.

Solusi:

* Turunkan `max_open_trades`.
* Batasi Pair Whitelist.
* Tambahkan Filter Strategy.
* Review Entry Logic.
* Backtest Ulang.
* Dry-run Ulang.

### Stoploss Terlalu Sering Kena

Kemungkinan penyebab:

* Entry Terlalu Agresif.
* Stoploss Terlalu Ketat.
* Market Sedang Choppy.
* Pair Terlalu Volatil.
* Timeframe Tidak Cocok.
* Strategy Melawan Trend.

Solusi:

* Cek Exit Reason.
* Cek Pair Performance.
* Cek Market Condition.
* Uji Stoploss Berbeda di Backtest.
* Tambahkan Trend Filter.
* Kurangi Pair Volatil.
* Stop Bot Jika Loss Beruntun.

### Profit Kecil Hilang Karena Fee

Kemungkinan penyebab:

* Target ROI Terlalu Kecil.
* Strategy Terlalu Sering Trading.
* Spread Terlalu Besar.
* Pair Kurang Likuid.
* Timeframe Terlalu Kecil.
* Fee Exchange Tidak Diperhitungkan.

Solusi:

* Periksa Fee Exchange.
* Gunakan Pair Lebih Likuid.
* Kurangi Frekuensi Trading.
* Uji Timeframe Lebih Besar.
* Sesuaikan Minimal ROI.
* Hindari Scalping Jika Belum Memahami Fee dan Slippage.

### FreqUI Tidak Bisa Diakses

Kemungkinan penyebab:

* Container Mati.
* API Server Mati.
* Apache Error.
* SSL Expired.
* DNS Bermasalah.
* Firewall Memblokir Akses.
* Reverse Proxy Salah.

Command cek:

```bash
docker compose ps
```

```bash
curl http://127.0.0.1:8080
```

```bash
sudo systemctl status apache2
```

```bash
sudo apachectl configtest
```

```bash
sudo ufw status
```

### Disk VPS Penuh

Gejala:

* Bot Error.
* Database Error.
* Log Tidak Bisa Ditulis.
* Container Gagal Start.
* Server Lambat.

Command cek:

```bash
df -h
```

```bash
docker system df
```

Solusi:

* Hapus Log Lama Jika Aman.
* Backup Lalu Hapus File Tidak Diperlukan.
* Bersihkan Docker Resource Tidak Terpakai.
* Perbesar Storage Jika Perlu.
* Jangan Menghapus Database Tanpa Backup.

### Exchange API Rate Limit

Gejala:

```text
Rate limit exceeded
Too many requests
```

Kemungkinan penyebab:

* Bot Terlalu Sering Request.
* Banyak Pair Dipantau.
* Banyak Instance Bot.
* Exchange Membatasi API.
* Network Retry Terlalu Sering.

Solusi:

* Kurangi Pair.
* Cek Config Exchange.
* Cek Jumlah Instance Bot.
* Cek Log Error.
* Jangan Menjalankan Terlalu Banyak Bot dengan API Key yang Sama.

---

## Routine Review

Operations tidak selesai setelah bot berjalan. Bot perlu direview secara rutin.

### Review Harian

Hal yang diperiksa:

* Status Bot.
* Trade Aktif.
* Profit dan Loss Harian.
* Error Log.
* API Error.
* Saldo Exchange.
* VPS Resource.
* Market Condition.

### Review Mingguan

Hal yang diperiksa:

* Total Trade.
* Winrate.
* Drawdown.
* Pair Performance.
* Exit Reason.
* Stoploss Count.
* Profit per Pair.
* Strategy Behavior.
* Fee Impact.
* Perbandingan dengan Backtest.

### Review Bulanan

Hal yang diperiksa:

* Apakah Strategy Masih Layak.
* Apakah Market Condition Berubah.
* Apakah Pairlist Perlu Disesuaikan.
* Apakah Modal Perlu Tetap, Turun, atau Naik.
* Apakah VPS Masih Cukup.
* Apakah Freqtrade Perlu Update.
* Apakah Backup Berjalan.
* Apakah Dokumentasi Internal Sudah Diperbarui.

Catatan review membantu pembaca mengambil keputusan berdasarkan data. Tanpa review, bot hanya berjalan berdasarkan harapan kosong.

---

## Checklist Operasional

Checklist harian:

| Checklist                        | Status |
| -------------------------------- | ------ |
| Bot Berjalan Normal              | Wajib  |
| FreqUI Bisa Diakses              | Wajib  |
| Tidak Ada Error Kritis           | Wajib  |
| Trade Aktif Masuk Akal           | Wajib  |
| Loss Harian Masih di Bawah Limit | Wajib  |
| VPS Resource Normal              | Wajib  |
| Exchange Tidak Bermasalah        | Wajib  |

Checklist mingguan:

| Checklist                        | Status           |
| -------------------------------- | ---------------- |
| Pair Performance Direview        | Wajib            |
| Exit Reason Direview             | Wajib            |
| Drawdown Direview                | Wajib            |
| Stoploss Count Direview          | Wajib            |
| Log Error Direview               | Wajib            |
| Backup Dicek                     | Wajib            |
| Update Freqtrade Dipertimbangkan | Direkomendasikan |

Checklist sebelum update:

| Checklist                 | Status           |
| ------------------------- | ---------------- |
| Backup Config             | Wajib            |
| Backup Strategy           | Wajib            |
| Baca Changelog            | Wajib            |
| Cek Trade Aktif           | Wajib            |
| Siapkan Rollback Plan     | Direkomendasikan |
| Update Saat Market Tenang | Direkomendasikan |

Checklist sebelum menaikkan modal:

| Checklist                                  | Status |
| ------------------------------------------ | ------ |
| Strategy Stabil di Dry-run                 | Wajib  |
| Live Kecil Sudah Berjalan Cukup Lama       | Wajib  |
| Drawdown Terkontrol                        | Wajib  |
| Tidak Ada Error Operasional                | Wajib  |
| Risk Limit Tertulis                        | Wajib  |
| Pengguna Siap Menerima Loss                | Wajib  |
| Tidak Naik Modal Setelah Satu Profit Besar | Wajib  |

---

## Glossary

| Istilah           | Penjelasan                                                    |
| ----------------- | ------------------------------------------------------------- |
| Operations        | Disiplin menjalankan dan merawat sistem bot                   |
| Dry-run           | Simulasi trading tanpa order sungguhan                        |
| Live Trading      | Trading dengan order sungguhan di exchange                    |
| Position Sizing   | Pengaturan ukuran posisi per trade                            |
| Exposure          | Total risiko atau nilai posisi yang sedang terbuka            |
| Risk Limit        | Batas kerugian yang ditentukan sebelum trading                |
| Drawdown          | Penurunan nilai akun dari titik tertinggi                     |
| Stoploss          | Batas keluar untuk membatasi kerugian                         |
| Kill Switch       | Rencana menghentikan bot dengan cepat                         |
| Monitoring        | Proses memantau bot, server, log, dan trade                   |
| Alerting          | Notifikasi otomatis untuk kondisi penting                     |
| FreqUI            | Web interface untuk memantau dan mengontrol Freqtrade         |
| API Key           | Kredensial untuk menghubungkan bot ke exchange                |
| IP Whitelist      | Pembatasan akses API hanya dari IP tertentu                   |
| Reverse Proxy     | Komponen yang meneruskan request dari domain ke service lokal |
| SSL               | Enkripsi koneksi web melalui HTTPS                            |
| VPS               | Server virtual untuk menjalankan bot                          |
| Incident Response | Langkah menangani masalah operasional                         |
| Backup            | Salinan file penting untuk pemulihan                          |
| Changelog         | Catatan perubahan versi software                              |
| Rollback          | Mengembalikan sistem ke versi sebelumnya                      |

---

## Key Takeaways

* Live trading adalah fase operations, bukan sekadar mengubah `dry_run` menjadi `false`.
* Dry-run membantu testing, tetapi tidak sama dengan live market.
* Pre-live checklist wajib dilakukan sebelum bot memakai uang sungguhan.
* Manajemen modal dan position sizing lebih penting daripada mengejar profit besar.
* Bot harus memiliki risk limit harian dan mingguan.
* Monitoring harus mencakup bot, trade, log, exchange, VPS, dan akses FreqUI.
* Pengguna harus tahu cara menghentikan bot sebelum menjalankan bot live.
* Kill switch plan wajib ada.
* Server perlu maintenance, update, dan backup.
* Strategy harus direview secara berkala.
* Tidak menjalankan bot juga bisa menjadi keputusan yang benar.
* Operations yang baik tidak menjamin profit, tetapi membantu mengurangi risiko kesalahan sistemik.

---

## Disclaimer

Artikel ini hanya untuk tujuan edukasi. Crypto trading memiliki risiko tinggi. Bot trading tidak menjamin profit. Live trading dapat menyebabkan kehilangan dana sungguhan.

Contoh command, checklist, dan parameter dalam artikel ini harus disesuaikan dengan kondisi masing-masing pengguna. Jangan menjalankan bot live sebelum memahami strategy, config, API key, exchange risk, server security, stoploss, position sizing, dan cara menghentikan bot. Jangan menggunakan uang kebutuhan hidup, dana darurat, uang pinjaman, atau uang operasional keluarga untuk trading.

Setiap keputusan teknis dan trading adalah tanggung jawab pribadi masing-masing pengguna.

---

2026 - Kenshin Himura
