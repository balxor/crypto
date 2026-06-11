# Crypto Trading for Gamers with Freqtrade

> Panduan belajar crypto trading dan bot trading menggunakan Freqtrade, ditulis untuk gamers yang ingin memahami market secara sistematis, teknis, dan bertanggung jawab.

## Tentang Repository Ini

Repository ini berisi seri artikel edukasi tentang dasar-dasar crypto trading, setup Freqtrade, pembuatan strategi, backtesting, dry-run, dan operasional bot trading.

Target utama repository ini adalah gamers yang terbiasa membaca sistem, statistik, market item, build, probability, dan resource management, tetapi belum familiar dengan crypto market dan risiko trading sungguhan.

Repository ini tidak dibuat untuk menjual mimpi profit cepat, tetapi repository ini dibuat untuk membangun cara berpikir yang lebih aman:

* Memahami market sebelum trading
* Memahami risiko sebelum menggunakan bot
* Memahami strategi sebelum live
* Memahami backtesting sebelum percaya pada hasil
* Memahami operations sebelum menjalankan bot dengan uang sungguhan

---

## Untuk Siapa Repository Ini?

Repository ini cocok untuk:

* Gamers yang baru mulai belajar crypto
* Pemula yang ingin memahami trading bot secara teknis
* Pengguna Linux pemula yang ingin belajar setup bot di VPS
* Orang yang ingin belajar Freqtrade dari dasar
* Pembaca yang ingin memahami backtesting, dry-run, dan risk management
* Trader pemula yang ingin berpikir lebih sistematis

Repository ini tidak cocok untuk:

* Orang yang mencari sinyal beli dan jual
* Orang yang ingin strategi profit instan
* Orang yang ingin copy-paste bot tanpa memahami risiko
* Orang yang ingin menggunakan leverage secara agresif
* Orang yang ingin menjalankan bot live tanpa testing

---

## Prinsip Utama

Seri ini menggunakan beberapa prinsip dasar:

1. **Edukasi**

   Jangan menjalankan bot live sebelum memahami market, exchange, API key, config, strategi, backtest, dan risiko.

2. **Dry-run**

   Semua strategi harus diuji dalam mode dry-run sebelum menggunakan uang sungguhan.

3. **Backtest**

   Backtest hanya menunjukkan performa strategi pada data historis. Market real bisa berbeda.

4. **Security**

   API key, VPS, SSH, firewall, reverse proxy, SSL, dan backup adalah bagian penting dari operasional bot.

5. **Risk Management**

   Strategi bagus tetap bisa gagal jika position sizing, stoploss, dan exposure tidak dikontrol.

6. **Bot hanya menjalankan aturan**

   Jika strategi buruk, bot akan menjalankan strategi buruk itu secara konsisten.

---

## Struktur Artikel

Seri ini dibagi menjadi empat artikel utama.

| Artikel   | Fokus      | Status  |
| --------- | ---------- | ------- |
| Artikel 1 | Foundation | Planned |
| Artikel 2 | Setup      | Planned |
| Artikel 3 | Strategy   | Planned |
| Artikel 4 | Operations | Planned |

---

## Artikel 1: Foundation

Artikel pertama membahas dasar-dasar crypto trading untuk pembaca yang benar-benar baru.

Topik utama:

* Apa itu cryptocurrency
* Blockchain, wallet, stablecoin, dan exchange
* Bagaimana pasar crypto bekerja
* Market 24/7
* Pair trading
* Order book
* Fee
* Apa itu trading
* Jenis-jenis trading: spot, futures, dan margin
* Apa itu trading bot
* Kenapa trading bot digunakan
* Pengenalan Freqtrade
* Apa yang bisa dilakukan Freqtrade
* Apa yang tidak bisa dilakukan Freqtrade
* Pengenalan candlestick
* Cara membaca chart dasar
* Glossary istilah crypto dan trading

Tujuan artikel ini adalah membangun dasar pemukiran sebelum pembaca masuk ke instalasi bot.

---

## Artikel 2: Setup

Artikel kedua membahas setup teknis dari nol sampai bot bisa berjalan dalam mode dry-run.

Topik utama:

* Persiapan akun exchange
* Membuat API key dengan izin minimal
* Prinsip keamanan API key
* Menyewa VPS
* Mengakses VPS menggunakan SSH
* Setup Ubuntu Server dari nol
* Update package dan konfigurasi dasar server
* Instalasi Freqtrade
* Struktur folder Freqtrade
* Konfigurasi dasar `config.json`
* Penjelasan parameter penting dalam config
* Instalasi FreqUI
* Apache reverse proxy
* SSL menggunakan Let's Encrypt
* Menjalankan bot pertama kali dalam mode dry-run
* Troubleshooting instalasi umum

Tujuan artikel ini adalah membuat pembaca memiliki environment Freqtrade yang bersih, aman, dan siap digunakan untuk eksperimen.

---

## Artikel 3: Strategy

Artikel ketiga membahas cara berpikir dan menulis strategi Freqtrade.

Topik utama:

* Memahami data candlestick
* Memahami dataframe
* OHLCV: open, high, low, close, volume
* Apa itu indikator teknikal
* Cara kerja indikator teknikal
* Contoh indikator: SMA, EMA, RSI, dan volume
* Struktur dasar strategy Freqtrade
* Menulis strategi dari nol
* Contoh strategi SMA crossover
* Contoh strategi RSI
* Contoh strategi kombinasi trend, RSI, dan volume
* ROI
* Stoploss
* Trailing stoploss
* Backtesting
* Cara menjalankan backtest
* Cara membaca hasil backtest
* Iterasi strategi
* Hyperparameter optimization
* Menghindari overfitting
* Menggunakan strategi komunitas sebagai referensi

Tujuan artikel ini adalah memahami bagaimana strategi dibuat, diuji, dievaluasi, dan diperbaiki.

---

## Artikel 4: Operations

Artikel keempat membahas operasional bot setelah setup dan strategi selesai diuji.

Topik utama:

* Transisi dari dry-run ke live
* Pre-live checklist
* Manajemen modal
* Position sizing
* Monitoring bot
* Membaca log
* Monitoring melalui FreqUI
* Alerting
* Kapan harus menghentikan bot
* Kill switch plan
* Maintenance server
* Update OS
* Update Freqtrade
* Backup konfigurasi
* Backup strategi
* Incident response ringan
* Troubleshooting umum saat live

Tujuan artikel ini adalah membantu pembaca memahami bahwa menjalankan bot. Live trading membutuhkan monitoring, maintenance, disiplin, dan rencana berhenti.

---

## Peringatan Security

Jangan pernah menyimpan informasi berikut ke dalam repository publik:

* API key
* API secret
* Password VPS
* Private key SSH
* Token Telegram
* Database production
* File konfigurasi live yang berisi secret
* Informasi akun exchange

Gunakan file contoh seperti:

```text
config.example.json
config.dryrun.example.json
```

Jangan upload file seperti:

```text
config.json
config-live.json
secrets.json
.env
id_rsa
```

Jika menggunakan GitHub, pastikan file sensitif masuk ke `.gitignore`.

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

---

## Workflow Belajar

Urutan belajar yang disarankan:

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

Jangan loncat dari instalasi langsung ke live trading.

---

## Catatan untuk Gamers

Jika kamu datang dari dunia game, beberapa konsep mungkin terasa familiar:

| Dunia Game      | Dunia Crypto Trading                                 |
| --------------- | ---------------------------------------------------- |
| Market board    | Exchange order book                                  |
| Item price      | Asset price                                          |
| Farming         | Akumulasi resource, tetapi tidak sama dengan trading |
| Build karakter  | Strategy design                                      |
| Stat balancing  | Risk management                                      |
| Patch update    | Perubahan market condition                           |
| Boss raid       | Live trading condition                               |
| Training ground | Dry-run                                              |
| Combat log      | Bot log                                              |
| Theorycrafting  | Backtesting dan strategy iteration                   |

Namun, ada satu perbedaan besar:

Di game, kesalahan biasanya menghabiskan waktu atau resource digital. Di crypto trading, kesalahan bisa menghilangkan uang sungguhan.

---

## Apa yang Akan Dipelajari

Setelah mengikuti seri ini, pembaca diharapkan mampu:

* Memahami konsep dasar crypto market
* Memahami perbedaan spot, futures, dan margin
* Memahami risiko trading bot
* Memahami fungsi Freqtrade
* Melakukan setup Freqtrade di VPS
* Menjalankan bot dalam mode dry-run
* Membaca struktur dasar strategy Freqtrade
* Menjalankan backtesting
* Membaca hasil backtest secara kritis
* Memahami risiko overfitting
* Melakukan monitoring bot
* Membuat checklist sebelum live
* Menjaga konfigurasi dan API key tetap aman

---

## Apa yang Tidak Dijanjikan

Repository ini tidak menjanjikan:

* Profit konsisten
* Strategi anti-rugi
* Sinyal beli dan jual
* Rekomendasi coin
* Financial advice
* Strategi futures agresif
* Leverage setup
* Auto-profit bot
* Passive income tanpa risiko

Jika ada strategi contoh di repository ini, strategi tersebut hanya untuk edukasi.

---

## Official References

Beberapa rujukan utama yang relevan:

* Freqtrade Official Documentation
* Freqtrade GitHub Repository
* Freqtrade Strategy Documentation
* Freqtrade Backtesting Documentation
* Freqtrade Configuration Documentation
* Freqtrade FreqUI Documentation

Selalu cek dokumentasi resmi karena fitur, command, exchange support, dan best practice dapat berubah.

---

## Disclaimer

Repository ini hanya untuk tujuan edukasi.

Crypto trading memiliki risiko tinggi. Harga aset crypto dapat bergerak sangat cepat dan ekstrem. Bot trading tidak menjamin profit. Backtesting tidak menjamin hasil di masa depan. Dry-run tidak sepenuhnya merepresentasikan kondisi live market.

Jangan menggunakan uang kebutuhan hidup, dana darurat, uang pinjaman, atau uang operasional keluarga untuk trading.

Setiap keputusan trading adalah tanggung jawab pribadi masing-masing pengguna.

Gunakan repository ini sebagai bahan belajar, bukan sebagai finansial advisor.

---

## Closing Note

Belajar crypto trading dengan Freqtrade sebaiknya dimulai dari pemahaman, bukan langsung dari eksekusi.

Untuk gamers, pendekatan terbaik adalah memperlakukan market seperti sistem kompleks yang perlu dibaca, diuji, dan dievaluasi.

Bukan seperti farming. Bukan seperti gacha. Bukan seperti cheat.

Gunakan data, gunakan risk management, dan mulai dari dry-run.

---

2026 - Kenshin Himura
