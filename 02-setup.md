# Artikel 2 - Setup

# Setup Freqtrade di VPS: Exchange, Ubuntu Server, Docker, FreqUI, dan Reverse Proxy

> Artikel kedua dari seri tutorial Freqtrade. Artikel ini membahas persiapan akun exchange dan API key, penyewaan dan pengamanan VPS Ubuntu Server, instalasi Freqtrade menggunakan Docker, konfigurasi dasar, menjalankan dry-run, akses FreqUI melalui Apache reverse proxy dengan SSL, serta troubleshooting instalasi.

---

## Daftar Isi

1. [Pendahuluan](#pendahuluan)
2. [Arsitektur Sistem](#arsitektur-sistem)
3. [Prinsip Keamanan Setup](#prinsip-keamanan-setup)
4. [Persiapan Exchange dan API Key](#persiapan-exchange-dan-api-key)
   * [Kriteria Exchange](#kriteria-exchange)
   * [Persiapan Akun](#persiapan-akun)
   * [Membuat API Key dengan Permission Minimal](#membuat-api-key-dengan-permission-minimal)
5. [Persiapan VPS](#persiapan-vps)
   * [Spesifikasi dan Pemilihan Provider](#spesifikasi-dan-pemilihan-provider)
   * [Akses Awal Melalui SSH](#akses-awal-melalui-ssh)
   * [Update Sistem dan Package Dasar](#update-sistem-dan-package-dasar)
   * [Konfigurasi Timezone](#konfigurasi-timezone)
6. [Pengamanan Server](#pengamanan-server)
   * [Membuat User Non-root](#membuat-user-non-root)
   * [Konfigurasi Firewall](#konfigurasi-firewall)
   * [Autentikasi SSH Key](#autentikasi-ssh-key)
7. [Instalasi Freqtrade](#instalasi-freqtrade)
   * [Metode Instalasi](#metode-instalasi)
   * [Instalasi Docker](#instalasi-docker)
   * [Setup Project Freqtrade](#setup-project-freqtrade)
   * [Struktur Folder](#struktur-folder)
8. [Konfigurasi Bot](#konfigurasi-bot)
   * [File config.json](#file-configjson)
   * [Parameter Penting](#parameter-penting)
9. [Menjalankan Bot dalam Mode Dry-run](#menjalankan-bot-dalam-mode-dry-run)
10. [FreqUI](#frequi)
11. [Apache Reverse Proxy](#apache-reverse-proxy)
12. [SSL dengan Let's Encrypt](#ssl-dengan-lets-encrypt)
13. [Referensi Command Docker Compose](#referensi-command-docker-compose)
14. [Troubleshooting](#troubleshooting)
15. [Checklist Sebelum Lanjut ke Artikel 3](#checklist-sebelum-lanjut-ke-artikel-3)
16. [Ringkasan](#ringkasan)
17. [Disclaimer](#disclaimer)

---

## Pendahuluan

Artikel ini adalah lanjutan dari Artikel 1 - Foundation. Artikel pertama membahas konsep dasar crypto market; artikel ini membahas sisi teknis. Target akhir artikel ini adalah environment Freqtrade yang berjalan di VPS dalam mode dry-run, dapat dipantau melalui FreqUI, dan dapat diakses secara aman melalui HTTPS.

Fokus pada tahap ini adalah membangun environment yang rapi, aman, dan dapat diuji - bukan profit. Live trading baru dipertimbangkan setelah seluruh tahapan validasi pada artikel-artikel berikutnya selesai.

Setelah menyelesaikan artikel ini, pembaca diharapkan dapat:

* Menyiapkan akun exchange untuk penggunaan bot
* Membuat API key dengan permission minimal
* Menyewa dan mengakses VPS melalui SSH
* Menyiapkan dan mengamankan Ubuntu Server
* Menginstal Freqtrade menggunakan Docker
* Membuat dan memahami konfigurasi dasar
* Menjalankan bot dalam mode dry-run
* Mengakses FreqUI melalui reverse proxy dan SSL
* Mendiagnosis error instalasi yang umum terjadi

### Prasyarat

* Memahami materi Artikel 1
* Terbiasa dengan command line dasar Linux
* Memiliki domain (untuk bagian reverse proxy dan SSL; opsional jika hanya menjalankan dry-run lokal)

---

## Arsitektur Sistem

Alur sistem yang dibangun pada artikel ini:

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

| Komponen             | Fungsi                                                |
| -------------------- | ----------------------------------------------------- |
| User Browser         | Mengakses FreqUI                                      |
| Domain               | Nama domain untuk membuka dashboard                   |
| HTTPS                | Enkripsi koneksi melalui SSL                          |
| Apache Reverse Proxy | Meneruskan request dari domain ke Freqtrade API lokal |
| FreqUI               | Web interface untuk memantau bot                      |
| Freqtrade Bot        | Program utama yang menjalankan strategi               |
| Exchange API         | Jalur komunikasi antara bot dan exchange              |
| Crypto Market        | Market tempat order dieksekusi                        |

Prinsip arsitektur yang digunakan: Freqtrade API hanya mendengarkan koneksi lokal di `127.0.0.1:8080` dan tidak diekspos langsung ke internet. Seluruh akses dari luar melewati Apache dengan HTTPS dan autentikasi FreqUI.

---

## Prinsip Keamanan Setup

Ketentuan berikut berlaku untuk seluruh tahapan pada artikel ini:

* API key tidak menggunakan permission withdrawal
* Secret (API key, password, JWT secret) tidak disimpan di repository publik
* Bot tidak dijalankan dalam mode live sebelum melewati dry-run
* Testing tidak menggunakan modal besar
* Port Freqtrade API tidak dibuka langsung ke internet
* OS dan dependency diupdate secara berkala
* FreqUI menggunakan password yang kuat
* VPS dianggap tidak aman secara default dan diamankan secara eksplisit

Kesalahan konfigurasi pada trading bot berdampak langsung: bot dapat membuka order yang tidak diinginkan, atau API key dapat disalahgunakan jika bocor. Setiap ketentuan di atas dijelaskan implementasinya pada bagian terkait.

---

## Persiapan Exchange dan API Key

### Kriteria Exchange

Freqtrade membutuhkan exchange yang mendukung API trading. Kriteria pemilihan exchange:

* Reputasi dan riwayat keamanan yang baik
* Dukungan API trading
* Ketersediaan pair yang akan digunakan
* Volume dan likuiditas yang memadai
* Struktur fee trading yang jelas
* Fitur keamanan seperti 2FA
* Dukungan IP whitelist untuk API (jika tersedia)
* Kompatibilitas dengan Freqtrade melalui CCXT

### Persiapan Akun

Langkah persiapan pada akun exchange:

1. Aktifkan 2FA
2. Selesaikan verifikasi akun sesuai ketentuan exchange
3. Pahami aturan deposit dan withdrawal
4. Pahami struktur fee trading
5. Tentukan pair likuid yang akan digunakan
6. Alokasikan hanya sebagian modal; jangan gunakan seluruh dana

Pada tahap belajar, seluruh pengujian dilakukan dalam mode dry-run. Dry-run tidak mengirim order sungguhan ke exchange.

### Membuat API Key dengan Permission Minimal

API key adalah kredensial yang menghubungkan bot ke exchange. Komponen API key pada umumnya:

| Komponen   | Fungsi                                  |
| ---------- | --------------------------------------- |
| API Key    | Identitas akses API                     |
| API Secret | Secret untuk autentikasi                |
| Passphrase | Digunakan oleh sebagian exchange        |
| Permission | Hak akses yang diberikan ke API key     |

Permission yang dibutuhkan bot trading:

* Read
* Trade

Permission yang tidak boleh diaktifkan:

* Withdraw
* Internal transfer (jika tidak dibutuhkan)
* Account management (jika tidak dibutuhkan)

Ketentuan pengelolaan API key:

* Aktifkan IP whitelist jika exchange mendukung, diisi dengan IP VPS
* Gunakan API key khusus untuk bot, terpisah dari API key lain
* Jangan menyimpan API key di repository GitHub
* Jangan mengirim API key melalui chat atau menyertakannya dalam screenshot
* Lakukan rotasi API key jika diduga bocor
* Hapus API key yang tidak digunakan

Pembatasan permission membatasi dampak terburuk jika API key bocor: tanpa permission withdrawal, pihak yang memperoleh API key tidak dapat menarik dana keluar dari akun.

---

## Persiapan VPS

### Spesifikasi dan Pemilihan Provider

VPS digunakan agar bot berjalan terus-menerus tanpa bergantung pada komputer pribadi. Spesifikasi minimum untuk Freqtrade tahap awal:

| Komponen | Rekomendasi                                                        |
| -------- | ------------------------------------------------------------------ |
| OS       | Ubuntu Server LTS                                                  |
| CPU      | 1–2 vCPU                                                           |
| RAM      | Minimal 2 GB                                                       |
| Storage  | Minimal 20 GB SSD                                                  |
| Network  | Koneksi stabil                                                     |
| Lokasi   | Dekat dengan server exchange lebih baik, tetapi tidak wajib        |
| Akses    | SSH dengan root atau user sudo                                     |

VPS spesifikasi kecil sudah memadai untuk tahap belajar. Resource dapat dinaikkan kemudian sesuai kebutuhan aktual.

Kriteria pemilihan provider:

* Reputasi dan uptime
* Kemudahan rebuild OS
* Fitur backup atau snapshot
* Firewall panel
* Lokasi data center
* Harga bulanan
* Dukungan IPv4
* Kemudahan reset password atau SSH key

### Akses Awal Melalui SSH

Setelah VPS dibuat, provider memberikan IP publik server, username awal, password atau SSH key, dan informasi OS.

Login menggunakan password:

```bash
ssh root@YOUR_SERVER_IP
```

Login menggunakan SSH key:

```bash
ssh -i ~/.ssh/your_key root@YOUR_SERVER_IP
```

Verifikasi kondisi server setelah login:

```bash
# Versi OS
lsb_release -a

# User aktif
whoami

# Resource server
free -h
df -h
nproc
```

### Update Sistem dan Package Dasar

Lakukan update sistem sebelum instalasi apa pun:

```bash
sudo apt update
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

### Konfigurasi Timezone

Cek timezone dan status sinkronisasi waktu:

```bash
timedatectl
```

Set timezone jika diperlukan:

```bash
sudo timedatectl set-timezone Asia/Jakarta
```

Waktu server harus akurat. Request ke API exchange menggunakan timestamp; selisih waktu yang terlalu besar antara server dan exchange menyebabkan request ditolak.

---

## Pengamanan Server

### Membuat User Non-root

Operasional harian sebaiknya tidak menggunakan root. Buat user baru:

```bash
adduser freqbot
```

Tambahkan ke group sudo:

```bash
usermod -aG sudo freqbot
```

Login sebagai user baru dan verifikasi akses sudo:

```bash
su - freqbot
sudo whoami
```

Output `root` menandakan user sudah memiliki akses sudo.

### Konfigurasi Firewall

Izinkan SSH, HTTP, dan HTTPS, lalu aktifkan firewall:

```bash
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

Verifikasi status:

```bash
sudo ufw status
```

Port Freqtrade API (`8080`) tidak dibuka di firewall. API server dikonfigurasi agar hanya listen di `127.0.0.1`, sehingga hanya dapat diakses dari dalam server.

### Autentikasi SSH Key

Gunakan SSH key dan nonaktifkan password login setelah SSH key terverifikasi berfungsi. Edit konfigurasi SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

Parameter yang diatur:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart service SSH:

```bash
sudo systemctl restart ssh
```

**Peringatan:** jangan menutup sesi SSH aktif sebelum login dengan SSH key berhasil diverifikasi dari terminal baru. Kesalahan pada tahap ini dapat mengunci akses ke server.

---

## Instalasi Freqtrade

### Metode Instalasi

Freqtrade dapat diinstal melalui beberapa metode:

| Metode          | Kelebihan                          | Kekurangan                              |
| --------------- | ---------------------------------- | --------------------------------------- |
| Docker          | Konsisten, mudah update, terisolasi | Perlu memahami Docker Compose           |
| Native (script) | Akses langsung ke Python environment | Dependency dapat konflik dengan OS      |
| Manual          | Fleksibel                          | Memerlukan pengalaman lebih             |

Seri ini menggunakan Docker karena environment lebih mudah direproduksi, dependency terisolasi dari OS, proses update terstruktur, dan sesuai untuk deployment jangka panjang di VPS.

### Instalasi Docker

Install package pendukung:

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
```

Tambahkan GPG key Docker:

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

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

Install Docker dan Docker Compose plugin:

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Verifikasi instalasi:

```bash
docker --version
docker compose version
```

Tambahkan user ke group docker agar dapat menjalankan Docker tanpa sudo:

```bash
sudo usermod -aG docker $USER
```

Logout lalu login kembali agar perubahan group aktif, kemudian tes:

```bash
docker run hello-world
```

### Setup Project Freqtrade

Buat folder kerja:

```bash
mkdir -p ~/freqtrade
cd ~/freqtrade
```

Download file Docker Compose resmi Freqtrade:

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

Generate config awal:

```bash
docker compose run --rm freqtrade new-config --config user_data/config.json
```

Command `new-config` menanyakan parameter awal secara interaktif: exchange, stake currency, stake amount, max open trades, timeframe, dan mode dry-run. Pada tahap ini, pilih dry-run.

### Struktur Folder

Struktur folder setelah setup awal:

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

| Path                         | Fungsi                          |
| ---------------------------- | ------------------------------- |
| `docker-compose.yml`         | Konfigurasi container Freqtrade |
| `user_data/config.json`      | Konfigurasi utama bot           |
| `user_data/strategies/`      | File strategy Python            |
| `user_data/data/`            | Data historis market            |
| `user_data/backtest_results/`| Hasil backtesting               |
| `user_data/logs/`            | Log bot                         |
| `user_data/hyperopts/`       | File hyperopt                   |
| `user_data/plot/`            | Output chart atau plot          |

Folder `user_data` berisi config, strategy, data historis, dan hasil testing. Folder ini perlu dimasukkan dalam rencana backup dan tidak boleh dihapus tanpa pertimbangan.

---

## Konfigurasi Bot

### File config.json

Konfigurasi utama berada di `user_data/config.json`:

```bash
nano user_data/config.json
```

Contoh konfigurasi dasar untuk dry-run:

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
  "bot_name": "freqtrade-lab",
  "initial_state": "stopped",
  "force_entry_enable": false,
  "internals": {
    "process_throttle_secs": 5
  }
}
```

Ketentuan penggunaan config:

* Seluruh nilai `CHANGE_THIS` wajib diganti dengan nilai acak yang kuat
* `dry_run` tetap `true` selama tahap belajar
* API server tetap listen di `127.0.0.1`
* `config.json` tidak di-commit ke repository; untuk repository publik, gunakan `config.example.json` tanpa secret
* Config ini tidak digunakan untuk live trading tanpa review menyeluruh

### Parameter Penting

#### max_open_trades

```json
"max_open_trades": 3
```

Jumlah maksimal posisi yang dapat dibuka bersamaan. Semakin banyak posisi aktif, semakin besar eksposur terhadap market. Untuk tahap awal, gunakan nilai kecil.

#### stake_currency

```json
"stake_currency": "USDT"
```

Mata uang yang digunakan sebagai modal trading. Untuk pair berformat `BTC/USDT`, stake currency adalah `USDT`.

#### stake_amount

```json
"stake_amount": 20
```

Besaran modal per trade. Nilai `20` berarti bot menggunakan sekitar 20 USDT per trade. Pada dry-run nilai ini hanya simulasi; pada live trading nilai ini menentukan ukuran order sungguhan.

#### dry_run

```json
"dry_run": true
```

Menentukan mode simulasi. Nilai `true` berarti bot tidak mengirim order sungguhan ke exchange. Nilai ini tidak diubah ke `false` sebelum seluruh tahapan validasi strategi selesai.

#### dry_run_wallet

```json
"dry_run_wallet": 1000
```

Saldo simulasi pada mode dry-run. Gunakan nilai yang mendekati modal live yang direncanakan. Saldo simulasi yang jauh lebih besar dari modal sebenarnya menghasilkan ekspektasi performa yang tidak realistis.

#### trading_mode

```json
"trading_mode": "spot"
```

Mode trading. Seri ini menggunakan `spot`. Mode futures memerlukan pemahaman tambahan dan tidak dibahas pada seri ini.

#### timeframe

```json
"timeframe": "5m"
```

Timeframe candle yang digunakan strategy. Timeframe kecil menghasilkan sinyal lebih sering dengan noise lebih tinggi; timeframe besar lebih lambat dan umumnya lebih stabil dianalisis. Nilai ini dapat di-override oleh strategy.

#### exchange.name

```json
"name": "binance"
```

Exchange yang digunakan. Nama harus sesuai dengan identifier exchange yang didukung Freqtrade dan CCXT. Daftar exchange yang didukung tersedia di dokumentasi Freqtrade.

#### exchange.key dan exchange.secret

```json
"key": "",
"secret": ""
```

API key dan API secret. Pada mode dry-run, sebagian exchange tetap memerlukan API key untuk membaca data tertentu, tetapi key dapat dikosongkan untuk pengujian dasar. Ketentuan: config asli berisi secret disimpan privat, repository publik hanya berisi file example tanpa secret.

#### pair_whitelist

```json
"pair_whitelist": [
  "BTC/USDT",
  "ETH/USDT"
]
```

Daftar pair yang boleh diperdagangkan bot. Untuk tahap awal, gunakan sedikit pair dengan kapitalisasi besar dan likuiditas tinggi.

#### pair_blacklist

```json
"pair_blacklist": []
```

Daftar pair yang dilarang diperdagangkan. Berguna terutama saat menggunakan pairlist dinamis, untuk mengecualikan pair tertentu seperti stablecoin pair atau leveraged token.

#### pairlists

```json
"pairlists": [
  {
    "method": "StaticPairList"
  }
]
```

Metode pemilihan pair. `StaticPairList` menggunakan `pair_whitelist` secara langsung, sehingga pair yang diperdagangkan jelas dan terkontrol. Pairlist dinamis (misalnya `VolumePairList`) dibahas pada artikel selanjutnya.

#### api_server

```json
"enabled": true,
"listen_ip_address": "127.0.0.1",
"listen_port": 8080
```

API server diperlukan untuk FreqUI. `listen_ip_address` diisi `127.0.0.1` agar API hanya dapat diakses dari dalam server; nilai `0.0.0.0` mengekspos API ke seluruh interface jaringan dan tidak digunakan pada setup ini.

```json
"username": "CHANGE_THIS_USERNAME",
"password": "CHANGE_THIS_PASSWORD"
```

Kredensial login FreqUI. Gunakan kombinasi yang kuat; hindari nilai umum seperti `admin`, `password`, `123456`, atau `freqtrade`. `jwt_secret_key` dan `ws_token` juga diisi nilai acak yang panjang.

#### initial_state

```json
"initial_state": "stopped"
```

Status bot saat container dinyalakan. Nilai `stopped` mencegah bot langsung trading saat startup; bot dijalankan secara manual melalui FreqUI atau API setelah kondisi diverifikasi.

---

## Menjalankan Bot dalam Mode Dry-run

Jalankan bot:

```bash
cd ~/freqtrade
docker compose up -d
```

Verifikasi container berjalan:

```bash
docker compose ps
```

Pantau log:

```bash
docker compose logs -f
```

Log normal menampilkan proses startup, koneksi ke exchange, pemrosesan pairlist, dan status bot. Karena `initial_state` bernilai `stopped`, bot belum mulai trading sampai dijalankan melalui FreqUI atau API.

Command operasional dasar:

```bash
# Menghentikan container
docker compose down

# Restart container
docker compose restart

# Update image
docker compose pull
docker compose up -d
```

---

## FreqUI

FreqUI adalah web interface resmi untuk mengontrol dan memantau Freqtrade. Fitur yang tersedia:

* Status bot dan kontrol start/stop
* Daftar trade aktif dan trade history
* Profit dan loss
* Pair yang dipantau
* Log dasar
* Chart dengan plot indikator
* Backtesting melalui mode webserver
* Sebagian pengaturan konfigurasi

Dengan API server listen di `127.0.0.1:8080`, FreqUI hanya dapat diakses dari dalam server:

```text
http://127.0.0.1:8080
```

Akses dari browser pengguna dilakukan melalui reverse proxy dengan HTTPS, yang dibahas pada bagian berikutnya. Port `8080` tidak dibuka langsung ke internet.

---

## Apache Reverse Proxy

Reverse proxy meneruskan request HTTPS dari domain (misalnya `https://bot.example.com`) ke Freqtrade API lokal di `http://127.0.0.1:8080`.

### Instalasi Apache

```bash
sudo apt install -y apache2
```

Aktifkan module yang diperlukan:

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

### Konfigurasi Virtual Host

Buat file virtual host:

```bash
sudo nano /etc/apache2/sites-available/freqtrade.conf
```

Isi konfigurasi (ganti `bot.example.com` dengan domain yang digunakan):

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

Baris `ProxyPass` untuk path `/api/v1/message/ws` diperlukan agar koneksi WebSocket FreqUI berfungsi.

Aktifkan site dan verifikasi konfigurasi:

```bash
sudo a2ensite freqtrade.conf
sudo apachectl configtest
sudo systemctl reload apache2
```

Pastikan DNS A record domain mengarah ke IP VPS, lalu verifikasi dari browser:

```text
http://bot.example.com
```

Jika container Freqtrade berjalan dan API server aktif, halaman FreqUI akan terbuka.

---

## SSL dengan Let's Encrypt

Setelah reverse proxy HTTP berfungsi, aktifkan HTTPS menggunakan Certbot.

### Instalasi Certbot

```bash
sudo apt install -y snapd
sudo snap install core
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

### Generate Sertifikat

```bash
sudo certbot --apache -d bot.example.com
```

Certbot membuat konfigurasi SSL untuk virtual host dan menawarkan redirect otomatis dari HTTP ke HTTPS. Pilih redirect agar seluruh akses terenkripsi.

Verifikasi proses renewal otomatis:

```bash
sudo certbot renew --dry-run
```

Setelah SSL aktif, FreqUI diakses melalui:

```text
https://bot.example.com
```

---

## Referensi Command Docker Compose

Command yang paling sering digunakan dalam operasional harian:

| Tujuan                      | Command                                          |
| --------------------------- | ------------------------------------------------ |
| Start bot                   | `docker compose up -d`                           |
| Stop bot                    | `docker compose down`                            |
| Restart bot                 | `docker compose restart`                         |
| Melihat status container    | `docker compose ps`                              |
| Melihat log                 | `docker compose logs -f`                         |
| Update image                | `docker compose pull && docker compose up -d`    |
| Masuk ke container          | `docker compose exec freqtrade bash`             |

Menjalankan subcommand Freqtrade secara manual:

```bash
# Melihat help
docker compose run --rm freqtrade --help

# Membuat config baru
docker compose run --rm freqtrade new-config --config user_data/config.json

# Membuat strategy baru dari template
docker compose run --rm freqtrade new-strategy --strategy MyFirstStrategy
```

---

## Troubleshooting

### Docker: permission denied

```text
permission denied while trying to connect to the Docker daemon socket
```

Penyebab umum: user belum masuk group docker, atau sesi SSH belum di-logout setelah penambahan group.

Solusi:

```bash
sudo usermod -aG docker $USER
```

Logout dari SSH, login kembali, lalu verifikasi:

```bash
docker ps
```

### Port 80 atau 443 tidak dapat diakses

Penyebab umum: firewall belum mengizinkan HTTP/HTTPS, Apache belum berjalan, port digunakan service lain, atau security group di panel VPS belum dibuka.

Diagnosis:

```bash
# Status Apache
sudo systemctl status apache2

# Service yang menggunakan port
sudo ss -tulpn | grep -E ':80|:443'

# Status firewall
sudo ufw status
```

Izinkan port jika belum:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### Domain tidak mengarah ke VPS

Penyebab umum: A record belum benar, propagasi DNS belum selesai, atau IP yang didaftarkan salah.

Diagnosis:

```bash
dig bot.example.com
# atau
nslookup bot.example.com
```

Pastikan hasil resolusi mengarah ke IP VPS.

### FreqUI tidak terbuka

Penyebab umum: container belum berjalan, API server tidak aktif atau listen di port yang salah, konfigurasi Apache salah, DNS belum mengarah, atau firewall memblokir.

Diagnosis bertahap:

```bash
# 1. Status container
docker compose ps

# 2. Log bot
docker compose logs -f

# 3. Tes API dari dalam server
curl http://127.0.0.1:8080
```

Interpretasi: jika `curl` dari dalam server gagal, masalah berada di Freqtrade (container atau konfigurasi API server). Jika `curl` berhasil tetapi akses melalui domain gagal, masalah berada di Apache, DNS, firewall, atau SSL.

### Config JSON error

```text
JSONDecodeError
```

Penyebab umum: koma berlebih atau kurang, tanda kutip tidak ditutup, atau komentar di dalam file JSON. Format JSON tidak mendukung komentar.

Validasi config:

```bash
python3 -m json.tool user_data/config.json
```

### Exchange connection error

Penyebab umum: API key atau secret salah, permission tidak mencukupi, IP whitelist tidak sesuai dengan IP VPS, gangguan di sisi exchange, pair tidak didukung, atau pembatasan regional.

Langkah diagnosis: periksa ulang API key dan permission, periksa IP whitelist, periksa pair whitelist, baca log Freqtrade, dan rujuk dokumentasi exchange.

### Bot berjalan tetapi tidak membuka trade

Kondisi ini belum tentu error. Penyebab umum:

* Strategy belum menghasilkan sinyal entry
* Pair tidak masuk whitelist
* Saldo dry-run tidak mencukupi
* `stake_amount` di bawah minimum order exchange
* `max_open_trades` sudah terpenuhi
* Bot masih dalam state `stopped`
* Kondisi market tidak memenuhi aturan strategy

Periksa status bot melalui FreqUI dan baca log:

```bash
docker compose logs -f
```

### SSL gagal dibuat

Penyebab umum: domain belum mengarah ke IP VPS, port 80 tertutup, konfigurasi Apache error, rate limit Let's Encrypt, atau perubahan DNS belum terpropagasi.

Diagnosis:

```bash
# Konfigurasi Apache
sudo apachectl configtest

# Akses HTTP dari luar
curl -I http://bot.example.com

# Firewall
sudo ufw status
```

Jalankan ulang Certbot setelah masalah DNS dan port terselesaikan.

---

## Checklist Sebelum Lanjut ke Artikel 3

| Checklist                                      | Status           |
| ---------------------------------------------- | ---------------- |
| Memahami fungsi exchange API                   | Wajib            |
| API key tidak memiliki permission withdrawal   | Wajib            |
| VPS dapat diakses melalui SSH                  | Wajib            |
| Ubuntu Server sudah diupdate                   | Wajib            |
| Firewall aktif                                 | Wajib            |
| Docker dan Docker Compose terpasang            | Wajib            |
| Image Freqtrade sudah diunduh                  | Wajib            |
| Folder `user_data` sudah dibuat                | Wajib            |
| `config.json` sudah dibuat                     | Wajib            |
| `dry_run` bernilai `true`                      | Wajib            |
| `dry_run_wallet` bernilai realistis            | Wajib            |
| Pair whitelist ditentukan secara sadar         | Wajib            |
| API server listen di `127.0.0.1`               | Wajib            |
| FreqUI dapat diakses                           | Wajib            |
| Log bot dapat dibaca                           | Wajib            |
| Prosedur menghentikan bot dipahami             | Wajib            |
| Apache reverse proxy berjalan                  | Direkomendasikan |
| SSL aktif                                      | Direkomendasikan |

Artikel berikutnya membahas pengembangan strategy. Strategy hanya dapat diuji dengan benar jika environment pada artikel ini sudah stabil dan aman.

---

## Ringkasan

* Target artikel ini adalah environment Freqtrade yang berjalan dalam mode dry-run di VPS, bukan live trading.
* API key dibuat dengan permission minimal (read dan trade); permission withdrawal tidak diaktifkan.
* VPS diamankan melalui update sistem, firewall, user non-root, dan autentikasi SSH key.
* Docker adalah metode instalasi yang digunakan karena konsisten dan mudah dikelola di VPS.
* `config.json` adalah pusat konfigurasi bot; seluruh secret di dalamnya dijaga dan tidak masuk repository publik.
* `dry_run` bernilai `true` selama tahap belajar, dan `initial_state` bernilai `stopped` agar bot tidak langsung trading saat startup.
* Freqtrade API hanya listen di `127.0.0.1`; akses eksternal dilakukan melalui Apache reverse proxy dengan SSL.
* Troubleshooting dilakukan bertahap: log bot, status container, validasi config, DNS, firewall, lalu koneksi exchange.

---

## Disclaimer

Artikel ini ditulis untuk tujuan edukasi.

Setup teknis yang salah dapat menyebabkan risiko keamanan, akses tidak sah, error trading, atau kehilangan dana jika digunakan dalam mode live. Jangan menggunakan API key dengan permission withdrawal untuk bot. Jangan menjalankan bot dalam mode live sebelum memahami konfigurasi, strategy, risk management, dan prosedur menghentikan bot.

Crypto trading memiliki risiko tinggi. Bot trading tidak menjamin profit. Dry-run tidak sama dengan live trading. Setiap keputusan teknis dan trading adalah tanggung jawab masing-masing pengguna.

---

2026 - Kenshin Himura
