# Artikel 2 — Setup

# Membangun Trading Lab: Setup Freqtrade di VPS dari Nol

> Artikel ini membahas persiapan exchange, API key, VPS, Ubuntu Server, instalasi Freqtrade, konfigurasi dasar, FreqUI, Apache reverse proxy, SSL, dry-run, dan troubleshooting instalasi awal.

---

## Daftar Isi

1. [Tujuan Artikel](#tujuan-artikel)
2. [Gambaran Arsitektur](#gambaran-arsitektur)
3. [Prinsip Setup yang Aman](#prinsip-setup-yang-aman)
4. [Persiapan Akun Exchange](#persiapan-akun-exchange)
5. [Membuat API Key dengan Izin Minimal](#membuat-api-key-dengan-izin-minimal)
6. [Memilih VPS](#memilih-vps)
7. [Mengakses VPS Menggunakan SSH](#mengakses-vps-menggunakan-ssh)
8. [Setup Ubuntu Server dari Nol](#setup-ubuntu-server-dari-nol)
9. [Mengamankan Server Secara Dasar](#mengamankan-server-secara-dasar)
10. [Memilih Metode Instalasi Freqtrade](#memilih-metode-instalasi-freqtrade)
11. [Instalasi Freqtrade Menggunakan Docker](#instalasi-freqtrade-menggunakan-docker)
12. [Struktur Folder Freqtrade](#struktur-folder-freqtrade)
13. [Membuat Config Awal](#membuat-config-awal)
14. [Penjelasan Parameter Penting config.json](#penjelasan-parameter-penting-configjson)
15. [Menjalankan Bot dalam Mode Dry-run](#menjalankan-bot-dalam-mode-dry-run)
16. [Pengenalan FreqUI](#pengenalan-frequi)
17. [Setup Apache Reverse Proxy](#setup-apache-reverse-proxy)
18. [SSL Menggunakan Let's Encrypt](#ssl-menggunakan-lets-encrypt)
19. [Menjalankan Bot dengan Docker Compose](#menjalankan-bot-dengan-docker-compose)
20. [Troubleshooting Instalasi Umum](#troubleshooting-instalasi-umum)
21. [Checklist Sebelum Lanjut ke Strategy](#checklist-sebelum-lanjut-ke-strategy)
22. [Key Takeaways](#key-takeaways)
23. [Disclaimer](#disclaimer)

---

## Tujuan Artikel

Artikel ini adalah lanjutan dari Artikel 1 — Foundation.

Jika artikel pertama membahas konsep dasar crypto market, artikel ini mulai masuk ke sisi teknis. Target akhirnya adalah membuat environment Freqtrade yang bisa berjalan dalam mode dry-run.

Pada tahap ini, fokus utama bukan profit.

Fokus utama adalah membangun trading lab yang rapi, aman, dan bisa diuji.

Dalam konteks gamer, tahap ini mirip membangun base sebelum masuk raid:

* Server harus siap.
* Akses harus aman.
* Tool harus terpasang.
* Config harus jelas.
* Bot harus berjalan dalam mode latihan.
* Risiko harus dikontrol sebelum masuk live.

Setelah menyelesaikan artikel ini, pembaca diharapkan memahami:

* Cara menyiapkan akun exchange untuk bot.
* Cara membuat API key dengan permission minimal.
* Cara menyewa dan mengakses VPS.
* Cara menyiapkan Ubuntu Server.
* Cara menginstal Freqtrade menggunakan Docker.
* Cara membuat config dasar.
* Cara menjalankan bot dalam mode dry-run.
* Cara mengakses FreqUI melalui reverse proxy dan SSL.
* Cara membaca error instalasi umum.

---

## Gambaran Arsitektur

Sebelum menjalankan command, pahami dulu gambaran sistemnya.

Arsitektur sederhana Freqtrade:

```text
User Browser
    ↓
Domain + HTTPS
    ↓
Apache Reverse Proxy
    ↓
FreqUI / Freqtrade API
    ↓
Freqtrade Bot
    ↓
Exchange API
    ↓
Crypto Market
```

Penjelasan:

| Komponen             | Fungsi                                                |
| -------------------- | ----------------------------------------------------- |
| User Browser         | Digunakan untuk mengakses FreqUI                      |
| Domain               | Nama domain untuk membuka dashboard                   |
| HTTPS                | Enkripsi koneksi melalui SSL                          |
| Apache Reverse Proxy | Meneruskan request dari domain ke Freqtrade API lokal |
| FreqUI               | Web interface untuk memantau bot                      |
| Freqtrade Bot        | Program utama yang menjalankan strategi               |
| Exchange API         | Jalur komunikasi antara bot dan exchange              |
| Crypto Market        | Market tempat order dieksekusi                        |

Pada setup yang aman, Freqtrade API sebaiknya hanya mendengarkan koneksi lokal:

```text
127.0.0.1:8080
```

Artinya, Freqtrade API tidak langsung dibuka ke internet. Akses dari luar diarahkan melalui Apache dan HTTPS.

---

## Prinsip Setup yang Aman

Sebelum mulai, pegang prinsip berikut.

* Jangan Menggunakan API Key dengan Permission Withdrawal.
* Jangan Menyimpan Secret di Repository Publik.
* Jangan Menjalankan Bot Live Sebelum Dry-run.
* Jangan Menggunakan Modal Besar untuk Testing.
* Jangan Membuka Port Freqtrade API Langsung ke Internet.
* Jangan Mengabaikan Update OS dan Dependency.
* Jangan Menggunakan Password Lemah untuk FreqUI.
* Jangan Menganggap VPS Selalu Aman Secara Default.

Freqtrade adalah alat yang kuat, tetapi kesalahan konfigurasi bisa berbahaya.

Dalam game, salah build mungkin membuat karakter tidak optimal.

Dalam trading bot, salah config bisa membuat bot membuka order yang tidak kamu inginkan.

---

## Persiapan Akun Exchange

Freqtrade membutuhkan exchange yang mendukung API trading.

Sebelum membuat API key, pastikan exchange yang digunakan:

* Memiliki Reputasi yang Baik.
* Mendukung API Trading.
* Mendukung Pair yang Ingin Digunakan.
* Memiliki Volume dan Likuiditas yang Cukup.
* Memiliki Biaya Trading yang Jelas.
* Memiliki Fitur Security Seperti 2FA.
* Mendukung IP Whitelist Jika Tersedia.
* Kompatibel dengan Freqtrade.

Hal yang perlu dilakukan di akun exchange:

1. Aktifkan 2FA.
2. Verifikasi akun sesuai kebutuhan exchange.
3. Pahami aturan deposit dan withdrawal.
4. Pahami fee trading.
5. Pilih pair yang likuid.
6. Jangan langsung menggunakan seluruh modal.

Untuk tahap belajar, gunakan mode dry-run terlebih dahulu.

Dry-run tidak mengirim order sungguhan ke exchange, tetapi tetap membantu memahami perilaku bot.

---

## Membuat API Key dengan Izin Minimal

API key adalah kredensial yang memungkinkan bot terhubung ke exchange.

API key biasanya terdiri dari:

| Komponen   | Fungsi                           |
| ---------- | -------------------------------- |
| API Key    | Identitas akses API              |
| API Secret | Secret untuk autentikasi         |
| Passphrase | Digunakan oleh beberapa exchange |
| Permission | Hak akses API                    |

Permission API harus dibatasi.

Untuk bot trading, permission yang umumnya dibutuhkan:

* Read.
* Trade.

Permission yang tidak boleh diaktifkan:

* Withdraw.
* Transfer Internal Jika Tidak Dibutuhkan.
* Account Management Jika Tidak Dibutuhkan.

Rekomendasi keamanan API key:

* Aktifkan IP Whitelist Jika Exchange Mendukung.
* Gunakan API Key Khusus untuk Bot.
* Jangan Gunakan API Key Utama untuk Eksperimen.
* Jangan Simpan API Key di GitHub.
* Jangan Kirim API Key Melalui Chat Publik.
* Jangan Masukkan API Key ke Screenshot.
* Rotate API Key Jika Pernah Bocor.
* Hapus API Key yang Tidak Digunakan.

Analogi gamer:

API key bukan sekadar password.

API key adalah akses bot ke inventory trading kamu. Jika permission terlalu luas, risiko juga menjadi lebih besar.

---

## Memilih VPS

VPS digunakan agar Freqtrade bisa berjalan stabil tanpa bergantung pada laptop pribadi.

Kriteria VPS yang layak untuk Freqtrade awal:

| Komponen | Rekomendasi Awal                                                  |
| -------- | ----------------------------------------------------------------- |
| OS       | Ubuntu Server LTS                                                 |
| CPU      | 1 sampai 2 vCPU                                                   |
| RAM      | Minimal 2 GB                                                      |
| Storage  | Minimal 20 GB SSD                                                 |
| Network  | Stabil                                                            |
| Lokasi   | Dekat dengan exchange lebih baik, tetapi tidak wajib untuk pemula |
| Akses    | SSH root atau user sudo                                           |

Untuk tahap belajar, VPS kecil sudah cukup.

Jangan langsung menyewa server mahal. Lebih baik mulai dari setup sederhana, lalu naikkan resource jika memang dibutuhkan.

Hal yang perlu diperhatikan saat memilih provider VPS:

* Reputasi Provider.
* Uptime.
* Kemudahan Rebuild OS.
* Backup Snapshot.
* Firewall Panel.
* Lokasi Data Center.
* Harga Bulanan.
* Dukungan IPv4.
* Kemudahan Reset Password atau SSH Key.

---

## Mengakses VPS Menggunakan SSH

Setelah VPS dibuat, biasanya provider akan memberikan:

* IP Public Server.
* Username Awal.
* Password atau SSH Key.
* Informasi OS.

Contoh login menggunakan SSH:

```bash
ssh root@YOUR_SERVER_IP
```

Jika menggunakan SSH key:

```bash
ssh -i ~/.ssh/your_key root@YOUR_SERVER_IP
```

Setelah berhasil login, cek versi OS:

```bash
lsb_release -a
```

Cek user aktif:

```bash
whoami
```

Cek resource server:

```bash
free -h
df -h
nproc
```

Jika baru pertama kali menggunakan VPS, jangan langsung install bot. Lakukan update dasar terlebih dahulu.

---

## Setup Ubuntu Server dari Nol

Update package index:

```bash
sudo apt update
```

Upgrade package:

```bash
sudo apt upgrade -y
```

Install package dasar:

```bash
sudo apt install -y \
  curl \
  wget \
  git \
  unzip \
  htop \
  nano \
  ufw \
  ca-certificates \
  gnupg \
  lsb-release
```

Cek timezone:

```bash
timedatectl
```

Set timezone jika diperlukan:

```bash
sudo timedatectl set-timezone Asia/Jakarta
```

Aktifkan sinkronisasi waktu:

```bash
timedatectl status
```

Waktu server harus akurat karena bot berkomunikasi dengan exchange. Jika waktu server meleset terlalu jauh, request API bisa gagal.

---

## Mengamankan Server Secara Dasar

### Membuat User Baru

Jangan selalu bekerja sebagai root.

Buat user baru:

```bash
adduser freqbot
```

Tambahkan ke group sudo:

```bash
usermod -aG sudo freqbot
```

Login sebagai user baru:

```bash
su - freqbot
```

Tes akses sudo:

```bash
sudo whoami
```

Jika output-nya `root`, user sudah memiliki akses sudo.

### Mengatur Firewall

Izinkan SSH:

```bash
sudo ufw allow OpenSSH
```

Izinkan HTTP dan HTTPS:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Aktifkan firewall:

```bash
sudo ufw enable
```

Cek status firewall:

```bash
sudo ufw status
```

Jangan membuka port Freqtrade API langsung ke internet.

Jika Freqtrade API berjalan di port `8080`, biarkan hanya listen di `127.0.0.1`.

### Menggunakan SSH Key

Untuk setup lebih aman, gunakan SSH key dan matikan password login setelah yakin SSH key berfungsi.

File konfigurasi SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

Parameter yang umum diperhatikan:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

Penting:

Jangan menutup sesi SSH aktif sebelum memastikan login dengan SSH key berhasil dari terminal baru.

---

## Memilih Metode Instalasi Freqtrade

Ada beberapa cara menginstal Freqtrade.

Untuk seri ini, metode utama yang disarankan adalah Docker.

Alasannya:

* Lebih Rapi untuk VPS.
* Lebih Mudah Direproduksi.
* Dependency Lebih Terkontrol.
* Update Lebih Terstruktur.
* Cocok untuk Deployment Jangka Panjang.
* Mengurangi Risiko Konflik Package Python di OS.

Metode native tetap bisa digunakan, tetapi untuk pemula yang ingin menjalankan Freqtrade di VPS, Docker lebih mudah dijaga.

Perbandingan singkat:

| Metode          | Kelebihan                         | Kekurangan                             |
| --------------- | --------------------------------- | -------------------------------------- |
| Docker          | Rapi, konsisten, mudah update     | Perlu memahami Docker Compose          |
| Native Install  | Lebih dekat ke Python environment | Dependency bisa lebih mudah berantakan |
| Manual Advanced | Fleksibel                         | Tidak cocok untuk pemula               |

Artikel ini menggunakan Docker sebagai jalur utama.

---

## Instalasi Freqtrade Menggunakan Docker

### Install Docker

Install package pendukung:

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
```

Buat folder keyring Docker:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Ambil GPG key Docker:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Atur permission key:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Tambahkan repository Docker:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update package index:

```bash
sudo apt update
```

Install Docker dan Docker Compose plugin:

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Cek versi Docker:

```bash
docker --version
docker compose version
```

Tambahkan user ke group docker:

```bash
sudo usermod -aG docker $USER
```

Logout lalu login kembali agar group aktif.

Tes Docker:

```bash
docker run hello-world
```

### Membuat Folder Project

Buat folder kerja:

```bash
mkdir -p ~/freqtrade
cd ~/freqtrade
```

Download file Docker Compose Freqtrade:

```bash
curl -L https://raw.githubusercontent.com/freqtrade/freqtrade/stable/docker-compose.yml \
  -o docker-compose.yml
```

Download image Freqtrade:

```bash
docker compose pull
```

Buat user directory:

```bash
docker compose run --rm freqtrade create-userdir --userdir user_data
```

Buat config awal:

```bash
docker compose run --rm freqtrade new-config --config user_data/config.json
```

Command `new-config` akan menanyakan beberapa parameter awal seperti exchange, stake currency, stake amount, max open trades, timeframe, dan mode dry-run.

Untuk tahap awal, pilih dry-run.

---

## Struktur Folder Freqtrade

Setelah setup awal, struktur folder akan terlihat seperti ini:

```text
freqtrade/
├── docker-compose.yml
└── user_data/
    ├── backtest_results/
    ├── data/
    ├── hyperopts/
    ├── logs/
    ├── notebooks/
    ├── plot/
    ├── strategies/
    └── config.json
```

Penjelasan:

| Path                         | Fungsi                          |
| ---------------------------- | ------------------------------- |
| `docker-compose.yml`         | Konfigurasi container Freqtrade |
| `user_data/config.json`      | Konfigurasi utama bot           |
| `user_data/strategies`       | Folder strategy Python          |
| `user_data/data`             | Data historis market            |
| `user_data/backtest_results` | Hasil backtesting               |
| `user_data/logs`             | Log bot                         |
| `user_data/hyperopts`        | File hyperopt                   |
| `user_data/plot`             | Output chart atau plot          |

Folder `user_data` adalah folder penting.

Jangan sembarangan menghapus folder ini karena berisi config, strategi, data, dan hasil testing.

---

## Membuat Config Awal

Config utama berada di:

```text
user_data/config.json
```

Buka file config:

```bash
nano user_data/config.json
```

Contoh struktur config sederhana:

```json
{
  "max_open_trades": 3,
  "stake_currency": "USDT",
  "stake_amount": 20,
  "dry_run": true,
  "dry_run_wallet": 1000,
  "trading_mode": "spot",
  "timeframe": "5m",
  "exchange": {
    "name": "binance",
    "key": "",
    "secret": "",
    "ccxt_config": {},
    "ccxt_async_config": {},
    "pair_whitelist": [
      "BTC/USDT",
      "ETH/USDT"
    ],
    "pair_blacklist": []
  },
  "pairlists": [
    {
      "method": "StaticPairList"
    }
  ],
  "api_server": {
    "enabled": true,
    "listen_ip_address": "127.0.0.1",
    "listen_port": 8080,
    "verbosity": "error",
    "enable_openapi": false,
    "jwt_secret_key": "CHANGE_THIS_TO_RANDOM_SECRET",
    "ws_token": "CHANGE_THIS_TO_RANDOM_WS_TOKEN",
    "CORS_origins": [],
    "username": "CHANGE_THIS_USERNAME",
    "password": "CHANGE_THIS_PASSWORD"
  },
  "bot_name": "freqtrade-gamers-lab",
  "initial_state": "stopped",
  "force_entry_enable": false,
  "internals": {
    "process_throttle_secs": 5
  }
}
```

Catatan:

* Jangan Gunakan Config Ini untuk Live Tanpa Review.
* Ganti Semua Nilai `CHANGE_THIS`.
* Jangan Commit `config.json` ke GitHub.
* Gunakan `config.example.json` untuk Repository Publik.
* Pastikan `dry_run` Bernilai `true` pada Tahap Belajar.
* Pastikan API Server Listen di `127.0.0.1`.

---

## Penjelasan Parameter Penting config.json

### max_open_trades

```json
"max_open_trades": 3
```

Parameter ini menentukan jumlah posisi maksimal yang dapat dibuka bot secara bersamaan.

Jika nilainya `3`, bot hanya dapat membuka maksimal tiga trade aktif.

Untuk pemula, gunakan angka kecil.

Semakin banyak posisi aktif, semakin besar exposure terhadap market.

### stake_currency

```json
"stake_currency": "USDT"
```

Parameter ini menentukan mata uang yang digunakan sebagai modal trading.

Jika menggunakan pair seperti `BTC/USDT`, stake currency biasanya `USDT`.

### stake_amount

```json
"stake_amount": 20
```

Parameter ini menentukan ukuran modal per trade.

Jika `stake_amount` adalah `20`, bot akan menggunakan sekitar 20 USDT per trade.

Untuk dry-run, angka ini hanya simulasi.

Untuk live, angka ini akan berpengaruh langsung ke order real.

### dry_run

```json
"dry_run": true
```

Parameter ini menentukan apakah bot berjalan dalam mode simulasi.

Jika `true`, bot tidak mengirim order real ke exchange.

Untuk pemula, nilai ini harus tetap `true`.

Jangan ubah ke `false` sebelum memahami risiko live trading.

### dry_run_wallet

```json
"dry_run_wallet": 1000
```

Parameter ini menentukan saldo simulasi yang digunakan dalam dry-run.

Jika `dry_run_wallet` adalah `1000`, bot akan menganggap tersedia 1000 USDT untuk simulasi.

Gunakan angka yang realistis.

Jangan menggunakan angka simulasi yang terlalu besar jika modal live nantinya kecil, karena hasil dry-run bisa membuat ekspektasi tidak realistis.

### trading_mode

```json
"trading_mode": "spot"
```

Parameter ini menentukan mode trading.

Untuk seri ini, gunakan spot.

Futures dan margin tidak disarankan untuk pemula.

### timeframe

```json
"timeframe": "5m"
```

Parameter ini menentukan timeframe candle yang digunakan strategy.

Contoh:

| Timeframe | Arti            |
| --------- | --------------- |
| `5m`      | Candle 5 menit  |
| `15m`     | Candle 15 menit |
| `1h`      | Candle 1 jam    |
| `4h`      | Candle 4 jam    |

Timeframe kecil menghasilkan sinyal lebih sering, tetapi lebih noisy.

Timeframe besar lebih lambat, tetapi sering lebih mudah dianalisis.

### exchange.name

```json
"name": "binance"
```

Parameter ini menentukan exchange yang digunakan.

Nama exchange harus sesuai dengan exchange yang didukung oleh Freqtrade dan CCXT.

Pastikan exchange yang dipilih legal dan sesuai kebutuhan pengguna.

### exchange.key dan exchange.secret

```json
"key": "",
"secret": ""
```

Parameter ini berisi API key dan API secret.

Untuk dry-run, beberapa exchange tetap membutuhkan API key untuk membaca informasi tertentu. Namun, jangan pernah memasukkan API key ke file yang akan diunggah ke GitHub.

Rekomendasi:

* Gunakan File Private untuk Config Asli.
* Gunakan File Example untuk Repository.
* Jangan Upload Secret.
* Jangan Screenshot Config yang Berisi Secret.

### pair_whitelist

```json
"pair_whitelist": [
  "BTC/USDT",
  "ETH/USDT"
]
```

Parameter ini menentukan pair yang boleh diperdagangkan oleh bot.

Untuk pemula, gunakan pair besar dan likuid.

Jangan langsung memasukkan terlalu banyak pair.

### pair_blacklist

```json
"pair_blacklist": []
```

Parameter ini menentukan pair yang dilarang diperdagangkan oleh bot.

Pair blacklist berguna jika menggunakan pairlist dinamis.

### pairlists

```json
"pairlists": [
  {
    "method": "StaticPairList"
  }
]
```

Parameter ini menentukan cara bot memilih pair.

Untuk awal, gunakan `StaticPairList` agar pair yang digunakan jelas dan terkontrol.

### api_server.enabled

```json
"enabled": true
```

Parameter ini mengaktifkan API server Freqtrade.

API server dibutuhkan untuk FreqUI.

### api_server.listen_ip_address

```json
"listen_ip_address": "127.0.0.1"
```

Parameter ini menentukan alamat listen API server.

Gunakan `127.0.0.1` agar API hanya dapat diakses dari server lokal.

Jangan gunakan `0.0.0.0` kecuali benar-benar memahami risiko dan sudah menyiapkan proteksi tambahan.

### api_server.listen_port

```json
"listen_port": 8080
```

Parameter ini menentukan port API server.

Dalam artikel ini, port yang digunakan adalah `8080`.

### api_server.username dan api_server.password

```json
"username": "CHANGE_THIS_USERNAME",
"password": "CHANGE_THIS_PASSWORD"
```

Parameter ini digunakan untuk login ke FreqUI.

Gunakan username dan password yang kuat.

Jangan gunakan password seperti:

```text
admin
password
123456
freqtrade
```

### initial_state

```json
"initial_state": "stopped"
```

Parameter ini menentukan status awal bot ketika dijalankan.

Untuk pemula, `stopped` lebih aman karena bot tidak langsung mulai trading saat container dinyalakan.

---

## Menjalankan Bot dalam Mode Dry-run

Setelah config dibuat, jalankan bot:

```bash
cd ~/freqtrade
docker compose up -d
```

Cek container:

```bash
docker compose ps
```

Cek log:

```bash
docker compose logs -f
```

Jika bot berjalan normal, log akan menampilkan proses startup, koneksi exchange, pairlist, dan status bot.

Karena `initial_state` diset ke `stopped`, bot mungkin belum mulai trading sampai diperintahkan melalui FreqUI atau command yang sesuai.

Untuk menghentikan container:

```bash
docker compose down
```

Untuk restart:

```bash
docker compose restart
```

Untuk update image:

```bash
docker compose pull
docker compose up -d
```

---

## Pengenalan FreqUI

FreqUI adalah web interface untuk mengontrol dan memantau Freqtrade.

Dengan FreqUI, pengguna dapat melihat:

* Status Bot.
* Trade Aktif.
* Trade History.
* Profit dan Loss.
* Pair yang Dipantau.
* Log Dasar.
* Chart.
* Backtesting pada Mode Webserver.
* Konfigurasi Tertentu.

Jika API server aktif dan listen pada `127.0.0.1:8080`, FreqUI dapat diakses dari dalam server melalui:

```text
http://127.0.0.1:8080
```

Namun, untuk akses dari browser lokal pengguna, lebih baik gunakan reverse proxy dengan HTTPS.

Jangan membuka port `8080` langsung ke internet.

---

## Setup Apache Reverse Proxy

Reverse proxy digunakan agar FreqUI dapat diakses melalui domain seperti:

```text
https://bot.example.com
```

Apache akan menerima request HTTPS dari internet, lalu meneruskannya ke Freqtrade API lokal di:

```text
http://127.0.0.1:8080
```

### Install Apache

Install Apache:

```bash
sudo apt install -y apache2
```

Aktifkan module proxy:

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_wstunnel
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod ssl
```

Restart Apache:

```bash
sudo systemctl restart apache2
```

### Buat Virtual Host

Buat file virtual host:

```bash
sudo nano /etc/apache2/sites-available/freqtrade.conf
```

Isi konfigurasi:

```apache
<VirtualHost *:80>
    ServerName bot.example.com

    ProxyPreserveHost On
    ProxyRequests Off

    RewriteEngine On

    ProxyPass /api/v1/message/ws ws://127.0.0.1:8080/api/v1/message/ws
    ProxyPassReverse /api/v1/message/ws ws://127.0.0.1:8080/api/v1/message/ws

    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/

    ErrorLog ${APACHE_LOG_DIR}/freqtrade-error.log
    CustomLog ${APACHE_LOG_DIR}/freqtrade-access.log combined
</VirtualHost>
```

Ganti `bot.example.com` dengan domain yang digunakan.

Aktifkan site:

```bash
sudo a2ensite freqtrade.conf
```

Tes konfigurasi Apache:

```bash
sudo apachectl configtest
```

Reload Apache:

```bash
sudo systemctl reload apache2
```

Pastikan DNS domain sudah mengarah ke IP VPS.

Cek dari browser:

```text
http://bot.example.com
```

Jika Freqtrade container berjalan dan API server aktif, halaman FreqUI akan terbuka.

---

## SSL Menggunakan Let's Encrypt

Setelah reverse proxy HTTP berjalan, aktifkan HTTPS menggunakan Certbot.

### Install Certbot

Install snap jika belum tersedia:

```bash
sudo apt install -y snapd
```

Install core snap:

```bash
sudo snap install core
sudo snap refresh core
```

Install Certbot:

```bash
sudo snap install --classic certbot
```

Buat symbolic link:

```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

### Generate SSL untuk Apache

Jalankan Certbot:

```bash
sudo certbot --apache -d bot.example.com
```

Ikuti instruksi di layar.

Certbot akan membuat konfigurasi SSL dan mengatur redirect HTTPS jika dipilih.

Tes renewal:

```bash
sudo certbot renew --dry-run
```

Setelah SSL aktif, akses FreqUI melalui:

```text
https://bot.example.com
```

Pastikan login FreqUI menggunakan username dan password yang kuat.

---

## Menjalankan Bot dengan Docker Compose

Command dasar yang paling sering digunakan:

### Start Bot

```bash
docker compose up -d
```

### Stop Bot

```bash
docker compose down
```

### Restart Bot

```bash
docker compose restart
```

### Melihat Status

```bash
docker compose ps
```

### Melihat Log

```bash
docker compose logs -f
```

### Update Image

```bash
docker compose pull
docker compose up -d
```

### Masuk ke Container

```bash
docker compose exec freqtrade bash
```

### Menjalankan Command Freqtrade Manual

Contoh melihat help:

```bash
docker compose run --rm freqtrade --help
```

Contoh membuat config baru:

```bash
docker compose run --rm freqtrade new-config --config user_data/config.json
```

Contoh membuat strategy baru:

```bash
docker compose run --rm freqtrade new-strategy --strategy MyFirstStrategy
```

---

## Troubleshooting Instalasi Umum

### Docker Permission Denied

Error:

```text
permission denied while trying to connect to the Docker daemon socket
```

Penyebab umum:

* User Belum Masuk Group Docker.
* Sesi SSH Belum Logout Login Ulang.

Solusi:

```bash
sudo usermod -aG docker $USER
```

Logout dari SSH, lalu login kembali.

Tes:

```bash
docker ps
```

### Port 80 atau 443 Tidak Bisa Dibuka

Penyebab umum:

* Firewall Belum Mengizinkan HTTP atau HTTPS.
* Apache Belum Berjalan.
* Port Dipakai Service Lain.
* Security Group VPS Belum Dibuka.

Cek Apache:

```bash
sudo systemctl status apache2
```

Cek port:

```bash
sudo ss -tulpn | grep -E ':80|:443'
```

Cek UFW:

```bash
sudo ufw status
```

Izinkan port:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### Domain Tidak Mengarah ke VPS

Penyebab umum:

* DNS A Record Belum Benar.
* Propagasi DNS Belum Selesai.
* Salah IP VPS.

Cek DNS:

```bash
dig bot.example.com
```

Atau:

```bash
nslookup bot.example.com
```

Pastikan hasilnya mengarah ke IP VPS.

### FreqUI Tidak Terbuka

Penyebab umum:

* Container Freqtrade Belum Berjalan.
* API Server Belum Aktif.
* API Server Tidak Listen di Port yang Benar.
* Apache Reverse Proxy Salah.
* Domain Belum Mengarah ke VPS.
* Firewall Memblokir HTTP atau HTTPS.

Cek container:

```bash
docker compose ps
```

Cek log:

```bash
docker compose logs -f
```

Cek port lokal:

```bash
curl http://127.0.0.1:8080
```

Jika curl dari server gagal, masalah ada di Freqtrade API.

Jika curl berhasil tetapi domain gagal, masalah ada di Apache, DNS, firewall, atau SSL.

### Config JSON Error

Error umum:

```text
JSONDecodeError
```

Penyebab umum:

* Koma Berlebih.
* Koma Kurang.
* Kutip Tidak Ditutup.
* Format JSON Tidak Valid.
* Komentar Dimasukkan ke File JSON.

JSON tidak mendukung komentar.

Validasi config dengan tool JSON validator atau gunakan:

```bash
python3 -m json.tool user_data/config.json
```

### Exchange Connection Error

Penyebab umum:

* API Key Salah.
* API Secret Salah.
* Permission API Tidak Cukup.
* IP Whitelist Tidak Sesuai.
* Exchange Sedang Bermasalah.
* Pair Tidak Didukung.
* Region atau Regulasi Membatasi Akses.

Solusi awal:

* Cek Ulang API Key.
* Cek Permission API.
* Cek IP Whitelist.
* Cek Pair Whitelist.
* Cek Dokumentasi Exchange.
* Cek Log Freqtrade.

### Bot Tidak Membuka Trade

Ini belum tentu error.

Penyebab umum:

* Strategy Belum Memberi Sinyal Entry.
* Pair Tidak Masuk Whitelist.
* Saldo Dry-run Tidak Cukup.
* Stake Amount Terlalu Kecil.
* Max Open Trades Sudah Terpenuhi.
* Bot Masih Dalam State Stopped.
* Market Tidak Memenuhi Kondisi Strategy.

Cek status bot melalui FreqUI.

Cek log:

```bash
docker compose logs -f
```

### SSL Gagal Dibuat

Penyebab umum:

* Domain Belum Mengarah ke IP VPS.
* Port 80 Tertutup.
* Apache Config Error.
* Rate Limit Let's Encrypt.
* DNS Baru Saja Diubah.

Cek config Apache:

```bash
sudo apachectl configtest
```

Cek akses HTTP:

```bash
curl -I http://bot.example.com
```

Cek firewall:

```bash
sudo ufw status
```

Ulangi Certbot setelah masalah DNS dan port selesai.

---

## Checklist Sebelum Lanjut ke Strategy

Sebelum masuk Artikel 3, pastikan semua item berikut sudah dipahami.

| Checklist                                    | Status           |
| -------------------------------------------- | ---------------- |
| Memahami Fungsi Exchange API                 | Wajib            |
| API Key Tidak Memiliki Permission Withdrawal | Wajib            |
| VPS Bisa Diakses Melalui SSH                 | Wajib            |
| Ubuntu Server Sudah Diupdate                 | Wajib            |
| Firewall Sudah Aktif                         | Wajib            |
| Docker Sudah Terpasang                       | Wajib            |
| Docker Compose Berjalan                      | Wajib            |
| Freqtrade Image Sudah Diunduh                | Wajib            |
| Folder `user_data` Sudah Dibuat              | Wajib            |
| `config.json` Sudah Dibuat                   | Wajib            |
| `dry_run` Bernilai `true`                    | Wajib            |
| `dry_run_wallet` Realistis                   | Wajib            |
| Pair Whitelist Dibuat Secara Sadar           | Wajib            |
| API Server Listen di `127.0.0.1`             | Wajib            |
| FreqUI Bisa Diakses                          | Wajib            |
| Apache Reverse Proxy Berjalan                | Direkomendasikan |
| SSL Aktif                                    | Direkomendasikan |
| Log Bot Bisa Dibaca                          | Wajib            |
| Cara Stop Bot Sudah Dipahami                 | Wajib            |

Jika checklist di atas belum terpenuhi, jangan lanjut ke live trading.

Artikel berikutnya akan membahas strategy. Namun, strategy hanya berguna jika environment sudah stabil dan aman.

---

## Key Takeaways

* Artikel ini membangun trading lab, bukan langsung menjalankan bot live.
* API key harus dibuat dengan permission minimal.
* Permission withdrawal tidak boleh diaktifkan untuk bot trading.
* VPS perlu disiapkan dengan update, firewall, user non-root, dan akses SSH yang aman.
* Docker adalah metode instalasi yang rapi untuk Freqtrade di VPS.
* `config.json` adalah pusat konfigurasi bot dan harus dijaga dengan serius.
* `dry_run` harus tetap `true` pada tahap belajar.
* FreqUI sebaiknya diakses melalui reverse proxy dan HTTPS.
* Port Freqtrade API tidak boleh dibuka langsung ke internet.
* Troubleshooting harus dimulai dari log, status container, config, DNS, firewall, dan koneksi exchange.

---

## Disclaimer

Artikel ini hanya untuk tujuan edukasi.

Setup teknis yang salah dapat menyebabkan risiko keamanan, risiko akses tidak sah, error trading, atau kehilangan dana jika digunakan dalam mode live.

Jangan menggunakan API key live dengan permission withdrawal.

Jangan menjalankan bot dalam mode live sebelum memahami config, strategy, risk management, dan cara menghentikan bot.

Crypto trading memiliki risiko tinggi. Bot trading tidak menjamin profit. Dry-run tidak sama dengan live trading.

Setiap keputusan teknis dan trading adalah tanggung jawab pribadi masing-masing pengguna.

---

2026 - Kenshin Himura
