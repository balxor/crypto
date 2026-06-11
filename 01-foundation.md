# Artikel 1 — Foundation

# Crypto Trading untuk Gamers: Foundation Sebelum Menggunakan Freqtrade

> Artikel ini membahas dasar cryptocurrency, cara kerja market, jenis trading, trading bot, Freqtrade, dan candlestick chart untuk pembaca yang datang dari dunia gaming dan ingin belajar crypto trading secara sistematis.

---

## Daftar Isi

1. [Tujuan Artikel](#tujuan-artikel)
2. [Kenapa Gamer Perlu Memahami Crypto Market dengan Benar](#kenapa-gamer-perlu-memahami-crypto-market-dengan-benar)
3. [Apa Itu Cryptocurrency](#apa-itu-cryptocurrency)
4. [Blockchain, Coin, Token, dan Wallet](#blockchain-coin-token-dan-wallet)
5. [Stablecoin dan Exchange](#stablecoin-dan-exchange)
6. [Bagaimana Pasar Crypto Bekerja](#bagaimana-pasar-crypto-bekerja)
7. [Pair, Order Book, Spread, Fee, dan Likuiditas](#pair-order-book-spread-fee-dan-likuiditas)
8. [Apa Itu Trading](#apa-itu-trading)
9. [Jenis-Jenis Trading Crypto](#jenis-jenis-trading-crypto)
10. [Apa Itu Trading Bot](#apa-itu-trading-bot)
11. [Pengenalan Freqtrade](#pengenalan-freqtrade)
12. [Apa yang Bisa dan Tidak Bisa Dilakukan Freqtrade](#apa-yang-bisa-dan-tidak-bisa-dilakukan-freqtrade)
13. [Pengenalan Candlestick Chart](#pengenalan-candlestick-chart)
14. [Cara Membaca Chart Dasar](#cara-membaca-chart-dasar)
15. [Mental Model Gamer dalam Trading](#mental-model-gamer-dalam-trading)
16. [Checklist Sebelum Lanjut ke Setup](#checklist-sebelum-lanjut-ke-setup)
17. [Glossary](#glossary)
18. [Key Takeaways](#key-takeaways)
19. [Disclaimer](#disclaimer)

---

## Tujuan Artikel

Artikel ini adalah baseline awal sebelum masuk ke setup Freqtrade. Sebelum menyewa VPS, membuat API key, menulis strategy, atau menjalankan bot, pembaca perlu memahami dulu apa yang sebenarnya sedang diperdagangkan, bagaimana market bekerja, dan risiko apa yang muncul ketika uang sungguhan masuk ke dalam sistem. Target artikel ini bukan membuat pembaca langsung melakukan trading, tetapi untuk membangun cara berpikir yang lebih terstruktur:

* Memahami cryptocurrency sebagai aset digital
* Memahami exchange dan wallet
* Memahami cara harga crypto bergerak
* Memahami perbedaan spot, margin, dan futures
* Memahami fungsi trading bot
* Memahami Freqtrade sebagai alat eksekusi strategi
* Memahami candlestick chart secara dasar
* Memahami bahwa risk management lebih penting daripada mengejar entry

Untuk pembaca yang datang dari dunia game, beberapa konsep market mungkin terasa familiar. Di banyak game, kita mengenal item market, auction house, supply-demand, item langka, harga naik ketika event, dan harga turun ketika supply terlalu banyak. Crypto market punya beberapa pola yang mirip, tetapi konsekuensinya berbeda. Di game, salah membeli item biasanya hanya mengurangi gold, zeny, diamond, atau waktu farming. Di crypto trading, kesalahan dapat menghilangkan uang sungguhan.

---

## Kenapa Gamer Perlu Memahami Crypto Market dengan Benar

Gamer sering memiliki kemampuan yang berguna untuk memahami trading:

* Terbiasa membaca sistem
* Terbiasa memahami statistik
* Terbiasa melakukan eksperimen build
* Terbiasa membaca patch note
* Terbiasa membandingkan item, stat, dan efek
* Terbiasa memahami economy dalam game
* Terbiasa melihat perubahan meta

Kemampuan tersebut membantu, tetapi tidak cukup. Market crypto bukan dungeon yang selalu memiliki pola tetap. Market crypto juga bukan sistem drop rate yang bisa dimenangkan hanya dengan grinding lebih lama. Harga bergerak karena banyak faktor: transaksi pembeli dan penjual, likuiditas, sentimen market, berita, kondisi makro, perilaku whale, leverage, liquidation, dan aktivitas algoritma.

Karena itu, pendekatan yang sehat adalah memperlakukan crypto market sebagai sistem kompleks yang perlu dipelajari, diuji, dan dikendalikan risikonya. Bukan sebagai shortcut untuk profit cepat.

---

## Apa Itu Cryptocurrency

Cryptocurrency adalah aset digital yang menggunakan teknologi kriptografi dan berjalan di atas jaringan digital tertentu. Banyak cryptocurrency berjalan di atas blockchain, yaitu sistem pencatatan data yang tersusun dalam blok dan saling terhubung. Dalam konteks trading, cryptocurrency diperlakukan sebagai aset yang dapat dibeli, dijual, disimpan, dan dipindahkan. Contoh cryptocurrency yang umum dikenal:

* Bitcoin
* Ethereum
* BNB
* Solana
* XRP
* Dogecoin

Namun, tidak semua crypto memiliki fungsi yang sama. Sebagian crypto berfungsi sebagai aset utama sebuah jaringan. Sebagian lain berfungsi sebagai token utilitas, governance token, stablecoin, token game, token DeFi, atau aset spekulatif. Hal penting yang perlu dipahami:

* Crypto tidak selalu memiliki nilai intrinsik yang jelas
* Harga crypto bisa sangat volatil
* Proyek crypto bisa gagal
* Exchange bisa bermasalah
* Wallet bisa kehilangan akses jika private key hilang
* Hype komunitas tidak selalu berarti proyek berkualitas
* Market cap besar tidak otomatis berarti tanpa risiko

Untuk pembaca gamer, crypto bisa dianalogikan seperti aset digital yang memiliki market global. Namun, analogi ini tidak boleh dibawa terlalu jauh. Item game biasanya berada dalam satu ekosistem tertutup. Crypto dapat diperdagangkan lintas platform, dipengaruhi market global, dan nilainya bisa berubah sangat cepat.

---

## Blockchain, Coin, Token, dan Wallet

Sebelum memahami trading, pembaca perlu mengenal beberapa istilah dasar.

### Blockchain

Blockchain adalah sistem pencatatan data yang tersusun dalam blok. Setiap blok berisi data transaksi dan terhubung dengan blok sebelumnya. Dalam banyak jaringan crypto, blockchain digunakan untuk mencatat transaksi secara transparan dan sulit diubah secara sembarangan. Secara sederhana, blockchain dapat dipahami sebagai buku catatan digital yang disimpan dan diverifikasi oleh jaringan. Namun, tidak semua hal yang memakai istilah blockchain otomatis aman, berguna, atau bernilai tinggi. Teknologi yang menarik tidak selalu berarti asetnya layak dibeli.

### Coin

Coin adalah aset crypto yang berjalan di blockchain miliknya sendiri.

Contoh:

* BTC berjalan di jaringan Bitcoin
* ETH berjalan di jaringan Ethereum
* SOL berjalan di jaringan Solana

Coin biasanya digunakan untuk membayar biaya transaksi, menjaga keamanan jaringan, atau menjalankan fungsi utama ekosistem tersebut.

### Token

Token adalah aset crypto yang berjalan di atas blockchain lain. Contoh token dapat berjalan di jaringan Ethereum, BNB Chain, Solana, atau jaringan lain. Token bisa memiliki banyak fungsi:

* Utilitas aplikasi
* Governance
* Reward
* Aset game
* Stablecoin
* DeFi
* NFT ecosystem
* Spekulasi market

Tidak semua token memiliki kualitas yang sama. Banyak token dibuat hanya untuk mengikuti hype.

### Wallet

Wallet adalah alat untuk menyimpan dan mengakses aset crypto. Wallet tidak benar-benar menyimpan coin seperti dompet fisik menyimpan uang. Wallet menyimpan akses terhadap aset yang tercatat di blockchain. Dalam wallet, ada beberapa konsep penting:

| Komponen    | Fungsi                                         |
| ----------- | ---------------------------------------------- |
| Address     | alamat publik untuk menerima crypto            |
| Private key | kunci rahasia untuk mengakses aset             |
| Seed phrase | kumpulan kata untuk memulihkan wallet          |
| Public key  | bagian kriptografi yang terkait dengan address |

Private key dan seed phrase harus dijaga dengan sangat serius. Jika private key atau seed phrase hilang, akses ke aset bisa hilang. Jika private key atau seed phrase dicuri, aset bisa diambil orang lain. Dalam analogi game, private key lebih sensitif daripada password akun. Jika password akun game bocor, mungkin masih ada customer support. Jika private key crypto bocor, transaksi biasanya tidak bisa dibatalkan.

---

## Stablecoin dan Exchange

### Stablecoin

Stablecoin adalah aset crypto yang dirancang agar nilainya mengikuti aset lain, biasanya dolar Amerika Serikat. Contoh stablecoin yang umum dikenal:

* USDT
* USDC
* DAI

Dalam trading crypto, stablecoin sering digunakan sebagai pasangan transaksi. Contoh pair:

```text
BTC/USDT
ETH/USDT
SOL/USDT
```

Jika seseorang membeli BTC/USDT, artinya ia membeli BTC menggunakan USDT. Jika seseorang menjual BTC/USDT, artinya ia menjual BTC dan menerima USDT. Stablecoin memudahkan trader berpindah dari aset volatil ke aset yang relatif stabil. Namun, stablecoin tetap memiliki risiko:

* Risiko penerbit
* Risiko regulasi
* Risiko likuiditas
* Risiko depeg
* Risiko exchange
* Risiko jaringan

Stablecoin tidak boleh dianggap sama persis dengan uang tunai di bank.

### Exchange

Exchange adalah platform tempat pengguna membeli dan menjual crypto. Fungsi exchange mirip marketplace. Ada pembeli, penjual, harga, order, fee, dan saldo akun. Hal yang perlu diperhatikan saat memilih exchange:

* Legalitas dan izin di wilayah pengguna
* Reputasi keamanan
* Volume transaksi
* Biaya trading
* Dukungan API
* Kualitas withdrawal dan deposit
* Riwayat gangguan layanan
* Dukungan customer support
* Kompatibilitas dengan Freqtrade

Untuk penggunaan bot, exchange harus mendukung API trading. API inilah yang memungkinkan Freqtrade membaca market, mengecek saldo, membuat order, dan mengelola posisi sesuai konfigurasi. API key harus dijaga seperti akses penting. Untuk bot trading, permission withdrawal sebaiknya tidak diaktifkan.

---

## Bagaimana Pasar Crypto Bekerja

Crypto market berjalan 24 jam sehari dan 7 hari seminggu. Tidak ada jam tutup seperti pasar saham tradisional. Market tetap bergerak saat malam, akhir pekan, dan hari libur. Karena market berjalan terus, ada beberapa konsekuensi:

* Harga bisa berubah ketika pengguna sedang tidur
* Berita dapat mempengaruhi harga kapan saja
* Bot bisa tetap bekerja tanpa harus menunggu jam market
* Risiko juga aktif sepanjang waktu
* Monitoring tetap diperlukan

Harga crypto terbentuk dari transaksi antara pembeli dan penjual. Jika banyak pembeli agresif masuk, harga bisa naik. Jika banyak penjual agresif keluar, harga bisa turun. Namun, harga tidak hanya bergerak karena jumlah pembeli dan penjual. Harga juga dipengaruhi oleh:

* Likuiditas
* Order book
* Spread
* Market maker
* Whale
* Leverage
* Liquidation
* Berita
* Sentimen sosial media
* Kondisi makro
* Pergerakan Bitcoin
* Volume
* Perilaku algoritma trading

Banyak pemula mengira market bergerak karena alasan yang sederhana. Misalnya:

> "Coin ini bagus, jadi pasti naik."

Di market real, kualitas proyek dan arah harga jangka pendek tidak selalu sejalan. Aset yang bagus tetap bisa turun. Aset yang buruk tetap bisa naik sementara. Market tidak selalu bergerak secara logis dalam jangka pendek.

---

## Pair, Order Book, Spread, Fee, dan Likuiditas

### Pair

Pair adalah pasangan aset yang diperdagangkan di exchange. Contoh:

```text
BTC/USDT
ETH/USDT
BNB/USDT
SOL/USDT
```

Pada pair `BTC/USDT`, BTC adalah aset yang dibeli atau dijual, sedangkan USDT adalah aset pembanding. Jika harga BTC/USDT adalah 70.000, artinya 1 BTC diperdagangkan sekitar 70.000 USDT.

### Order Book

Order book adalah daftar order beli dan jual yang menunggu eksekusi. Order book biasanya memiliki dua sisi:

| Sisi | Arti                                                |
| ---- | --------------------------------------------------- |
| Bid  | daftar pembeli yang ingin membeli di harga tertentu |
| Ask  | daftar penjual yang ingin menjual di harga tertentu |

Jika market memiliki banyak bid dan ask di berbagai level harga, market tersebut biasanya lebih likuid. Dalam analogi game, order book mirip market board yang berisi daftar harga beli dan harga jual dari banyak pemain. Bedanya, di crypto, order book bisa berubah sangat cepat.

### Spread

Spread adalah selisih antara harga beli tertinggi dan harga jual terendah. Contoh sederhana:

| Komponen      | Harga   |
| ------------- | ------- |
| Bid tertinggi | 99.900  |
| Ask terendah  | 100.000 |

Spread = 100. Spread kecil biasanya lebih baik untuk trader karena biaya tidak langsung lebih rendah. Spread besar sering muncul pada market dengan likuiditas rendah.

### Fee

Fee adalah biaya transaksi yang dikenakan exchange. Fee dapat terlihat kecil, tetapi sangat berpengaruh jika strategi melakukan banyak transaksi. Dalam scalping, fee dapat mengubah strategi yang terlihat profit menjadi rugi. Karena itu, setiap strategi harus mempertimbangkan biaya transaksi.

### Likuiditas

Likuiditas menunjukkan seberapa mudah aset dibeli atau dijual tanpa menggerakkan harga terlalu jauh. Aset likuid biasanya memiliki:

* Volume besar
* Spread kecil
* Order book tebal
* Banyak partisipan market

Aset tidak likuid biasanya memiliki:

* Volume kecil
* Spread besar
* Order book tipis
* Harga mudah loncat
* Risiko slippage lebih tinggi

Banyak pemula tertarik pada coin kecil karena potensi naiknya terlihat besar. Masalahnya, coin kecil sering sulit dijual ketika market bergerak berlawanan. Masuk posisi mudah. Keluar posisi bisa sulit.

---

## Apa Itu Trading

Trading adalah aktivitas membeli dan menjual aset dengan tujuan mendapatkan selisih harga. Namun, definisi ini terlalu sederhana jika tidak disertai risk management. Trading yang sehat bukan sekadar membeli ketika harga terlihat murah dan menjual ketika harga terlihat mahal. Trading yang sehat membutuhkan:

* Rencana entry
* Rencana exit
* Batas risiko
* Ukuran posisi
* Pemahaman timeframe
* Pemahaman kondisi market
* Evaluasi hasil
* Disiplin menjalankan aturan

Dalam trading, tujuan utama bukan selalu benar, tetapi menjaga agar kerugian tetap terkendali ketika salah, dan membiarkan profit berkembang ketika analisis benar. Trader pemula sering terlalu fokus pada pertanyaan:

```text
Coin apa yang akan naik?
```

Trader yang lebih matang bertanya:

```text
Berapa risiko jika analisis saya salah?
```

Perbedaan ini penting. Market tidak bisa dikendalikan. Yang bisa dikendalikan adalah ukuran posisi, risiko, dan keputusan keluar.

---

## Jenis-Jenis Trading Crypto

Crypto trading memiliki beberapa jenis utama. Artikel ini hanya menjelaskan konsep dasar. Pembaca pemula sebaiknya memahami spot trading lebih dulu sebelum masuk ke margin atau futures.

### Spot Trading

Spot trading adalah aktivitas membeli dan menjual aset crypto secara langsung. Jika membeli BTC di spot market, pengguna benar-benar memiliki saldo BTC di exchange. Contoh:

* Membeli BTC menggunakan USDT
* Menyimpan BTC
* Menjual BTC kembali menjadi USDT

Spot trading adalah bentuk trading yang paling dasar. Risikonya tetap tinggi karena harga aset bisa turun, tetapi spot trading tidak memiliki risiko liquidation seperti futures. Untuk pemula, spot trading adalah tempat belajar yang lebih masuk akal dibanding futures.

### Margin Trading

Margin trading memungkinkan pengguna meminjam dana untuk memperbesar posisi. Dengan margin, trader dapat memiliki exposure lebih besar dari modal aslinya. Risikonya juga lebih besar. Jika market bergerak berlawanan, kerugian dapat meningkat lebih cepat. Dalam kondisi tertentu, posisi bisa dipaksa ditutup oleh sistem. Margin trading tidak disarankan untuk pemula.

### Futures Trading

Futures trading adalah trading kontrak derivatif. Trader tidak selalu membeli aset secara langsung, tetapi membuka posisi berdasarkan pergerakan harga aset. Dalam futures, trader bisa membuka posisi:

| Posisi | Arti                 |
| ------ | -------------------- |
| Long   | berharap harga naik  |
| Short  | berharap harga turun |

Futures sering menggunakan leverage. Leverage dapat memperbesar profit, tetapi juga memperbesar kerugian. Jika posisi bergerak berlawanan terlalu jauh, trader bisa terkena liquidation. Bagi pemula, futures terlihat menarik karena bisa profit ketika harga naik atau turun. Namun, futures juga merupakan area yang sangat berbahaya jika tidak memahami leverage, margin, funding fee, liquidation price, dan position sizing. Untuk seri ini, pendekatan utama adalah:

```text
Belajar spot dulu.
Pahami risk management dulu.
Gunakan dry-run dulu.
Jangan mulai dari leverage.
```

---

## Apa Itu Trading Bot

Trading bot adalah program yang menjalankan aturan trading secara otomatis. Bot dapat membaca data market, memproses indikator, membuat sinyal entry, membuat sinyal exit, mengelola order, dan mencatat aktivitas trading. Namun, trading bot bukan mesin profit. Bot hanya menjalankan aturan.

Jika aturan strategi buruk, bot akan menjalankan strategi buruk itu secara konsisten. Jika konfigurasi salah, bot bisa menjalankan kesalahan tersebut berulang kali. Jika risk management lemah, bot tidak akan menyelamatkan akun. Trading bot berguna untuk:

* Mengurangi keputusan impulsif
* Menjalankan strategi secara konsisten
* Menguji strategi dengan data historis
* Menjalankan dry-run
* Mencatat hasil trade
* Membantu analisis berbasis data
* Mengurangi emosi dalam eksekusi

Trading bot tidak bisa:

* Menjamin profit
* Memprediksi masa depan dengan pasti
* Menghapus risiko market
* Memperbaiki strategi yang buruk
* Menggantikan pemahaman trader
* Menghilangkan kebutuhan monitoring
* Menghindari kerugian sepenuhnya

Dalam konteks gamer, trading bot bisa dipahami sebagai automation tool untuk menjalankan build yang sudah dirancang. Namun, bot bukan cheat. Bot bukan auto-win. Bot juga bukan cara untuk mengalahkan market tanpa risiko. Bot hanya menjalankan sistem yang dibuat oleh manusia.

---

## Pengenalan Freqtrade

Freqtrade adalah bot trading crypto open-source berbasis Python. Freqtrade banyak digunakan untuk eksperimen strategi karena menyediakan workflow yang lengkap:

* Strategy development
* Backtesting
* Dry-run
* Live trading
* Konfigurasi exchange
* Money management
* Hyperparameter optimization
* Integrasi web UI
* Integrasi Telegram
* Log dan trade history

Dalam seri ini, Freqtrade diperlakukan sebagai trading lab. Artinya, Freqtrade digunakan untuk belajar bagaimana strategi dibuat, diuji, dievaluasi, dan dijalankan secara disiplin. Bukan sebagai alat untuk mencari profit cepat. Untuk pembaca teknis, Freqtrade menarik karena strategi dapat ditulis menggunakan Python. Data market dapat diproses dalam bentuk dataframe.

Indikator dapat dihitung, sinyal entry dan exit dapat dibuat, lalu strategi dapat diuji dengan data historis. Namun, semakin fleksibel sebuah tool, semakin besar pula risiko salah konfigurasi. Karena itu, pendekatan seri ini adalah:

```text
Pahami dulu.
Setup dengan aman.
Jalankan dry-run.
Backtest strategi.
Evaluasi hasil.
Baru pertimbangkan live dengan modal kecil.
```

---

## Apa yang Bisa dan Tidak Bisa Dilakukan Freqtrade

### Freqtrade Bisa Digunakan Untuk

Freqtrade dapat digunakan untuk:

* Menjalankan bot trading berbasis strategi
* Membaca data market dari exchange
* Menjalankan strategi spot
* Menjalankan strategi futures pada exchange yang didukung
* Melakukan backtesting
* Menjalankan dry-run
* Mengoptimasi parameter strategi
* Mencatat hasil trade
* Memantau bot melalui FreqUI
* Mengirim notifikasi melalui Telegram
* Mengatur stoploss, ROI, dan trailing stoploss
* Mengelola pairlist
* Mengelola konfigurasi trading

### Freqtrade Tidak Bisa Digunakan Untuk

Freqtrade tidak dapat:

* Menjamin profit
* Mengetahui masa depan
* Menghilangkan risiko volatilitas
* Menghindari semua loss
* Memperbaiki strategi yang buruk secara otomatis
* Menggantikan pemahaman trader
* Menjaga VPS jika server tidak dikelola
* Melindungi API key jika pengguna menyimpannya sembarangan
* Memastikan hasil live sama dengan backtest

Poin terakhir sangat penting. Backtest adalah simulasi berdasarkan data historis. Dry-run adalah simulasi berjalan pada kondisi market saat ini, tetapi tidak menggunakan order real. Live trading adalah eksekusi order sungguhan di exchange. Ketiganya tidak sama. Strategi yang terlihat bagus di backtest tetap harus diuji melalui dry-run. Setelah itu pun, live trading tetap harus dimulai dengan risiko kecil.

---

## Pengenalan Candlestick Chart

Candlestick chart adalah cara visual untuk menampilkan pergerakan harga dalam periode waktu tertentu. Satu candle biasanya memiliki empat data utama:

| Komponen | Arti            |
| -------- | --------------- |
| Open     | harga pembukaan |
| High     | harga tertinggi |
| Low      | harga terendah  |
| Close    | harga penutupan |

Empat data ini sering disebut OHLC. Jika ditambah volume, formatnya menjadi OHLCV:

| Komponen | Arti                       |
| -------- | -------------------------- |
| Open     | harga pembukaan            |
| High     | harga tertinggi            |
| Low      | harga terendah             |
| Close    | harga penutupan            |
| Volume   | jumlah aktivitas transaksi |

Freqtrade menggunakan data candle untuk menjalankan strategi. Strategi membaca candle, menghitung indikator, lalu menentukan apakah kondisi entry atau exit terpenuhi.

### Body Candle

Body candle menunjukkan jarak antara open dan close. Jika close lebih tinggi dari open, candle menunjukkan tekanan beli pada periode tersebut. Jika close lebih rendah dari open, candle menunjukkan tekanan jual pada periode tersebut. Warna candle tergantung platform chart. Umumnya, candle bullish ditampilkan hijau dan candle bearish ditampilkan merah, tetapi warna bisa berbeda sesuai pengaturan.

### Wick atau Shadow

Wick adalah garis atas dan bawah pada candle. Wick atas menunjukkan harga sempat naik, tetapi tidak bertahan sampai close. Wick bawah menunjukkan harga sempat turun, tetapi tidak bertahan sampai close. Wick panjang dapat menunjukkan rejection, tetapi tidak boleh dibaca sendirian. Satu candle bukan merupakan bukti yang cukup untuk mengambil keputusan trading.

### Timeframe

Timeframe adalah periode waktu yang digunakan untuk membentuk satu candle. Contoh:

| Timeframe | Arti                          |
| --------- | ----------------------------- |
| 1m        | satu candle mewakili 1 menit  |
| 5m        | satu candle mewakili 5 menit  |
| 15m       | satu candle mewakili 15 menit |
| 1h        | satu candle mewakili 1 jam    |
| 4h        | satu candle mewakili 4 jam    |
| 1d        | satu candle mewakili 1 hari   |

Timeframe kecil bergerak cepat dan penuh noise. Timeframe besar bergerak lebih lambat, tetapi sering lebih mudah dibaca oleh pemula. Dalam Freqtrade, timeframe strategy adalah salah satu keputusan penting. Strategy timeframe 5m akan memiliki karakter berbeda dengan strategy timeframe 1h.

### Volume

Volume menunjukkan jumlah aktivitas transaksi pada periode candle tertentu. Pergerakan harga dengan volume besar biasanya lebih penting daripada pergerakan harga dengan volume kecil. Jika harga naik tetapi volume lemah, kenaikan bisa rapuh. Jika harga turun dengan volume besar, tekanan jual bisa kuat. Volume tidak selalu memberikan jawaban pasti, tetapi membantu membaca kualitas pergerakan harga.

---

## Cara Membaca Chart Dasar

Membaca chart bukan soal mencari pola ajaib. Chart membantu trader memahami struktur pergerakan harga. Untuk pemula, fokus awal cukup pada beberapa hal berikut.

### 1. Arah Trend

Trend menunjukkan kecenderungan arah harga.

| Trend     | Ciri Umum                                              |
| --------- | ------------------------------------------------------ |
| Uptrend   | harga cenderung membuat high dan low yang lebih tinggi |
| Downtrend | harga cenderung membuat high dan low yang lebih rendah |
| Sideways  | harga bergerak dalam range tanpa arah jelas            |

Trading melawan trend membutuhkan pengalaman lebih tinggi. Untuk pemula, memahami arah trend lebih penting daripada mencari entry cepat.

### 2. Area Support

Support adalah area harga tempat pembeli sebelumnya cukup kuat menahan penurunan. Support bukan garis yang kaku. Support bisa ditembus. Jika harga menembus support dengan volume besar, kondisi market bisa berubah.

### 3. Area Resistance

Resistance adalah area harga tempat penjual sebelumnya cukup kuat menahan kenaikan. Resistance juga bukan sebuah garis yang pasti. Jika harga menembus resistance dengan volume kuat, market bisa melanjutkan kenaikan.

### 4. Candle Jangan Dibaca Sendiri

Satu candle tidak cukup untuk membuat keputusan. Candle perlu dibaca bersama:

* Trend
* Volume
* Area support
* Area resistance
* Timeframe
* Kondisi market secara umum

### 5. Jangan Mengejar Candle

Pemula sering membeli setelah candle besar naik karena takut tertinggal. Ini sering menjadi jebakan. Candle besar setelah kenaikan panjang bisa berarti momentum kuat, tetapi bisa juga berarti pembeli telat masuk. Dalam trading, entry yang buruk dapat merusak strategi yang sebenarnya benar.

---

## Mental Model Gamer dalam Trading

Gamer punya banyak analogi yang bisa membantu memahami trading, tetapi analogi tersebut harus digunakan dengan hati-hati.

### Trading Bukan Farming

Dalam game, farming lebih lama biasanya menghasilkan resource lebih banyak. Dalam trading, berada lebih lama di posisi buruk bisa memperbesar kerugian. Market tidak memberikan reward hanya karena trader menunggu.

### Trading Bukan Gacha

Dalam gacha, pemain berharap mendapat rare drop. Dalam trading, berharap tanpa risk management adalah kesalahan. All-in pada coin kecil karena berharap naik 100x lebih dekat ke perjudian daripada trading sistematis.

### Strategy Bukan Build Auto-Win

Dalam game, build yang kuat bisa mendominasi meta tertentu. Dalam market, strategi yang bagus tetap bisa gagal ketika kondisi berubah. Tidak ada strategi yang menang di semua kondisi.

### Backtest Bukan Damage Simulator Sempurna

Backtest membantu melihat performa strategi pada data masa lalu. Namun, backtest tidak menjamin performa masa depan. Market live memiliki slippage, fee, latency, perubahan likuiditas, dan kondisi yang tidak selalu muncul sempurna dalam simulasi.

### Dry-run Adalah Training Ground

Dry-run membantu melihat perilaku bot tanpa menggunakan uang sungguhan. Namun, dry-run tetap bukan live trading. Dry-run tidak sepenuhnya merepresentasikan tekanan psikologis, eksekusi order real, dan risiko operasional live.

### Live Trading Adalah Raid

Ketika bot berjalan live, risiko menjadi nyata. Karena itu, perlu checklist, monitoring, backup, dan rencana menghentikan bot. Tidak ada raid serius yang masuk tanpa persiapan. Tidak ada live bot yang sehat tanpa rencana risiko.

---

## Checklist Sebelum Lanjut ke Setup

Sebelum melanjutkan ke Artikel 2, pastikan pembaca memahami poin berikut.

| Checklist                                         | Status |
| ------------------------------------------------- | ------ |
| Memahami apa itu cryptocurrency                   | Wajib  |
| Memahami perbedaan coin dan token                 | Wajib  |
| Memahami fungsi wallet dan private key            | Wajib  |
| Memahami stablecoin                               | Wajib  |
| Memahami exchange dan API key                     | Wajib  |
| Memahami pair trading                             | Wajib  |
| Memahami order book secara dasar                  | Wajib  |
| Memahami fee dan spread                           | Wajib  |
| Memahami likuiditas                               | Wajib  |
| Memahami spot trading                             | Wajib  |
| Memahami risiko margin dan futures                | Wajib  |
| Memahami trading bot bukan mesin profit           | Wajib  |
| Memahami Freqtrade sebagai alat eksekusi strategi | Wajib  |
| Memahami OHLCV                                    | Wajib  |
| Memahami candlestick dasar                        | Wajib  |
| Memahami bahwa backtest bukan jaminan profit      | Wajib  |

Jika sebagian besar poin di atas belum jelas, sebaiknya ulangi artikel ini sebelum masuk ke setup teknis.

---

## Glossary

| Istilah         | Penjelasan                                                                |
| --------------- | ------------------------------------------------------------------------- |
| Cryptocurrency  | aset digital yang menggunakan teknologi kriptografi dan jaringan digital  |
| Blockchain      | sistem pencatatan data yang tersusun dalam blok dan saling terhubung      |
| Coin            | crypto yang berjalan di blockchain miliknya sendiri                       |
| Token           | crypto yang berjalan di atas blockchain lain                              |
| Wallet          | alat untuk mengakses dan mengelola aset crypto                            |
| Address         | alamat publik untuk menerima crypto                                       |
| Private Key     | kunci rahasia untuk mengakses aset crypto                                 |
| Seed Phrase     | kumpulan kata untuk memulihkan wallet                                     |
| Stablecoin      | crypto yang dirancang mengikuti nilai aset tertentu, biasanya USD         |
| Exchange        | platform untuk membeli dan menjual crypto                                 |
| API Key         | kredensial untuk menghubungkan aplikasi atau bot ke exchange              |
| Pair            | pasangan aset yang diperdagangkan, contoh BTC/USDT                        |
| Bid             | harga beli yang dipasang pembeli                                          |
| Ask             | harga jual yang dipasang penjual                                          |
| Order Book      | daftar order beli dan jual di market                                      |
| Spread          | selisih antara bid tertinggi dan ask terendah                             |
| Fee             | biaya transaksi                                                           |
| Likuiditas      | kemudahan membeli atau menjual aset tanpa menggerakkan harga terlalu jauh |
| Slippage        | perbedaan antara harga yang diharapkan dan harga eksekusi                 |
| Volatilitas     | tingkat perubahan harga dalam periode tertentu                            |
| Spot Trading    | membeli dan menjual aset crypto secara langsung                           |
| Margin Trading  | trading menggunakan dana pinjaman untuk memperbesar posisi                |
| Futures Trading | trading kontrak derivatif berdasarkan pergerakan harga aset               |
| Long            | posisi yang mengharapkan harga naik                                       |
| Short           | posisi yang mengharapkan harga turun                                      |
| Leverage        | penggunaan daya ungkit untuk memperbesar exposure posisi                  |
| Liquidation     | penutupan paksa posisi karena margin tidak cukup                          |
| Trading Bot     | program yang menjalankan aturan trading secara otomatis                   |
| Freqtrade       | bot trading crypto open-source berbasis Python                            |
| Backtesting     | pengujian strategi menggunakan data historis                              |
| Dry-run         | simulasi bot tanpa menggunakan uang sungguhan                             |
| Candlestick     | visual pergerakan harga dalam periode tertentu                            |
| OHLC            | Open, High, Low, Close                                                    |
| OHLCV           | Open, High, Low, Close, Volume                                            |
| Timeframe       | periode waktu yang digunakan untuk membentuk candle                       |
| Support         | area harga tempat pembeli sebelumnya menahan penurunan                    |
| Resistance      | area harga tempat penjual sebelumnya menahan kenaikan                     |
| Stoploss        | batas keluar untuk membatasi kerugian                                     |
| Take Profit     | target keluar ketika posisi mencapai profit tertentu                      |
| Drawdown        | penurunan nilai akun dari titik tertinggi                                 |
| FOMO            | takut tertinggal peluang sehingga masuk market tanpa rencana              |
| Overtrading     | terlalu sering trading tanpa alasan yang kuat                             |

---

## Key Takeaways

* Crypto trading memiliki beberapa kemiripan dengan ekonomi dalam game, tetapi risikonya jauh lebih nyata.
* Cryptocurrency adalah aset digital yang dapat diperdagangkan, tetapi tidak semua aset crypto memiliki kualitas dan risiko yang sama.
* Exchange, wallet, stablecoin, pair, order book, fee, dan likuiditas adalah fondasi yang wajib dipahami sebelum trading.
* Spot trading lebih cocok sebagai area belajar awal dibanding margin atau futures.
* Trading bot bukan mesin profit. Bot hanya menjalankan aturan yang dibuat oleh pengguna.
* Freqtrade adalah alat yang kuat untuk belajar, menguji, dan menjalankan strategi, tetapi tetap membutuhkan pemahaman dan risk management.
* Candlestick chart membantu membaca pergerakan harga, tetapi tidak boleh digunakan sebagai satu-satunya dasar keputusan.
* Backtesting dan dry-run adalah tahap wajib sebelum mempertimbangkan live trading.
* Mindset gamer dapat membantu dalam membaca sistem, tetapi harus diarahkan ke disiplin, bukan farming, gacha, atau all-in build.

---

## Disclaimer

Artikel ini hanya untuk tujuan edukasi.

Crypto trading memiliki risiko tinggi. Harga aset crypto dapat bergerak sangat cepat dan ekstrem. Bot trading tidak menjamin profit. Backtesting tidak menjamin hasil masa depan. Dry-run tidak sepenuhnya merepresentasikan kondisi live market. Artikel ini bukan nasihat keuangan, bukan rekomendasi investasi, dan bukan ajakan membeli atau menjual aset crypto tertentu. Jangan menggunakan uang kebutuhan hidup, dana darurat, uang pinjaman, atau uang operasional keluarga untuk trading.

Setiap keputusan trading adalah tanggung jawab pribadi masing-masing pengguna. Lanjutkan membaca setup teknis hanya jika sudah memahami risiko dasar yang dijelaskan dalam artikel ini.

---

2026 - Kenshin Himura
