# Glossary

# Glossary Crypto Trading dan Freqtrade untuk Gamers

> Dokumen ini berisi daftar istilah penting yang digunakan dalam seri artikel crypto trading dengan Freqtrade. Glossary ini ditulis untuk membantu pembaca memahami istilah crypto, trading, bot, risk management, dan server operations secara konsisten.

---

## Daftar Isi

1. [Tujuan Glossary](#tujuan-glossary)
2. [Cara Menggunakan Glossary Ini](#cara-menggunakan-glossary-ini)
3. [Istilah Dasar Cryptocurrency](#istilah-dasar-cryptocurrency)
4. [Istilah Wallet dan Security](#istilah-wallet-dan-security)
5. [Istilah Exchange dan Market](#istilah-exchange-dan-market)
6. [Istilah Trading](#istilah-trading)
7. [Istilah Risk Management](#istilah-risk-management)
8. [Istilah Candlestick dan Chart](#istilah-candlestick-dan-chart)
9. [Istilah Indikator Teknikal](#istilah-indikator-teknikal)
10. [Istilah Freqtrade](#istilah-freqtrade)
11. [Istilah Backtesting dan Strategy](#istilah-backtesting-dan-strategy)
12. [Istilah Server dan Operations](#istilah-server-dan-operations)
13. [Istilah yang Sering Disalahpahami](#istilah-yang-sering-disalahpahami)
14. [Ringkasan untuk Pembaca Baru](#ringkasan-untuk-pembaca-baru)

---

## Tujuan Glossary

Glossary ini dibuat agar pembaca memiliki referensi istilah yang konsisten selama mengikuti seluruh seri artikel. Seri ini membahas beberapa area sekaligus:

* Cryptocurrency.
* Exchange.
* Trading.
* Candlestick.
* Trading Bot.
* Freqtrade.
* Strategy.
* Backtesting.
* Risk Management.
* VPS.
* Server Operations.

Karena target pembaca adalah gamers yang baru masuk ke crypto trading, beberapa istilah akan dijelaskan dengan analogi sederhana. Namun, analogi tersebut tetap dijaga agar tidak menyesatkan. Crypto trading bukan game. Bot trading bukan auto-win. Backtesting bukan jaminan hasil masa depan.

---

## Cara Menggunakan Glossary Ini

Gunakan glossary ini sebagai dokumen referensi. Jika menemukan istilah baru di artikel, cari istilah tersebut di dokumen ini. Rekomendasi penggunaan:

* Baca Bagian Dasar Sebelum Masuk Artikel Strategy.
* Gunakan Glossary Saat Membaca Config Freqtrade.
* Gunakan Glossary Saat Membaca Hasil Backtest.
* Gunakan Glossary Saat Melakukan Troubleshooting.
* Tambahkan Istilah Baru Jika Repository Berkembang.

Jika ada istilah yang memiliki makna berbeda tergantung konteks, penjelasannya akan diberi catatan.

---

## Istilah Dasar Cryptocurrency

| Istilah            | Penjelasan                                                                                                  |
| ------------------ | ----------------------------------------------------------------------------------------------------------- |
| Cryptocurrency     | Aset digital yang menggunakan kriptografi dan berjalan di jaringan digital tertentu.                        |
| Crypto Asset       | Istilah umum untuk aset digital seperti coin, token, stablecoin, dan aset blockchain lainnya.               |
| Blockchain         | Sistem pencatatan data yang tersusun dalam blok dan saling terhubung.                                       |
| Coin               | Crypto yang berjalan di blockchain miliknya sendiri.                                                        |
| Token              | Crypto yang berjalan di atas blockchain lain.                                                               |
| Native Coin        | Coin utama dalam sebuah blockchain, misalnya ETH di Ethereum atau SOL di Solana.                            |
| Stablecoin         | Crypto yang dirancang mengikuti nilai aset lain, umumnya dolar Amerika Serikat.                             |
| Depeg              | Kondisi ketika stablecoin tidak lagi mengikuti nilai aset acuannya secara stabil.                           |
| Market Cap         | Nilai kapitalisasi pasar suatu aset, biasanya dihitung dari harga dikali jumlah supply beredar.             |
| Circulating Supply | Jumlah token atau coin yang beredar di market.                                                              |
| Total Supply       | Total token atau coin yang sudah dibuat atau tersedia.                                                      |
| Max Supply         | Jumlah maksimal token atau coin yang dapat ada, jika dibatasi.                                              |
| Tokenomics         | Struktur ekonomi sebuah token, termasuk supply, distribusi, utility, dan insentif.                          |
| Utility            | Fungsi atau kegunaan token dalam ekosistem tertentu.                                                        |
| Governance Token   | Token yang digunakan untuk voting atau pengambilan keputusan dalam protokol tertentu.                       |
| DeFi               | Decentralized Finance, ekosistem layanan finansial berbasis blockchain.                                     |
| NFT                | Non-Fungible Token, aset digital unik yang tidak dapat dipertukarkan satu banding satu seperti token biasa. |
| Gas Fee            | Biaya transaksi di jaringan blockchain tertentu.                                                            |
| Network Fee        | Biaya yang dibayarkan untuk memproses transaksi di blockchain.                                              |
| Mainnet            | Jaringan utama blockchain yang digunakan untuk transaksi sungguhan.                                         |
| Testnet            | Jaringan percobaan untuk testing tanpa menggunakan aset bernilai nyata.                                     |

Catatan:

Stablecoin membantu trader berpindah dari aset volatil ke aset yang relatif stabil. Namun, stablecoin tetap memiliki risiko penerbit, regulasi, likuiditas, dan depeg.

---

## Istilah Wallet dan Security

| Istilah               | Penjelasan                                                                    |
| --------------------- | ----------------------------------------------------------------------------- |
| Wallet                | Alat untuk mengakses dan mengelola aset crypto.                               |
| Address               | Alamat publik untuk menerima aset crypto.                                     |
| Private Key           | Kunci rahasia untuk mengakses aset crypto.                                    |
| Seed Phrase           | Kumpulan kata yang dapat digunakan untuk memulihkan wallet.                   |
| Public Key            | Komponen kriptografi yang terkait dengan address.                             |
| Hot Wallet            | Wallet yang terhubung ke internet.                                            |
| Cold Wallet           | Wallet yang disimpan offline atau tidak selalu terhubung ke internet.         |
| Custodial Wallet      | Wallet yang private key-nya dikelola pihak ketiga, seperti exchange.          |
| Non-Custodial Wallet  | Wallet yang private key-nya dikendalikan sendiri oleh pengguna.               |
| 2FA                   | Two-Factor Authentication, lapisan keamanan tambahan selain password.         |
| API Key               | Kredensial untuk menghubungkan aplikasi atau bot ke exchange.                 |
| API Secret            | Secret yang digunakan bersama API key untuk autentikasi.                      |
| API Permission        | Hak akses yang diberikan kepada API key, misalnya read, trade, atau withdraw. |
| IP Whitelist          | Pembatasan API agar hanya dapat digunakan dari IP tertentu.                   |
| Withdrawal Permission | Izin API untuk melakukan penarikan dana.                                      |
| Secret Leak           | Kondisi ketika API key, private key, token, atau password bocor.              |
| Phishing              | Upaya menipu pengguna agar memberikan kredensial atau secret.                 |
| Malware               | Software berbahaya yang dapat mencuri data, key, atau akses akun.             |
| Backup                | Salinan data penting untuk pemulihan.                                         |
| Recovery              | Proses memulihkan akses atau data dari backup.                                |

Prinsip penting:

* Jangan Pernah Membagikan Private Key.
* Jangan Pernah Membagikan Seed Phrase.
* Jangan Aktifkan Withdrawal Permission untuk Bot.
* Jangan Menyimpan API Secret di Repository Publik.
* Gunakan 2FA untuk Exchange.
* Gunakan IP Whitelist Jika Exchange Mendukung.

Analogi gamer:

API key dengan permission trade mirip akses karakter ke market. API key dengan permission withdrawal jauh lebih berbahaya karena bisa memindahkan aset keluar dari akun.

---

## Istilah Exchange dan Market

| Istilah      | Penjelasan                                                                     |
| ------------ | ------------------------------------------------------------------------------ |
| Exchange     | Platform untuk membeli dan menjual crypto.                                     |
| CEX          | Centralized Exchange, exchange yang dikelola perusahaan atau entitas terpusat. |
| DEX          | Decentralized Exchange, exchange berbasis smart contract.                      |
| Pair         | Pasangan aset yang diperdagangkan, misalnya BTC/USDT.                          |
| Base Asset   | Aset utama dalam pair, misalnya BTC dalam BTC/USDT.                            |
| Quote Asset  | Aset pembanding dalam pair, misalnya USDT dalam BTC/USDT.                      |
| Order Book   | Daftar order beli dan jual di market.                                          |
| Bid          | Harga beli yang ditawarkan pembeli.                                            |
| Ask          | Harga jual yang ditawarkan penjual.                                            |
| Spread       | Selisih antara bid tertinggi dan ask terendah.                                 |
| Liquidity    | Kemudahan membeli atau menjual aset tanpa menggerakkan harga terlalu jauh.     |
| Volume       | Jumlah aktivitas transaksi dalam periode tertentu.                             |
| Market Order | Order yang langsung dieksekusi pada harga market terbaik yang tersedia.        |
| Limit Order  | Order yang hanya dieksekusi pada harga tertentu atau lebih baik.               |
| Maker        | Pihak yang menambahkan likuiditas ke order book.                               |
| Taker        | Pihak yang mengambil likuiditas dari order book.                               |
| Trading Fee  | Biaya yang dikenakan exchange untuk transaksi.                                 |
| Maker Fee    | Fee untuk order yang menambah likuiditas.                                      |
| Taker Fee    | Fee untuk order yang mengambil likuiditas.                                     |
| Slippage     | Perbedaan antara harga yang diharapkan dan harga eksekusi aktual.              |
| Rate Limit   | Batas jumlah request API dalam periode tertentu.                               |
| Maintenance  | Periode saat exchange melakukan perawatan sistem.                              |
| Delisting    | Penghapusan pair atau aset dari exchange.                                      |
| Listing      | Penambahan aset baru ke exchange.                                              |

Catatan:

Market dengan volume tinggi dan spread kecil biasanya lebih sehat untuk pemula. Market dengan likuiditas rendah dapat membuat entry mudah tetapi exit sulit.

---

## Istilah Trading

| Istilah          | Penjelasan                                                                       |
| ---------------- | -------------------------------------------------------------------------------- |
| Trading          | Aktivitas membeli dan menjual aset untuk mendapatkan selisih harga.              |
| Trader           | Orang atau sistem yang melakukan trading.                                        |
| Investing        | Membeli aset dengan orientasi jangka panjang.                                    |
| Spot Trading     | Membeli dan menjual aset crypto secara langsung.                                 |
| Margin Trading   | Trading menggunakan dana pinjaman untuk memperbesar posisi.                      |
| Futures Trading  | Trading kontrak derivatif berdasarkan pergerakan harga aset.                     |
| Long             | Posisi yang mengharapkan harga naik.                                             |
| Short            | Posisi yang mengharapkan harga turun.                                            |
| Leverage         | Penggunaan daya ungkit untuk memperbesar exposure posisi.                        |
| Liquidation      | Penutupan paksa posisi karena margin tidak cukup.                                |
| Entry            | Kondisi atau titik masuk posisi.                                                 |
| Exit             | Kondisi atau titik keluar posisi.                                                |
| Take Profit      | Target keluar ketika posisi mencapai profit tertentu.                            |
| Stoploss         | Batas keluar untuk membatasi kerugian.                                           |
| Break Even       | Kondisi ketika posisi tidak profit dan tidak rugi setelah memperhitungkan biaya. |
| Scalping         | Trading jangka sangat pendek dengan target kecil dan frekuensi tinggi.           |
| Day Trading      | Trading yang biasanya dibuka dan ditutup dalam hari yang sama.                   |
| Swing Trading    | Trading yang memanfaatkan pergerakan beberapa hari sampai beberapa minggu.       |
| Position Trading | Trading atau investasi jangka lebih panjang.                                     |
| Overtrading      | Terlalu sering trading tanpa alasan yang kuat.                                   |
| Revenge Trading  | Trading karena ingin membalas kerugian sebelumnya.                               |
| FOMO             | Fear Of Missing Out, takut tertinggal peluang.                                   |
| Panic Selling    | Menjual karena panik, bukan karena rencana.                                      |
| Market Condition | Kondisi market saat ini, misalnya bullish, bearish, atau sideways.               |

Catatan:

Spot trading lebih cocok untuk pembelajaran awal. Futures dan margin memiliki risiko tambahan seperti leverage, margin call, liquidation, dan funding fee.

---

## Istilah Risk Management

| Istilah              | Penjelasan                                               |
| -------------------- | -------------------------------------------------------- |
| Risk Management      | Proses mengatur risiko agar kerugian tetap terkendali.   |
| Capital              | Modal yang digunakan untuk trading.                      |
| Position Size        | Ukuran posisi dalam satu trade.                          |
| Position Sizing      | Cara menentukan ukuran posisi.                           |
| Exposure             | Total nilai posisi yang sedang terbuka atau berisiko.    |
| Max Open Trades      | Jumlah maksimal posisi aktif secara bersamaan.           |
| Risk Per Trade       | Risiko maksimal yang bersedia diterima pada satu trade.  |
| Risk Limit           | Batas risiko yang ditentukan sebelum trading.            |
| Daily Loss Limit     | Batas kerugian harian.                                   |
| Weekly Loss Limit    | Batas kerugian mingguan.                                 |
| Drawdown             | Penurunan nilai akun dari titik tertinggi.               |
| Max Drawdown         | Drawdown terbesar dalam periode tertentu.                |
| Winrate              | Persentase trade yang berakhir profit.                   |
| Risk Reward Ratio    | Perbandingan antara potensi risiko dan potensi reward.   |
| Volatility           | Tingkat perubahan harga dalam periode tertentu.          |
| Correlation          | Hubungan pergerakan antara dua aset.                     |
| Diversification      | Penyebaran risiko ke beberapa aset atau strategi.        |
| Stoploss Discipline  | Disiplin mengikuti batas kerugian yang sudah ditetapkan. |
| Capital Preservation | Fokus menjaga modal agar tetap bertahan.                 |
| Ruin Risk            | Risiko kehilangan modal secara besar hingga sulit pulih. |

Prinsip penting:

* Jangan Ukur Strategy Hanya dari Profit.
* Ukur Strategy dari Drawdown, Konsistensi, dan Risiko.
* Jangan Biarkan Satu Trade Menghancurkan Akun.
* Jangan Naikkan Modal Hanya Karena Satu Hasil Profit.
* Jangan Menggunakan Uang Kebutuhan Hidup untuk Trading.

Analogi gamer:

Risk management mirip pengelolaan resource sebelum raid. Jika semua resource habis di satu percobaan, kesempatan belajar berikutnya hilang.

---

## Istilah Candlestick dan Chart

| Istilah        | Penjelasan                                                           |
| -------------- | -------------------------------------------------------------------- |
| Candlestick    | Visual pergerakan harga dalam periode tertentu.                      |
| Candle         | Satu unit candlestick.                                               |
| Open           | Harga pembukaan candle.                                              |
| High           | Harga tertinggi candle.                                              |
| Low            | Harga terendah candle.                                               |
| Close          | Harga penutupan candle.                                              |
| OHLC           | Open, High, Low, Close.                                              |
| OHLCV          | Open, High, Low, Close, Volume.                                      |
| Body           | Bagian candle antara open dan close.                                 |
| Wick           | Garis atas atau bawah candle.                                        |
| Shadow         | Istilah lain untuk wick.                                             |
| Bullish Candle | Candle yang menutup lebih tinggi dari open.                          |
| Bearish Candle | Candle yang menutup lebih rendah dari open.                          |
| Timeframe      | Periode waktu untuk membentuk candle.                                |
| Trend          | Arah umum pergerakan harga.                                          |
| Uptrend        | Kondisi harga cenderung naik.                                        |
| Downtrend      | Kondisi harga cenderung turun.                                       |
| Sideways       | Kondisi harga bergerak dalam range tanpa arah jelas.                 |
| Support        | Area harga tempat pembeli sebelumnya cukup kuat.                     |
| Resistance     | Area harga tempat penjual sebelumnya cukup kuat.                     |
| Breakout       | Kondisi harga menembus area penting seperti resistance atau support. |
| Pullback       | Koreksi sementara dalam trend.                                       |
| Reversal       | Perubahan arah trend.                                                |
| Retest         | Harga kembali menguji area yang baru ditembus.                       |
| Noise          | Pergerakan kecil yang tidak selalu memiliki makna besar.             |

Catatan:

Candlestick tidak boleh dibaca sendirian. Candle perlu dibaca bersama trend, volume, support, resistance, dan timeframe.

---

## Istilah Indikator Teknikal

| Istilah            | Penjelasan                                                                                   |
| ------------------ | -------------------------------------------------------------------------------------------- |
| Indicator          | Perhitungan berbasis data market untuk membantu analisis.                                    |
| Technical Analysis | Analisis berdasarkan harga, volume, dan indikator.                                           |
| SMA                | Simple Moving Average, rata-rata harga dalam periode tertentu.                               |
| EMA                | Exponential Moving Average, moving average yang memberi bobot lebih besar pada data terbaru. |
| RSI                | Relative Strength Index, indikator momentum dengan skala 0 sampai 100.                       |
| MACD               | Moving Average Convergence Divergence, indikator momentum dan trend.                         |
| Bollinger Bands    | Indikator volatilitas berbasis moving average dan standar deviasi.                           |
| ATR                | Average True Range, indikator volatilitas.                                                   |
| Volume MA          | Rata-rata volume dalam periode tertentu.                                                     |
| Overbought         | Kondisi ketika aset dianggap terlalu tinggi berdasarkan indikator tertentu.                  |
| Oversold           | Kondisi ketika aset dianggap terlalu rendah berdasarkan indikator tertentu.                  |
| Crossover          | Kondisi ketika satu garis indikator memotong garis lain.                                     |
| Signal Line        | Garis referensi dalam indikator tertentu.                                                    |
| Lagging Indicator  | Indikator yang bereaksi setelah harga bergerak.                                              |
| Leading Indicator  | Indikator yang mencoba memberi sinyal lebih awal.                                            |
| False Signal       | Sinyal yang terlihat valid tetapi gagal menghasilkan pergerakan sesuai harapan.              |
| Filter             | Syarat tambahan untuk mengurangi sinyal buruk.                                               |

Catatan:

Indikator bukan alat prediksi pasti. Indikator hanya membantu membaca data. Strategy yang sehat biasanya tidak bergantung pada satu indikator saja.

---

## Istilah Freqtrade

| Istilah              | Penjelasan                                                               |
| -------------------- | ------------------------------------------------------------------------ |
| Freqtrade            | Bot trading crypto open-source berbasis Python.                          |
| Bot                  | Program yang menjalankan aturan trading secara otomatis.                 |
| Strategy             | File Python yang berisi aturan entry, exit, indikator, dan risk control. |
| Config               | File konfigurasi bot, biasanya `config.json`.                            |
| User Data            | Folder utama untuk config, strategy, data, log, dan hasil backtest.      |
| Pairlist             | Daftar pair yang boleh dipantau atau diperdagangkan bot.                 |
| Whitelist            | Daftar pair yang diizinkan.                                              |
| Blacklist            | Daftar pair yang dilarang.                                               |
| Dry-run              | Mode simulasi tanpa order sungguhan.                                     |
| Live Trading         | Mode trading dengan order sungguhan.                                     |
| Stake Currency       | Aset yang digunakan sebagai modal trading, misalnya USDT.                |
| Stake Amount         | Jumlah modal per trade.                                                  |
| Dry-run Wallet       | Saldo simulasi untuk mode dry-run.                                       |
| Trading Mode         | Mode trading seperti spot atau futures.                                  |
| Timeframe            | Timeframe candle yang digunakan strategy.                                |
| Startup Candle Count | Jumlah candle awal yang dibutuhkan sebelum strategy aktif.               |
| Minimal ROI          | Target profit minimum berdasarkan durasi trade.                          |
| Stoploss             | Batas kerugian maksimal berdasarkan strategy.                            |
| Trailing Stoploss    | Stoploss yang dapat bergerak mengikuti profit.                           |
| FreqUI               | Web interface untuk memantau dan mengontrol Freqtrade.                   |
| API Server           | Server lokal Freqtrade yang digunakan oleh FreqUI dan API.               |
| Telegram Integration | Integrasi Freqtrade dengan Telegram untuk notifikasi dan kontrol.        |
| Docker Compose       | Tool untuk menjalankan container Freqtrade secara terstruktur.           |

Catatan:

Freqtrade adalah alat eksekusi strategi. Freqtrade tidak menjamin profit, tidak memprediksi masa depan, dan tidak memperbaiki strategy yang buruk secara otomatis.

---

## Istilah Backtesting dan Strategy

| Istilah              | Penjelasan                                                                                   |
| -------------------- | -------------------------------------------------------------------------------------------- |
| Backtesting          | Pengujian strategy menggunakan data historis.                                                |
| Historical Data      | Data market masa lalu yang digunakan untuk backtesting.                                      |
| Timerange            | Periode waktu yang dipakai dalam backtest.                                                   |
| Strategy Development | Proses membuat dan memperbaiki strategy.                                                     |
| Entry Signal         | Sinyal untuk masuk posisi.                                                                   |
| Exit Signal          | Sinyal untuk keluar posisi.                                                                  |
| Enter Long           | Sinyal masuk posisi long.                                                                    |
| Exit Long            | Sinyal keluar dari posisi long.                                                              |
| Dataframe            | Struktur data berbentuk tabel yang digunakan Freqtrade untuk memproses candle dan indikator. |
| populate_indicators  | Function untuk menghitung indikator dalam strategy Freqtrade.                                |
| populate_entry_trend | Function untuk membuat sinyal entry.                                                         |
| populate_exit_trend  | Function untuk membuat sinyal exit.                                                          |
| Hyperopt             | Proses optimasi parameter strategy.                                                          |
| Parameter            | Nilai yang mengatur perilaku strategy.                                                       |
| Optimization         | Proses mencari kombinasi parameter yang lebih baik berdasarkan objective tertentu.           |
| Hyperopt Loss        | Fungsi penilaian dalam proses hyperopt.                                                      |
| Overfitting          | Kondisi ketika strategy terlalu cocok dengan data masa lalu tetapi buruk di data baru.       |
| Out-of-sample Test   | Pengujian strategy pada data yang tidak digunakan saat optimasi.                             |
| Walk-forward Testing | Pengujian strategy secara bertahap pada periode waktu berbeda.                               |
| Trade Count          | Jumlah trade dalam backtest.                                                                 |
| Average Duration     | Rata-rata durasi trade.                                                                      |
| Exit Reason          | Alasan posisi ditutup.                                                                       |
| Pair Performance     | Performa strategy pada masing-masing pair.                                                   |

Prinsip penting:

* Backtesting Hanya Menguji Masa Lalu.
* Hyperopt Bukan Mesin Profit.
* Strategy Sederhana Lebih Mudah Diaudit.
* Overfitting Adalah Risiko Besar dalam Bot Trading.
* Dry-run Tetap Dibutuhkan Setelah Backtest.

Analogi gamer:

Backtesting mirip damage simulator, tetapi simulator tidak selalu sama dengan pertarungan sebenarnya. Live market memiliki fee, slippage, latency, emosi, dan kondisi tak terduga.

---

## Istilah Server dan Operations

| Istilah           | Penjelasan                                                     |
| ----------------- | -------------------------------------------------------------- |
| VPS               | Virtual Private Server untuk menjalankan bot.                  |
| Ubuntu Server     | Sistem operasi server yang umum digunakan untuk deployment.    |
| SSH               | Protokol untuk mengakses server dari terminal.                 |
| SSH Key           | Kunci autentikasi untuk login SSH.                             |
| Root User         | User dengan akses tertinggi di Linux.                          |
| Sudo              | Perintah untuk menjalankan command dengan hak administrator.   |
| Firewall          | Sistem pembatas akses network.                                 |
| UFW               | Uncomplicated Firewall, tool firewall sederhana di Ubuntu.     |
| Docker            | Platform untuk menjalankan aplikasi dalam container.           |
| Container         | Lingkungan aplikasi yang terisolasi.                           |
| Docker Compose    | Tool untuk mengatur beberapa container atau service.           |
| Apache            | Web server yang dapat digunakan sebagai reverse proxy.         |
| Reverse Proxy     | Komponen yang meneruskan request dari domain ke service lokal. |
| SSL               | Teknologi enkripsi koneksi web.                                |
| HTTPS             | HTTP dengan enkripsi SSL atau TLS.                             |
| Certbot           | Tool untuk membuat dan memperbarui sertifikat Let's Encrypt.   |
| Domain            | Nama alamat website atau service.                              |
| DNS               | Sistem yang mengarahkan domain ke IP server.                   |
| Log               | Catatan aktivitas sistem atau aplikasi.                        |
| Monitoring        | Proses memantau status bot, server, dan service.               |
| Alerting          | Notifikasi otomatis ketika terjadi kondisi penting.            |
| Maintenance       | Perawatan sistem seperti update dan pengecekan resource.       |
| Backup            | Salinan data penting.                                          |
| Restore           | Proses mengembalikan data dari backup.                         |
| Rollback          | Mengembalikan sistem ke kondisi sebelumnya.                    |
| Changelog         | Catatan perubahan software.                                    |
| Incident Response | Langkah menangani masalah operasional.                         |
| Kill Switch       | Rencana menghentikan bot dengan cepat.                         |

Prinsip penting:

* Jangan Membuka Port API Bot Langsung ke Internet.
* Gunakan HTTPS untuk Akses Web.
* Gunakan Password Kuat.
* Aktifkan Firewall.
* Backup Config dan Strategy.
* Pahami Cara Stop Bot Sebelum Live.
* Update Server Secara Berkala.

---

## Istilah yang Sering Disalahpahami

### Bot Trading

Pemahaman keliru:

```text id="1m7ha6"
Bot trading adalah mesin profit otomatis.
```

Pemahaman yang lebih benar:

```text id="p6u38p"
Bot trading adalah alat untuk menjalankan aturan trading secara otomatis.
```

Bot tidak menciptakan edge sendiri. Edge harus berasal dari strategy, risk management, dan disiplin operasional.

### Backtesting

Pemahaman keliru:

```text id="d2q7fp"
Backtest profit berarti strategy pasti profit di live.
```

Pemahaman yang lebih benar:

```text id="qcse3e"
Backtest hanya menunjukkan performa strategy pada data historis.
```

Backtest harus dilanjutkan dengan validasi, dry-run, dan evaluasi live kecil jika benar-benar siap.

### Dry-run

Pemahaman keliru:

```text id="jgmq3w"
Dry-run sama dengan live trading tanpa risiko.
```

Pemahaman yang lebih benar:

```text id="d0sxk2"
Dry-run membantu melihat perilaku bot, tetapi tidak sepenuhnya merepresentasikan live execution.
```

Dry-run tidak selalu menangkap slippage, latency, order fill, dan tekanan psikologis live trading.

### Winrate

Pemahaman keliru:

```text id="o5m5bc"
Winrate tinggi berarti strategy bagus.
```

Pemahaman yang lebih benar:

```text id="12y0wd"
Winrate harus dibaca bersama average profit, average loss, drawdown, dan risk reward.
```

Strategy dengan winrate tinggi tetap bisa rugi jika loss kecil tidak dikontrol.

### Stoploss

Pemahaman keliru:

```text id="8jxp23"
Stoploss adalah tanda strategy gagal.
```

Pemahaman yang lebih benar:

```text id="s2v76m"
Stoploss adalah mekanisme untuk membatasi kerugian.
```

Stoploss membantu menjaga modal agar tetap bisa digunakan untuk peluang berikutnya.

### Futures

Pemahaman keliru:

```text id="7anxzv"
Futures lebih baik karena bisa profit saat harga naik atau turun.
```

Pemahaman yang lebih benar:

```text id="5x2p8v"
Futures memiliki risiko leverage, liquidation, funding fee, dan margin management.
```

Futures tidak cocok sebagai area belajar pertama untuk pemula.

### Hyperopt

Pemahaman keliru:

```text id="8t3bnw"
Hyperopt akan menemukan parameter terbaik untuk live trading.
```

Pemahaman yang lebih benar:

```text id="bk9jk0"
Hyperopt mencari parameter yang baik pada data dan objective tertentu, tetapi tetap bisa overfitting.
```

Parameter hasil hyperopt harus diuji di data lain dan dry-run.

### Strategy Komunitas

Pemahaman keliru:

```text id="exlojx"
Strategy publik yang banyak dipakai pasti aman.
```

Pemahaman yang lebih benar:

```text id="lyk5wh"
Strategy publik harus diaudit, dibacktest ulang, dan dipahami sebelum digunakan.
```

Copy-paste strategy tanpa memahami logic adalah risiko besar.

---

## Ringkasan untuk Pembaca Baru

Jika baru mulai, pahami urutan berikut:

```text id="p0ojub"
Crypto Asset
    ↓
Exchange
    ↓
Pair
    ↓
Order Book
    ↓
Candlestick
    ↓
Trading
    ↓
Risk Management
    ↓
Trading Bot
    ↓
Freqtrade
    ↓
Backtesting
    ↓
Dry-run
    ↓
Live dengan Risiko Kecil
```

Istilah paling penting untuk dipahami lebih dulu:

| Prioritas | Istilah         |
| --------: | --------------- |
|         1 | Exchange        |
|         2 | Pair            |
|         3 | Stablecoin      |
|         4 | Wallet          |
|         5 | API Key         |
|         6 | Spot Trading    |
|         7 | Candlestick     |
|         8 | Stoploss        |
|         9 | Position Sizing |
|        10 | Dry-run         |
|        11 | Backtesting     |
|        12 | Overfitting     |
|        13 | Freqtrade       |
|        14 | Strategy        |
|        15 | Kill Switch     |

Untuk pembaca gamer, gunakan analogi ini secara hati-hati:

| Dunia Game        | Dunia Trading               |
| ----------------- | --------------------------- |
| Market Board      | Exchange dan Order Book     |
| Item Price        | Asset Price                 |
| Build             | Strategy                    |
| Stat Balancing    | Risk Management             |
| Training Ground   | Dry-run                     |
| Damage Simulator  | Backtesting                 |
| Combat Log        | Bot Log                     |
| Patch Note        | Changelog dan Market Regime |
| Raid Preparation  | Pre-live Checklist          |
| Emergency Retreat | Kill Switch                 |

Namun, ingat perbedaan utamanya:

```text id="q70n3l"
Game menggunakan resource digital.
Crypto trading menggunakan uang sungguhan.
```

---

## Key Takeaways

* Glossary ini membantu menjaga istilah tetap konsisten di seluruh repository.
* Crypto trading memiliki banyak istilah yang terlihat sederhana, tetapi memiliki risiko teknis dan finansial.
* Trading bot bukan mesin profit, melainkan alat eksekusi strategy.
* Freqtrade harus dipahami sebagai trading lab dan execution tool.
* Risk management, dry-run, backtesting, dan operations sama pentingnya dengan entry signal.
* Pembaca gamer dapat memakai analogi game untuk memahami konsep, tetapi tidak boleh memperlakukan trading seperti farming, gacha, atau shortcut profit.

---

2026 - Kenshin Himura
