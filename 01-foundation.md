# Artikel 1 - Foundation

# Crypto Trading untuk Gamers: Dasar Cryptocurrency, Market, dan Freqtrade

> Artikel pertama dari seri tutorial Freqtrade. Artikel ini membahas konsep dasar cryptocurrency, mekanisme pasar, jenis-jenis trading, fungsi trading bot, dan cara membaca candlestick chart. Materi ini ditujukan untuk pembaca dengan latar belakang gaming yang ingin mempelajari crypto trading secara sistematis sebelum melakukan setup teknis.

---

## Daftar Isi

1. [Pendahuluan](#pendahuluan)
2. [Konsep Dasar Cryptocurrency](#konsep-dasar-cryptocurrency)
   * [Cryptocurrency](#cryptocurrency)
   * [Blockchain](#blockchain)
   * [Coin dan Token](#coin-dan-token)
   * [Wallet](#wallet)
   * [Stablecoin](#stablecoin)
3. [Exchange dan API](#exchange-dan-api)
4. [Mekanisme Pasar Crypto](#mekanisme-pasar-crypto)
   * [Jam Operasional Market](#jam-operasional-market)
   * [Pembentukan Harga](#pembentukan-harga)
   * [Pair](#pair)
   * [Order Book](#order-book)
   * [Spread](#spread)
   * [Fee](#fee)
   * [Likuiditas dan Slippage](#likuiditas-dan-slippage)
5. [Dasar Trading](#dasar-trading)
   * [Definisi dan Komponen Trading](#definisi-dan-komponen-trading)
   * [Spot Trading](#spot-trading)
   * [Margin Trading](#margin-trading)
   * [Futures Trading](#futures-trading)
6. [Trading Bot](#trading-bot)
7. [Freqtrade](#freqtrade)
   * [Fitur Utama](#fitur-utama)
   * [Batasan Freqtrade](#batasan-freqtrade)
   * [Backtest, Dry-run, dan Live Trading](#backtest-dry-run-dan-live-trading)
8. [Candlestick Chart](#candlestick-chart)
   * [OHLCV](#ohlcv)
   * [Body dan Wick](#body-dan-wick)
   * [Timeframe](#timeframe)
   * [Volume](#volume)
9. [Membaca Chart Dasar](#membaca-chart-dasar)
10. [Kesalahan Umum Pemula](#kesalahan-umum-pemula)
11. [Checklist Sebelum Lanjut ke Artikel 2](#checklist-sebelum-lanjut-ke-artikel-2)
12. [Glossary](#glossary)
13. [Ringkasan](#ringkasan)
14. [Disclaimer](#disclaimer)

---

## Pendahuluan

### Tujuan

Artikel ini adalah materi prasyarat sebelum masuk ke setup Freqtrade pada artikel berikutnya. Sebelum menyewa VPS, membuat API key, menulis strategy, atau menjalankan bot, pembaca perlu memahami aset yang diperdagangkan, mekanisme market, dan risiko yang muncul ketika dana sungguhan digunakan.

Setelah membaca artikel ini, pembaca diharapkan memahami:

* Cryptocurrency sebagai aset digital beserta risikonya
* Fungsi exchange dan wallet
* Mekanisme pembentukan harga di market crypto
* Perbedaan spot, margin, dan futures trading
* Fungsi dan batasan trading bot
* Posisi Freqtrade dalam workflow pengembangan strategi
* Struktur candlestick chart dan data OHLCV
* Peran risk management dalam trading

### Target Pembaca

Artikel ini ditulis untuk pembaca dengan latar belakang gaming. Pemain game pada umumnya sudah mengenal konsep ekonomi virtual: item market, auction house, supply-demand, kelangkaan item, dan fluktuasi harga saat event. Konsep-konsep tersebut memiliki kemiripan dengan market crypto, sehingga dapat digunakan sebagai titik awal pemahaman.

Perbedaan utamanya ada pada konsekuensi. Kesalahan transaksi di dalam game mengurangi mata uang virtual atau waktu farming. Kesalahan di crypto trading mengurangi dana sungguhan, dan pada sebagian besar kasus transaksi tidak dapat dibatalkan.

Kemampuan yang umum dimiliki gamer - membaca sistem, memahami statistik, melakukan eksperimen build, dan mengikuti perubahan meta - berguna dalam mempelajari trading, tetapi tidak cukup sebagai modal tunggal. Harga di market crypto dipengaruhi banyak faktor sekaligus: transaksi pembeli dan penjual, likuiditas, sentimen, berita, kondisi makro, aktivitas whale, leverage, liquidation, dan algoritma trading. Market crypto perlu diperlakukan sebagai sistem kompleks yang dipelajari dan diuji secara bertahap, dengan risiko yang dikendalikan sejak awal.

---

## Konsep Dasar Cryptocurrency

### Cryptocurrency

Cryptocurrency adalah aset digital yang menggunakan teknologi kriptografi dan berjalan di atas jaringan terdistribusi, umumnya blockchain. Dalam konteks trading, cryptocurrency diperlakukan sebagai aset yang dapat dibeli, dijual, disimpan, dan dipindahkan.

Contoh cryptocurrency yang umum diperdagangkan: Bitcoin (BTC), Ethereum (ETH), BNB, Solana (SOL), XRP, dan Dogecoin (DOGE).

Fungsi setiap aset berbeda-beda. Sebagian merupakan aset utama sebuah jaringan, sebagian lainnya berfungsi sebagai token utilitas, governance token, stablecoin, token game, token DeFi, atau aset spekulatif.

Risiko dasar yang melekat pada aset crypto:

* Nilai intrinsik sering tidak terdefinisi dengan jelas
* Volatilitas harga tinggi
* Proyek dapat gagal atau ditinggalkan
* Exchange dapat mengalami gangguan atau kebangkrutan
* Akses wallet hilang permanen jika private key hilang
* Popularitas komunitas tidak berkorelasi langsung dengan kualitas proyek
* Market cap besar tidak menghilangkan risiko

### Blockchain

Blockchain adalah sistem pencatatan data yang tersusun dalam blok-blok yang saling terhubung secara kronologis. Setiap blok berisi data transaksi dan referensi ke blok sebelumnya. Pada jaringan crypto, blockchain berfungsi mencatat transaksi secara transparan dan sulit dimodifikasi setelah tercatat.

Penggunaan teknologi blockchain pada sebuah proyek tidak menjadikan asetnya otomatis aman atau bernilai. Penilaian aset tetap memerlukan analisis terpisah dari teknologinya.

### Coin dan Token

**Coin** adalah aset crypto yang berjalan di blockchain miliknya sendiri. Contoh: BTC di jaringan Bitcoin, ETH di jaringan Ethereum, SOL di jaringan Solana. Coin umumnya digunakan untuk membayar biaya transaksi, mengamankan jaringan, atau menjalankan fungsi utama ekosistem.

**Token** adalah aset crypto yang berjalan di atas blockchain milik pihak lain, misalnya token yang diterbitkan di jaringan Ethereum, BNB Chain, atau Solana. Fungsi token bervariasi: utilitas aplikasi, governance, reward, aset game, stablecoin, instrumen DeFi, atau aset spekulatif. Kualitas antar token sangat bervariasi; sebagian besar token diterbitkan tanpa fundamental yang memadai.

### Wallet

Wallet adalah perangkat lunak atau perangkat keras untuk mengelola akses terhadap aset crypto. Wallet tidak menyimpan aset secara fisik; aset tercatat di blockchain, dan wallet menyimpan kunci kriptografi untuk mengaksesnya.

Komponen utama wallet:

| Komponen    | Fungsi                                          |
| ----------- | ----------------------------------------------- |
| Address     | Alamat publik untuk menerima crypto             |
| Public key  | Komponen kriptografi yang terkait dengan address |
| Private key | Kunci rahasia untuk mengotorisasi transaksi     |
| Seed phrase | Rangkaian kata untuk memulihkan wallet          |

Private key dan seed phrase adalah komponen paling sensitif:

* Jika hilang, akses ke aset hilang permanen
* Jika bocor, aset dapat diambil pihak lain dan transaksi tidak dapat dibatalkan

Berbeda dengan akun game yang masih memiliki jalur pemulihan melalui customer support, kehilangan private key tidak memiliki mekanisme pemulihan terpusat.

### Stablecoin

Stablecoin adalah aset crypto yang dirancang agar nilainya mengikuti aset acuan, umumnya dolar Amerika Serikat. Contoh: USDT, USDC, dan DAI.

Dalam trading, stablecoin digunakan sebagai aset pembanding pada pair transaksi:

```text
BTC/USDT
ETH/USDT
SOL/USDT
```

Membeli pair BTC/USDT berarti membeli BTC menggunakan USDT. Menjual pair tersebut berarti menjual BTC dan menerima USDT. Stablecoin memudahkan trader berpindah dari aset volatil ke aset yang relatif stabil tanpa keluar dari ekosistem crypto.

Stablecoin tetap memiliki risiko: risiko penerbit, regulasi, likuiditas, depeg (nilai lepas dari acuan), exchange, dan jaringan. Stablecoin tidak setara dengan uang tunai di rekening bank.

---

## Exchange dan API

Exchange adalah platform tempat pengguna membeli dan menjual crypto. Exchange mempertemukan order pembeli dan penjual, mengelola saldo akun, dan mengenakan fee transaksi.

Kriteria pemilihan exchange:

* Legalitas dan izin operasi di wilayah pengguna
* Reputasi keamanan dan riwayat insiden
* Volume transaksi dan likuiditas
* Struktur fee trading
* Dukungan API trading
* Kualitas proses deposit dan withdrawal
* Riwayat gangguan layanan
* Dukungan customer support
* Kompatibilitas dengan Freqtrade (didukung melalui library CCXT)

Untuk penggunaan bot, exchange wajib menyediakan API trading. API memungkinkan Freqtrade membaca data market, mengecek saldo, membuat order, dan mengelola posisi sesuai konfigurasi.

Ketentuan keamanan API key untuk bot trading:

* API key diperlakukan sebagai kredensial sensitif, sama seperti password
* Permission withdrawal tidak diaktifkan
* Jika exchange mendukung, akses API dibatasi berdasarkan IP (IP whitelist)

---

## Mekanisme Pasar Crypto

### Jam Operasional Market

Market crypto beroperasi 24 jam sehari, 7 hari seminggu, tanpa jam tutup seperti pasar saham. Konsekuensinya:

* Harga dapat berubah signifikan di luar jam aktif pengguna
* Berita dapat mempengaruhi harga kapan saja
* Bot dapat beroperasi terus-menerus tanpa menunggu jam market
* Risiko juga aktif sepanjang waktu, sehingga monitoring tetap diperlukan

### Pembentukan Harga

Harga terbentuk dari transaksi antara pembeli dan penjual. Tekanan beli yang dominan mendorong harga naik; tekanan jual yang dominan mendorong harga turun. Selain itu, harga dipengaruhi oleh likuiditas, struktur order book, spread, aktivitas market maker dan whale, leverage dan liquidation di market derivatif, berita, sentimen media sosial, kondisi makro, pergerakan Bitcoin sebagai aset acuan market, volume, dan aktivitas algoritma trading.

Kualitas fundamental proyek dan arah harga jangka pendek tidak selalu sejalan. Aset dengan fundamental baik dapat turun, dan aset dengan fundamental buruk dapat naik dalam periode tertentu. Asumsi "proyek bagus berarti harga naik" tidak dapat dijadikan dasar keputusan trading jangka pendek.

### Pair

Pair adalah pasangan aset yang diperdagangkan di exchange, ditulis dalam format `BASE/QUOTE`.

```text
BTC/USDT
ETH/USDT
BNB/USDT
SOL/USDT
```

Pada pair `BTC/USDT`, BTC adalah base currency (aset yang dibeli atau dijual) dan USDT adalah quote currency (aset pembanding). Harga BTC/USDT sebesar 70.000 berarti 1 BTC diperdagangkan seharga 70.000 USDT.

### Order Book

Order book adalah daftar order beli dan jual yang menunggu eksekusi, terdiri dari dua sisi:

| Sisi | Arti                                                  |
| ---- | ----------------------------------------------------- |
| Bid  | Order beli yang dipasang pembeli pada harga tertentu  |
| Ask  | Order jual yang dipasang penjual pada harga tertentu  |

Market dengan bid dan ask yang tebal di berbagai level harga umumnya lebih likuid. Struktur order book serupa dengan market board di game yang menampilkan daftar harga beli dan jual dari banyak pemain, dengan perbedaan bahwa order book crypto berubah dalam hitungan detik.

### Spread

Spread adalah selisih antara bid tertinggi dan ask terendah.

| Komponen      | Harga   |
| ------------- | ------- |
| Bid tertinggi | 99.900  |
| Ask terendah  | 100.000 |
| Spread        | 100     |

Spread merupakan biaya tidak langsung bagi trader. Spread kecil umum ditemukan pada market likuid; spread besar umum ditemukan pada market dengan likuiditas rendah.

### Fee

Fee adalah biaya transaksi yang dikenakan exchange pada setiap order yang tereksekusi. Pada strategi dengan frekuensi transaksi tinggi seperti scalping, akumulasi fee dapat mengubah strategi yang terlihat profitable di atas kertas menjadi merugi. Perhitungan fee wajib dimasukkan dalam evaluasi setiap strategi. Freqtrade memperhitungkan fee dalam backtesting berdasarkan konfigurasi.

### Likuiditas dan Slippage

Likuiditas menunjukkan seberapa mudah aset dibeli atau dijual tanpa menggerakkan harga secara signifikan.

| Karakteristik    | Market likuid | Market tidak likuid |
| ---------------- | ------------- | ------------------- |
| Volume           | Besar         | Kecil               |
| Spread           | Kecil         | Besar               |
| Order book       | Tebal         | Tipis               |
| Pergerakan harga | Bertahap      | Mudah melompat      |
| Risiko slippage  | Rendah        | Tinggi              |

Slippage adalah selisih antara harga yang diharapkan dan harga eksekusi aktual.

Coin berkapitalisasi kecil sering menarik bagi pemula karena potensi kenaikannya terlihat besar. Risikonya, aset tersebut sulit dijual pada harga wajar ketika market bergerak berlawanan: posisi mudah dibuka tetapi sulit ditutup.

---

## Dasar Trading

### Definisi dan Komponen Trading

Trading adalah aktivitas membeli dan menjual aset untuk mendapatkan selisih harga. Pelaksanaannya memerlukan komponen-komponen berikut:

* Rencana entry
* Rencana exit
* Batas risiko per posisi
* Ukuran posisi (position sizing)
* Pemilihan timeframe
* Pemahaman kondisi market
* Evaluasi hasil secara berkala
* Disiplin menjalankan aturan yang ditetapkan

Prinsip dasarnya: kerugian dijaga tetap kecil dan terkendali ketika analisis salah, dan profit dibiarkan berkembang ketika analisis benar. Arah market tidak dapat dikendalikan; yang dapat dikendalikan adalah ukuran posisi, batas risiko, dan keputusan keluar. Karena itu, pertanyaan "berapa risiko jika analisis ini salah" lebih menentukan hasil jangka panjang dibanding pertanyaan "aset mana yang akan naik".

### Spot Trading

Spot trading adalah pembelian dan penjualan aset crypto secara langsung. Pembelian BTC di spot market menghasilkan saldo BTC yang benar-benar dimiliki di akun exchange.

Alur dasarnya: membeli BTC menggunakan USDT, menyimpan BTC, lalu menjual BTC kembali menjadi USDT.

Risiko spot trading tetap ada karena harga aset dapat turun, tetapi spot trading tidak memiliki risiko liquidation. Spot trading adalah titik awal yang disarankan untuk pemula, dan menjadi fokus utama seri tutorial ini.

### Margin Trading

Margin trading memungkinkan pengguna meminjam dana untuk memperbesar posisi melebihi modal aslinya. Eksposur yang lebih besar berarti kerugian bertambah lebih cepat ketika market bergerak berlawanan, dan pada kondisi tertentu posisi ditutup paksa oleh sistem. Margin trading tidak disarankan untuk pemula.

### Futures Trading

Futures trading adalah perdagangan kontrak derivatif. Trader membuka posisi berdasarkan pergerakan harga aset tanpa harus memiliki aset tersebut.

| Posisi | Arti                              |
| ------ | --------------------------------- |
| Long   | Posisi yang profit jika harga naik  |
| Short  | Posisi yang profit jika harga turun |

Futures umumnya menggunakan leverage, yang memperbesar profit dan kerugian secara proporsional. Posisi yang bergerak berlawanan melebihi batas margin akan terkena liquidation (penutupan paksa).

Futures menarik bagi pemula karena memungkinkan profit di kedua arah harga. Namun futures memerlukan pemahaman tentang leverage, margin, funding fee, liquidation price, dan position sizing. Tanpa pemahaman tersebut, potensi kehilangan seluruh modal sangat tinggi.

Urutan belajar yang digunakan dalam seri ini:

```text
1. Pelajari spot trading
2. Pahami risk management
3. Jalankan dry-run
4. Hindari leverage pada tahap awal
```

---

## Trading Bot

Trading bot adalah program yang menjalankan aturan trading secara otomatis: membaca data market, menghitung indikator, menghasilkan sinyal entry dan exit, mengelola order, dan mencatat aktivitas trading.

Bot menjalankan aturan persis seperti yang dikonfigurasi. Strategi yang buruk akan dijalankan secara konsisten sebagai strategi buruk. Konfigurasi yang salah akan dieksekusi berulang kali. Risk management yang lemah tidak dikoreksi oleh bot.

Kegunaan trading bot:

* Mengurangi keputusan impulsif dan faktor emosi dalam eksekusi
* Menjalankan strategi secara konsisten 24/7
* Menguji strategi terhadap data historis (backtesting)
* Menjalankan simulasi tanpa dana sungguhan (dry-run)
* Mencatat seluruh hasil trade untuk analisis

Batasan trading bot:

* Tidak menjamin profit
* Tidak memprediksi pergerakan harga di masa depan
* Tidak menghilangkan risiko market
* Tidak memperbaiki strategi yang buruk
* Tidak menggantikan pemahaman pengguna
* Tidak menghilangkan kebutuhan monitoring

---

## Freqtrade

Freqtrade adalah bot trading crypto open-source berbasis Python. Freqtrade menyediakan workflow lengkap untuk pengembangan strategi: penulisan strategy dalam Python, pengunduhan data market, backtesting, hyperparameter optimization (hyperopt), dry-run, dan live trading.

Strategy pada Freqtrade ditulis sebagai class Python. Data market diproses dalam bentuk DataFrame (pandas), indikator dihitung dari data OHLCV, lalu kondisi entry dan exit didefinisikan sebagai aturan terhadap DataFrame tersebut. Strategi kemudian diuji terhadap data historis sebelum dijalankan.

Dalam seri ini, Freqtrade diposisikan sebagai lingkungan belajar dan pengujian strategi, dengan urutan kerja:

```text
1. Pahami konsep dasar (artikel ini)
2. Setup environment dengan aman
3. Jalankan dry-run
4. Backtest strategi
5. Evaluasi hasil
6. Pertimbangkan live trading dengan modal kecil
```

### Fitur Utama

* Menjalankan bot trading berbasis strategy Python
* Membaca data market dari exchange yang didukung
* Mendukung spot trading, dan futures trading pada exchange tertentu
* Backtesting terhadap data historis
* Dry-run (simulasi pada kondisi market real-time tanpa order sungguhan)
* Hyperparameter optimization (hyperopt)
* Pencatatan hasil trade ke database
* Monitoring melalui FreqUI (web interface)
* Notifikasi melalui Telegram
* Pengaturan stoploss, minimal ROI, dan trailing stoploss
* Pengelolaan pairlist statis dan dinamis
* Konfigurasi money management

### Batasan Freqtrade

* Tidak menjamin profit
* Tidak menghilangkan risiko volatilitas dan loss
* Tidak memperbaiki strategi yang buruk secara otomatis
* Tidak mengelola keamanan VPS; administrasi server adalah tanggung jawab pengguna
* Tidak melindungi API key yang disimpan secara tidak aman
* Tidak menjamin hasil live sama dengan hasil backtest

### Backtest, Dry-run, dan Live Trading

Tiga mode ini sering disamakan oleh pemula, padahal karakteristiknya berbeda:

| Mode         | Data                    | Order            | Risiko dana |
| ------------ | ----------------------- | ---------------- | ----------- |
| Backtest     | Historis                | Simulasi         | Tidak ada   |
| Dry-run      | Real-time               | Simulasi         | Tidak ada   |
| Live trading | Real-time               | Order sungguhan  | Ada         |

Backtest tidak sepenuhnya merepresentasikan kondisi live karena slippage, latency, perubahan likuiditas, dan partial fill tidak selalu tersimulasi dengan akurat. Dry-run lebih mendekati kondisi live, tetapi tetap tidak mencakup eksekusi order sungguhan dan tekanan psikologis penggunaan dana real.

Urutan validasi strategi: backtest terlebih dahulu, dilanjutkan dry-run dalam periode yang cukup, lalu live trading dimulai dengan modal kecil.

---

## Candlestick Chart

Candlestick chart adalah representasi visual pergerakan harga dalam periode waktu tertentu. Freqtrade menggunakan data candle sebagai input strategi: strategy membaca data candle, menghitung indikator, lalu mengevaluasi kondisi entry dan exit.

### OHLCV

Satu candle terdiri dari empat data harga (OHLC), dan ditambah volume menjadi OHLCV:

| Komponen | Arti                              |
| -------- | --------------------------------- |
| Open     | Harga pembukaan periode           |
| High     | Harga tertinggi periode           |
| Low      | Harga terendah periode            |
| Close    | Harga penutupan periode           |
| Volume   | Jumlah aktivitas transaksi periode |

OHLCV adalah format data standar yang digunakan Freqtrade untuk backtesting dan eksekusi strategi.

### Body dan Wick

**Body** adalah bagian candle antara open dan close. Close di atas open menunjukkan tekanan beli dominan pada periode tersebut; close di bawah open menunjukkan tekanan jual dominan. Secara umum candle bullish ditampilkan hijau dan candle bearish ditampilkan merah, meskipun skema warna dapat berbeda antar platform.

**Wick** (shadow) adalah garis di atas dan di bawah body. Wick atas menunjukkan harga sempat naik tetapi tidak bertahan hingga close; wick bawah menunjukkan harga sempat turun tetapi tidak bertahan hingga close. Wick panjang dapat mengindikasikan rejection pada level harga tertentu, tetapi satu candle tidak cukup sebagai dasar keputusan trading dan harus dibaca bersama konteks lainnya.

### Timeframe

Timeframe adalah periode waktu pembentukan satu candle.

| Timeframe | Periode per candle |
| --------- | ------------------ |
| 1m        | 1 menit            |
| 5m        | 5 menit            |
| 15m       | 15 menit           |
| 1h        | 1 jam              |
| 4h        | 4 jam              |
| 1d        | 1 hari             |

Timeframe kecil menghasilkan lebih banyak sinyal tetapi mengandung lebih banyak noise. Timeframe besar bergerak lebih lambat dan umumnya lebih mudah dibaca oleh pemula. Pada Freqtrade, timeframe adalah parameter strategy yang menentukan karakter strategi secara keseluruhan; strategy 5m dan strategy 1h memiliki perilaku, frekuensi trade, dan profil risiko yang berbeda.

### Volume

Volume menunjukkan jumlah aktivitas transaksi pada periode candle. Pergerakan harga yang disertai volume besar umumnya lebih signifikan dibanding pergerakan dengan volume kecil. Kenaikan harga dengan volume lemah berpotensi tidak bertahan; penurunan harga dengan volume besar mengindikasikan tekanan jual yang kuat. Volume berfungsi sebagai konfirmasi kualitas pergerakan harga, bukan sebagai sinyal tunggal.

---

## Membaca Chart Dasar

Untuk pemula, pembacaan chart cukup difokuskan pada elemen-elemen berikut.

### Arah Trend

| Trend     | Ciri umum                                              |
| --------- | ------------------------------------------------------ |
| Uptrend   | Harga membentuk high dan low yang semakin tinggi       |
| Downtrend | Harga membentuk high dan low yang semakin rendah       |
| Sideways  | Harga bergerak dalam range tanpa arah yang jelas       |

Trading searah trend lebih mudah dikelola dibanding melawan trend. Identifikasi arah trend lebih penting bagi pemula dibanding pencarian titik entry yang presisi.

### Support dan Resistance

**Support** adalah area harga tempat tekanan beli sebelumnya cukup kuat menahan penurunan. **Resistance** adalah area harga tempat tekanan jual sebelumnya cukup kuat menahan kenaikan.

Keduanya adalah area, bukan garis pasti, dan dapat ditembus. Penembusan support atau resistance yang disertai volume besar mengindikasikan perubahan kondisi market dan kelanjutan pergerakan ke arah penembusan.

### Pembacaan dalam Konteks

Satu candle tidak cukup sebagai dasar keputusan. Candle dibaca bersama trend, volume, area support dan resistance, timeframe, dan kondisi market secara umum.

---

## Kesalahan Umum Pemula

Beberapa pola kesalahan yang umum terjadi pada trader pemula, termasuk yang datang dari latar belakang gaming:

**Menahan posisi rugi terlalu lama.** Berbeda dengan farming di game yang memberikan hasil proporsional terhadap waktu, menahan posisi yang salah arah memperbesar kerugian. Market tidak memberikan kompensasi atas durasi menunggu.

**All-in pada coin kecil.** Mengalokasikan seluruh modal pada satu aset berkapitalisasi kecil dengan harapan kenaikan berlipat termasuk kategori spekulasi tanpa risk management, dan secara statistik lebih dekat ke perjudian dibanding trading sistematis.

**Mengandalkan satu strategi untuk semua kondisi.** Strategi yang profitable pada kondisi market tertentu dapat merugi ketika kondisi berubah. Tidak ada strategi yang profitable di seluruh kondisi market, sehingga evaluasi berkala diperlukan.

**Memperlakukan hasil backtest sebagai jaminan.** Backtest menunjukkan performa strategi terhadap data masa lalu. Kondisi live memiliki slippage, fee, latency, dan perubahan likuiditas yang tidak selalu tersimulasi dengan akurat.

**Mengejar candle besar (FOMO).** Membeli setelah kenaikan besar karena takut tertinggal sering menghasilkan entry pada harga puncak lokal. Entry yang buruk dapat merusak performa strategi yang sebenarnya valid.

**Menjalankan live trading tanpa persiapan operasional.** Live trading memerlukan checklist, monitoring, backup, dan prosedur penghentian bot. Persiapan operasional adalah bagian dari risk management, bukan opsional.

---

## Checklist Sebelum Lanjut ke Artikel 2

Sebelum melanjutkan ke artikel setup, pastikan seluruh poin berikut sudah dipahami:

| Checklist                                              | Status |
| ------------------------------------------------------ | ------ |
| Definisi cryptocurrency dan risikonya                  | Wajib  |
| Perbedaan coin dan token                               | Wajib  |
| Fungsi wallet, private key, dan seed phrase            | Wajib  |
| Fungsi dan risiko stablecoin                           | Wajib  |
| Fungsi exchange dan keamanan API key                   | Wajib  |
| Konsep pair trading                                    | Wajib  |
| Struktur order book                                    | Wajib  |
| Pengaruh fee dan spread terhadap strategi              | Wajib  |
| Likuiditas dan slippage                                | Wajib  |
| Mekanisme spot trading                                 | Wajib  |
| Risiko margin dan futures trading                      | Wajib  |
| Fungsi dan batasan trading bot                         | Wajib  |
| Posisi Freqtrade dalam workflow strategi               | Wajib  |
| Format data OHLCV                                      | Wajib  |
| Struktur candlestick dan timeframe                     | Wajib  |
| Perbedaan backtest, dry-run, dan live trading          | Wajib  |

Jika sebagian besar poin belum dipahami, ulangi artikel ini sebelum masuk ke setup teknis.

---

## Glossary

| Istilah         | Penjelasan                                                                 |
| --------------- | -------------------------------------------------------------------------- |
| Cryptocurrency  | Aset digital yang menggunakan teknologi kriptografi dan jaringan digital   |
| Blockchain      | Sistem pencatatan data yang tersusun dalam blok dan saling terhubung       |
| Coin            | Crypto yang berjalan di blockchain miliknya sendiri                        |
| Token           | Crypto yang berjalan di atas blockchain lain                               |
| Wallet          | Alat untuk mengakses dan mengelola aset crypto                             |
| Address         | Alamat publik untuk menerima crypto                                        |
| Private Key     | Kunci rahasia untuk mengotorisasi transaksi                                |
| Seed Phrase     | Rangkaian kata untuk memulihkan wallet                                     |
| Stablecoin      | Crypto yang dirancang mengikuti nilai aset acuan, umumnya USD              |
| Exchange        | Platform untuk membeli dan menjual crypto                                  |
| API Key         | Kredensial untuk menghubungkan aplikasi atau bot ke exchange               |
| Pair            | Pasangan aset yang diperdagangkan, contoh BTC/USDT                         |
| Base Currency   | Aset yang dibeli atau dijual pada sebuah pair                              |
| Quote Currency  | Aset pembanding pada sebuah pair                                           |
| Bid             | Order beli yang dipasang pembeli                                           |
| Ask             | Order jual yang dipasang penjual                                           |
| Order Book      | Daftar order beli dan jual yang menunggu eksekusi                          |
| Spread          | Selisih antara bid tertinggi dan ask terendah                              |
| Fee             | Biaya transaksi yang dikenakan exchange                                    |
| Likuiditas      | Kemudahan membeli atau menjual aset tanpa menggerakkan harga signifikan    |
| Slippage        | Selisih antara harga yang diharapkan dan harga eksekusi                    |
| Volatilitas     | Tingkat perubahan harga dalam periode tertentu                             |
| Spot Trading    | Pembelian dan penjualan aset crypto secara langsung                        |
| Margin Trading  | Trading menggunakan dana pinjaman untuk memperbesar posisi                 |
| Futures Trading | Trading kontrak derivatif berdasarkan pergerakan harga aset                |
| Long            | Posisi yang profit jika harga naik                                         |
| Short           | Posisi yang profit jika harga turun                                        |
| Leverage        | Penggunaan daya ungkit untuk memperbesar eksposur posisi                   |
| Liquidation     | Penutupan paksa posisi karena margin tidak mencukupi                       |
| Funding Fee     | Biaya periodik antar posisi long dan short pada perpetual futures          |
| Trading Bot     | Program yang menjalankan aturan trading secara otomatis                    |
| Freqtrade       | Bot trading crypto open-source berbasis Python                             |
| Strategy        | Class Python berisi aturan entry, exit, dan parameter trading di Freqtrade |
| Backtesting     | Pengujian strategi menggunakan data historis                               |
| Dry-run         | Simulasi bot pada market real-time tanpa order sungguhan                   |
| Hyperopt        | Proses optimasi parameter strategi di Freqtrade                            |
| FreqUI          | Web interface untuk monitoring Freqtrade                                   |
| Pairlist        | Daftar pair yang diperdagangkan oleh bot                                   |
| Candlestick     | Representasi visual pergerakan harga dalam periode tertentu                |
| OHLC            | Open, High, Low, Close                                                     |
| OHLCV           | Open, High, Low, Close, Volume                                             |
| Timeframe       | Periode waktu pembentukan satu candle                                      |
| Support         | Area harga tempat tekanan beli menahan penurunan                           |
| Resistance      | Area harga tempat tekanan jual menahan kenaikan                            |
| Stoploss        | Batas keluar untuk membatasi kerugian                                      |
| Take Profit     | Target keluar ketika posisi mencapai profit tertentu                       |
| Trailing Stop   | Stoploss yang bergerak mengikuti harga ke arah profit                      |
| Drawdown        | Penurunan nilai akun dari titik tertinggi                                  |
| Position Sizing | Penentuan ukuran posisi berdasarkan batas risiko                           |
| FOMO            | Masuk market tanpa rencana karena takut tertinggal peluang                 |
| Overtrading     | Frekuensi trading berlebihan tanpa dasar yang kuat                         |

---

## Ringkasan

* Market crypto memiliki kemiripan struktural dengan ekonomi dalam game, tetapi melibatkan dana sungguhan dengan transaksi yang umumnya tidak dapat dibatalkan.
* Exchange, wallet, stablecoin, pair, order book, fee, likuiditas, dan slippage adalah fondasi yang wajib dipahami sebelum trading.
* Spot trading adalah titik awal yang disarankan; margin dan futures memerlukan pemahaman tambahan tentang leverage dan liquidation.
* Trading bot menjalankan aturan yang dikonfigurasi pengguna secara konsisten; kualitas hasil ditentukan oleh kualitas strategi dan risk management, bukan oleh bot itu sendiri.
* Freqtrade menyediakan workflow lengkap untuk pengembangan strategi: penulisan strategy, backtesting, hyperopt, dry-run, dan live trading.
* Backtest, dry-run, dan live trading adalah tiga mode yang berbeda. Validasi strategi dilakukan berurutan dari backtest, dry-run, lalu live dengan modal kecil.
* Data OHLCV dan candlestick chart adalah input dasar strategi; pembacaannya selalu dilakukan dalam konteks trend, volume, dan struktur market.
* Risk management - batas risiko, position sizing, dan rencana exit - adalah faktor penentu utama keberlangsungan akun.

---

## Disclaimer

Artikel ini ditulis untuk tujuan edukasi.

Crypto trading memiliki risiko tinggi. Harga aset crypto dapat bergerak cepat dan ekstrem. Bot trading tidak menjamin profit. Hasil backtesting tidak menjamin hasil di masa depan. Dry-run tidak sepenuhnya merepresentasikan kondisi live market. Artikel ini bukan nasihat keuangan, bukan rekomendasi investasi, dan bukan ajakan membeli atau menjual aset crypto tertentu.

Jangan menggunakan dana kebutuhan hidup, dana darurat, dana pinjaman, atau dana operasional keluarga untuk trading. Setiap keputusan trading adalah tanggung jawab masing-masing pengguna. Lanjutkan ke artikel setup hanya setelah memahami risiko dasar yang dijelaskan dalam artikel ini.

---

2026 - Kenshin Himura
