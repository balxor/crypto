# Glossary

# Glossary Crypto Trading dan Freqtrade

> Dokumen referensi istilah yang digunakan dalam seri tutorial Freqtrade. Glossary ini mencakup istilah cryptocurrency, exchange, trading, risk management, candlestick, indikator teknikal, Freqtrade, backtesting, serta server dan operations, agar penggunaannya konsisten di seluruh seri.

---

## Daftar Isi

1. [Tentang Glossary](#tentang-glossary)
2. [Cryptocurrency](#cryptocurrency)
3. [Wallet dan Security](#wallet-dan-security)
4. [Exchange dan Market](#exchange-dan-market)
5. [Trading](#trading)
6. [Risk Management](#risk-management)
7. [Candlestick dan Chart](#candlestick-dan-chart)
8. [Indikator Teknikal](#indikator-teknikal)
9. [Freqtrade](#freqtrade)
10. [Backtesting dan Strategy](#backtesting-dan-strategy)
11. [Server dan Operations](#server-dan-operations)
12. [Istilah yang Sering Disalahpahami](#istilah-yang-sering-disalahpahami)
13. [Urutan Pembelajaran untuk Pembaca Baru](#urutan-pembelajaran-untuk-pembaca-baru)

---

## Tentang Glossary

Glossary ini berfungsi sebagai referensi istilah selama mengikuti seri tutorial. Seri ini mencakup cryptocurrency, exchange, trading, candlestick, trading bot, Freqtrade, strategy, backtesting, risk management, VPS, dan server operations.

Penggunaan yang disarankan:

* Baca bagian dasar (Cryptocurrency, Exchange, Trading) sebelum masuk ke artikel strategy
* Rujuk bagian Freqtrade saat membaca konfigurasi
* Rujuk bagian Backtesting dan Strategy saat membaca hasil backtest
* Rujuk bagian Server dan Operations saat melakukan troubleshooting

Istilah yang maknanya bergantung pada konteks diberi catatan tambahan.

---

## Cryptocurrency

| Istilah            | Penjelasan                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------ |
| Cryptocurrency     | Aset digital yang menggunakan kriptografi dan berjalan di jaringan digital tertentu.             |
| Crypto Asset       | Istilah umum untuk aset digital seperti coin, token, dan stablecoin.                             |
| Blockchain         | Sistem pencatatan data yang tersusun dalam blok dan saling terhubung.                            |
| Coin               | Crypto yang berjalan di blockchain miliknya sendiri.                                             |
| Token              | Crypto yang berjalan di atas blockchain lain.                                                    |
| Native Coin        | Coin utama sebuah blockchain, misalnya ETH di Ethereum atau SOL di Solana.                       |
| Stablecoin         | Crypto yang dirancang mengikuti nilai aset acuan, umumnya dolar Amerika Serikat.                 |
| Depeg              | Kondisi ketika stablecoin tidak lagi mengikuti nilai aset acuannya.                              |
| Market Cap         | Nilai kapitalisasi pasar, dihitung dari harga dikali supply beredar.                             |
| Circulating Supply | Jumlah coin atau token yang beredar di market.                                                   |
| Total Supply       | Total coin atau token yang sudah dibuat atau tersedia.                                           |
| Max Supply         | Jumlah maksimal coin atau token yang dapat ada, jika dibatasi.                                   |
| Tokenomics         | Struktur ekonomi token: supply, distribusi, utility, dan insentif.                               |
| Utility            | Fungsi token dalam sebuah ekosistem.                                                             |
| Governance Token   | Token untuk voting atau pengambilan keputusan dalam suatu protokol.                              |
| DeFi               | Decentralized Finance, ekosistem layanan finansial berbasis blockchain.                          |
| NFT                | Non-Fungible Token, aset digital unik yang tidak dapat dipertukarkan satu banding satu.          |
| Gas Fee            | Biaya transaksi pada jaringan blockchain tertentu.                                               |
| Network Fee        | Biaya untuk memproses transaksi di blockchain.                                                   |
| Mainnet            | Jaringan utama blockchain untuk transaksi sungguhan.                                             |
| Testnet            | Jaringan percobaan untuk testing tanpa aset bernilai nyata.                                      |

> **Catatan:** Stablecoin memudahkan perpindahan dari aset volatil ke aset yang relatif stabil, tetapi tetap memiliki risiko penerbit, regulasi, likuiditas, dan depeg.

---

## Wallet dan Security

| Istilah               | Penjelasan                                                                    |
| --------------------- | ----------------------------------------------------------------------------- |
| Wallet                | Alat untuk mengakses dan mengelola aset crypto.                               |
| Address               | Alamat publik untuk menerima aset crypto.                                     |
| Private Key           | Kunci rahasia untuk mengotorisasi transaksi.                                  |
| Seed Phrase           | Rangkaian kata untuk memulihkan wallet.                                       |
| Public Key            | Komponen kriptografi yang terkait dengan address.                             |
| Hot Wallet            | Wallet yang terhubung ke internet.                                            |
| Cold Wallet           | Wallet yang disimpan offline.                                                 |
| Custodial Wallet      | Wallet yang private key-nya dikelola pihak ketiga, seperti exchange.          |
| Non-Custodial Wallet  | Wallet yang private key-nya dikendalikan sendiri oleh pengguna.               |
| 2FA                   | Two-Factor Authentication, lapisan keamanan tambahan selain password.         |
| API Key               | Kredensial untuk menghubungkan aplikasi atau bot ke exchange.                 |
| API Secret            | Secret yang digunakan bersama API key untuk autentikasi.                      |
| API Permission        | Hak akses API key, misalnya read, trade, atau withdraw.                       |
| IP Whitelist          | Pembatasan API agar hanya dapat digunakan dari IP tertentu.                   |
| Withdrawal Permission | Izin API untuk melakukan penarikan dana.                                      |
| Secret Leak           | Kebocoran API key, private key, token, atau password.                         |
| Phishing              | Upaya menipu pengguna agar menyerahkan kredensial atau secret.                |
| Malware               | Software berbahaya yang dapat mencuri data, key, atau akses akun.             |
| Backup                | Salinan data penting untuk pemulihan.                                         |
| Recovery              | Proses memulihkan akses atau data dari backup.                                |

Ketentuan keamanan dasar:

* Private key dan seed phrase tidak dibagikan kepada siapa pun
* Withdrawal permission tidak diaktifkan untuk bot
* API secret tidak disimpan di repository publik
* 2FA diaktifkan pada akun exchange
* IP whitelist digunakan jika exchange mendukung

API key dengan permission trade hanya dapat membuka dan menutup posisi. API key dengan permission withdrawal dapat memindahkan aset keluar dari akun, sehingga kebocorannya jauh lebih berbahaya. Karena itu permission withdrawal tidak diaktifkan untuk bot.

---

## Exchange dan Market

| Istilah      | Penjelasan                                                                     |
| ------------ | ------------------------------------------------------------------------------ |
| Exchange     | Platform untuk membeli dan menjual crypto.                                     |
| CEX          | Centralized Exchange, exchange yang dikelola entitas terpusat.                 |
| DEX          | Decentralized Exchange, exchange berbasis smart contract.                      |
| Pair         | Pasangan aset yang diperdagangkan, misalnya BTC/USDT.                          |
| Base Asset   | Aset utama dalam pair, misalnya BTC pada BTC/USDT.                             |
| Quote Asset  | Aset pembanding dalam pair, misalnya USDT pada BTC/USDT.                       |
| Order Book   | Daftar order beli dan jual di market.                                          |
| Bid          | Harga beli yang dipasang pembeli.                                              |
| Ask          | Harga jual yang dipasang penjual.                                             |
| Spread       | Selisih antara bid tertinggi dan ask terendah.                                 |
| Liquidity    | Kemudahan membeli atau menjual aset tanpa menggerakkan harga signifikan.       |
| Volume       | Jumlah aktivitas transaksi dalam periode tertentu.                             |
| Market Order | Order yang dieksekusi langsung pada harga market terbaik yang tersedia.        |
| Limit Order  | Order yang dieksekusi hanya pada harga tertentu atau lebih baik.               |
| Maker        | Pihak yang menambahkan likuiditas ke order book.                               |
| Taker        | Pihak yang mengambil likuiditas dari order book.                               |
| Trading Fee  | Biaya transaksi yang dikenakan exchange.                                       |
| Maker Fee    | Fee untuk order yang menambah likuiditas.                                      |
| Taker Fee    | Fee untuk order yang mengambil likuiditas.                                     |
| Slippage     | Selisih antara harga yang diharapkan dan harga eksekusi aktual.                |
| Rate Limit   | Batas jumlah request API dalam periode tertentu.                               |
| Maintenance  | Periode perawatan sistem exchange.                                             |
| Listing      | Penambahan aset baru ke exchange.                                              |
| Delisting    | Penghapusan pair atau aset dari exchange.                                      |

> **Catatan:** Market dengan volume tinggi dan spread kecil lebih sesuai untuk pemula. Market dengan likuiditas rendah memudahkan entry tetapi menyulitkan exit pada harga wajar.

---

## Trading

| Istilah          | Penjelasan                                                                       |
| ---------------- | -------------------------------------------------------------------------------- |
| Trading          | Aktivitas membeli dan menjual aset untuk mendapatkan selisih harga.              |
| Trader           | Orang atau sistem yang melakukan trading.                                        |
| Investing        | Membeli aset dengan orientasi jangka panjang.                                    |
| Spot Trading     | Membeli dan menjual aset crypto secara langsung.                                 |
| Margin Trading   | Trading menggunakan dana pinjaman untuk memperbesar posisi.                      |
| Futures Trading  | Trading kontrak derivatif berdasarkan pergerakan harga aset.                     |
| Long             | Posisi yang profit jika harga naik.                                              |
| Short            | Posisi yang profit jika harga turun.                                             |
| Leverage         | Penggunaan daya ungkit untuk memperbesar eksposur posisi.                        |
| Liquidation      | Penutupan paksa posisi karena margin tidak cukup.                                |
| Funding Fee      | Biaya periodik antar posisi long dan short pada perpetual futures.               |
| Entry            | Titik atau kondisi masuk posisi.                                                 |
| Exit             | Titik atau kondisi keluar posisi.                                                |
| Take Profit      | Target keluar ketika posisi mencapai profit tertentu.                            |
| Stoploss         | Batas keluar untuk membatasi kerugian.                                           |
| Break Even       | Kondisi posisi tidak profit dan tidak rugi setelah memperhitungkan biaya.        |
| Scalping         | Trading jangka sangat pendek dengan target kecil dan frekuensi tinggi.           |
| Day Trading      | Trading yang dibuka dan ditutup dalam hari yang sama.                            |
| Swing Trading    | Trading yang memanfaatkan pergerakan beberapa hari sampai beberapa minggu.       |
| Position Trading | Trading atau investasi jangka lebih panjang.                                     |
| Overtrading      | Frekuensi trading berlebihan tanpa dasar yang kuat.                              |
| Revenge Trading  | Trading untuk membalas kerugian sebelumnya.                                      |
| FOMO             | Fear Of Missing Out, masuk market karena takut tertinggal peluang.              |
| Panic Selling    | Menjual karena panik, bukan berdasarkan rencana.                                 |
| Market Condition | Kondisi market saat ini: bullish, bearish, atau sideways.                        |

> **Catatan:** Spot trading lebih sesuai untuk pembelajaran awal. Futures dan margin menambahkan risiko leverage, margin call, liquidation, dan funding fee.

---

## Risk Management

| Istilah              | Penjelasan                                               |
| -------------------- | -------------------------------------------------------- |
| Risk Management      | Proses mengatur risiko agar kerugian tetap terkendali.   |
| Capital              | Modal yang digunakan untuk trading.                      |
| Position Size        | Ukuran posisi dalam satu trade.                          |
| Position Sizing      | Cara menentukan ukuran posisi.                           |
| Exposure             | Total nilai posisi yang sedang terbuka atau berisiko.    |
| Max Open Trades      | Jumlah maksimal posisi aktif secara bersamaan.           |
| Risk Per Trade       | Risiko maksimal yang diterima pada satu trade.           |
| Risk Limit           | Batas risiko yang ditetapkan sebelum trading.            |
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
| Ruin Risk            | Risiko kehilangan modal dalam jumlah besar hingga sulit pulih. |

Prinsip risk management:

* Strategy dinilai dari drawdown, konsistensi, dan risiko, bukan hanya dari profit
* Satu trade tidak boleh dapat menghabiskan akun
* Modal tidak dinaikkan hanya karena satu hasil profit
* Dana kebutuhan hidup tidak digunakan untuk trading

---

## Candlestick dan Chart

| Istilah        | Penjelasan                                                           |
| -------------- | -------------------------------------------------------------------- |
| Candlestick    | Representasi visual pergerakan harga dalam periode tertentu.         |
| Candle         | Satu unit candlestick.                                               |
| Open           | Harga pembukaan candle.                                              |
| High           | Harga tertinggi candle.                                              |
| Low            | Harga terendah candle.                                               |
| Close          | Harga penutupan candle.                                              |
| OHLC           | Open, High, Low, Close.                                              |
| OHLCV          | Open, High, Low, Close, Volume.                                      |
| Body           | Bagian candle antara open dan close.                                 |
| Wick           | Garis di atas atau di bawah body candle.                             |
| Shadow         | Istilah lain untuk wick.                                             |
| Bullish Candle | Candle yang menutup lebih tinggi dari open.                          |
| Bearish Candle | Candle yang menutup lebih rendah dari open.                          |
| Timeframe      | Periode waktu pembentukan satu candle.                               |
| Trend          | Arah umum pergerakan harga.                                          |
| Uptrend        | Kondisi harga cenderung naik.                                        |
| Downtrend      | Kondisi harga cenderung turun.                                       |
| Sideways       | Kondisi harga bergerak dalam range tanpa arah yang jelas.            |
| Support        | Area harga tempat tekanan beli sebelumnya menahan penurunan.         |
| Resistance     | Area harga tempat tekanan jual sebelumnya menahan kenaikan.          |
| Breakout       | Penembusan area penting seperti support atau resistance.             |
| Pullback       | Koreksi sementara dalam trend.                                       |
| Reversal       | Perubahan arah trend.                                                |
| Retest         | Harga kembali menguji area yang baru ditembus.                       |
| Noise          | Pergerakan kecil yang tidak signifikan.                              |

> **Catatan:** Candlestick dibaca bersama trend, volume, support, resistance, dan timeframe, bukan secara terpisah.

---

## Indikator Teknikal

| Istilah            | Penjelasan                                                                                   |
| ------------------ | -------------------------------------------------------------------------------------------- |
| Indicator          | Perhitungan berbasis data market untuk membantu analisis.                                    |
| Technical Analysis | Analisis berdasarkan harga, volume, dan indikator.                                           |
| SMA                | Simple Moving Average, rata-rata harga dalam periode tertentu.                               |
| EMA                | Exponential Moving Average, moving average yang memberi bobot lebih besar pada data terbaru. |
| RSI                | Relative Strength Index, indikator momentum dengan skala 0–100.                              |
| MACD               | Moving Average Convergence Divergence, indikator momentum dan trend.                         |
| Bollinger Bands    | Indikator volatilitas berbasis moving average dan standar deviasi.                           |
| ATR                | Average True Range, indikator volatilitas.                                                   |
| Volume MA          | Rata-rata volume dalam periode tertentu.                                                     |
| Overbought         | Kondisi aset dianggap terlalu tinggi berdasarkan indikator tertentu.                         |
| Oversold           | Kondisi aset dianggap terlalu rendah berdasarkan indikator tertentu.                         |
| Crossover          | Kondisi satu garis indikator memotong garis lain.                                            |
| Signal Line        | Garis referensi dalam indikator tertentu.                                                    |
| Lagging Indicator  | Indikator yang bereaksi setelah harga bergerak.                                              |
| Leading Indicator  | Indikator yang berupaya memberi sinyal lebih awal.                                           |
| False Signal       | Sinyal yang terlihat valid tetapi tidak menghasilkan pergerakan sesuai harapan.             |
| Filter             | Syarat tambahan untuk mengurangi sinyal buruk.                                               |

> **Catatan:** Indikator membantu membaca data, bukan memprediksi harga secara pasti. Strategy yang sehat umumnya tidak bergantung pada satu indikator tunggal.

---

## Freqtrade

| Istilah              | Penjelasan                                                               |
| -------------------- | ------------------------------------------------------------------------ |
| Freqtrade            | Bot trading crypto open-source berbasis Python.                          |
| Bot                  | Program yang menjalankan aturan trading secara otomatis.                 |
| Strategy             | Class Python berisi aturan entry, exit, indikator, dan risk control.     |
| Config               | File konfigurasi bot, umumnya `config.json`.                             |
| User Data            | Folder utama berisi config, strategy, data, log, dan hasil backtest.     |
| Pairlist             | Daftar pair yang dipantau atau diperdagangkan bot.                       |
| Whitelist            | Daftar pair yang diizinkan.                                              |
| Blacklist            | Daftar pair yang dilarang.                                              |
| Dry-run              | Mode simulasi tanpa order sungguhan.                                     |
| Live Trading         | Mode trading dengan order sungguhan.                                     |
| Stake Currency       | Aset yang digunakan sebagai modal trading, misalnya USDT.                |
| Stake Amount         | Jumlah modal per trade.                                                  |
| Dry-run Wallet       | Saldo simulasi untuk mode dry-run.                                       |
| Trading Mode         | Mode trading, seperti spot atau futures.                                 |
| Timeframe            | Timeframe candle yang digunakan strategy.                                |
| Startup Candle Count | Jumlah candle awal yang dibutuhkan sebelum indikator strategy valid.     |
| Minimal ROI          | Target profit minimum berdasarkan durasi trade.                          |
| Stoploss             | Batas kerugian maksimal yang ditetapkan strategy.                        |
| Trailing Stoploss    | Stoploss yang bergerak mengikuti harga ke arah profit.                   |
| FreqUI               | Web interface untuk memantau dan mengontrol Freqtrade.                   |
| API Server           | Server lokal Freqtrade yang digunakan FreqUI dan API.                    |
| Hyperopt             | Proses optimasi parameter strategy.                                      |
| Telegram Integration | Integrasi Freqtrade dengan Telegram untuk notifikasi dan kontrol.        |
| CCXT                 | Library yang menghubungkan Freqtrade ke berbagai exchange.               |
| Docker Compose       | Tool untuk menjalankan container Freqtrade secara terstruktur.           |

> **Catatan:** Freqtrade adalah alat eksekusi strategi. Freqtrade tidak menjamin profit, tidak memprediksi pergerakan harga, dan tidak memperbaiki strategy yang buruk secara otomatis.

---

## Backtesting dan Strategy

| Istilah                | Penjelasan                                                                                   |
| ---------------------- | -------------------------------------------------------------------------------------------- |
| Backtesting            | Pengujian strategy menggunakan data historis.                                                |
| Historical Data        | Data market masa lalu yang digunakan untuk backtesting.                                      |
| Timerange              | Periode waktu yang digunakan dalam backtest.                                                 |
| Strategy Development   | Proses membuat dan memperbaiki strategy.                                                     |
| Entry Signal           | Sinyal untuk masuk posisi.                                                                   |
| Exit Signal            | Sinyal untuk keluar posisi.                                                                  |
| Enter Long             | Sinyal masuk posisi long.                                                                    |
| Exit Long              | Sinyal keluar dari posisi long.                                                              |
| DataFrame              | Struktur data tabel (pandas) untuk memproses candle dan indikator.                          |
| `populate_indicators`  | Method untuk menghitung indikator dalam strategy Freqtrade.                                  |
| `populate_entry_trend` | Method untuk mendefinisikan sinyal entry.                                                    |
| `populate_exit_trend`  | Method untuk mendefinisikan sinyal exit.                                                     |
| Hyperopt               | Proses optimasi parameter strategy.                                                          |
| Parameter              | Nilai yang mengatur perilaku strategy.                                                       |
| Optimization           | Pencarian kombinasi parameter yang lebih baik terhadap objective tertentu.                  |
| Hyperopt Loss          | Loss function sebagai objective penilaian pada hyperopt.                                     |
| Epoch                  | Satu iterasi pencarian parameter pada hyperopt.                                             |
| Overfitting            | Kondisi strategy terlalu cocok dengan data historis dan gagal pada data baru.                |
| Out-of-sample Test     | Pengujian strategy pada data yang tidak digunakan saat optimasi.                            |
| Walk-forward Testing   | Pengujian strategy secara bertahap pada periode waktu berbeda.                              |
| Trade Count            | Jumlah trade dalam backtest.                                                                 |
| Average Duration       | Rata-rata durasi trade.                                                                      |
| Exit Reason            | Alasan posisi ditutup.                                                                       |
| Pair Performance       | Performa strategy pada masing-masing pair.                                                   |

Prinsip backtesting dan strategy:

* Backtesting hanya menguji performa pada data masa lalu
* Hyperopt mencari parameter pada data tertentu dan dapat menghasilkan overfitting
* Strategy sederhana lebih mudah diaudit
* Dry-run tetap dibutuhkan setelah backtest sebelum live

---

## Server dan Operations

| Istilah           | Penjelasan                                                     |
| ----------------- | -------------------------------------------------------------- |
| VPS               | Virtual Private Server untuk menjalankan bot.                  |
| Ubuntu Server     | Sistem operasi server yang umum untuk deployment.              |
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
| HTTPS             | HTTP dengan enkripsi SSL/TLS.                                  |
| Certbot           | Tool untuk membuat dan memperbarui sertifikat Let's Encrypt.   |
| Domain            | Nama alamat website atau service.                              |
| DNS               | Sistem yang mengarahkan domain ke IP server.                   |
| Log               | Catatan aktivitas sistem atau aplikasi.                        |
| Monitoring        | Pemantauan status bot, server, dan service.                    |
| Alerting          | Notifikasi otomatis untuk kondisi penting.                     |
| Maintenance       | Perawatan sistem seperti update dan pengecekan resource.       |
| Backup            | Salinan data penting.                                          |
| Restore           | Proses mengembalikan data dari backup.                         |
| Rollback          | Pengembalian sistem ke kondisi sebelumnya.                     |
| Changelog         | Catatan perubahan versi software.                              |
| Incident Response | Prosedur menangani masalah operasional.                        |
| Kill Switch       | Prosedur menghentikan bot dengan cepat.                        |

Ketentuan operasional dasar:

* Port API bot tidak dibuka langsung ke internet
* Akses web menggunakan HTTPS
* Password kuat dan firewall aktif
* Config dan strategy di-backup
* Prosedur menghentikan bot dikuasai sebelum live
* Server diupdate secara berkala

---

## Istilah yang Sering Disalahpahami

Bagian ini meluruskan pemahaman keliru yang umum terjadi pada pemula.

**Bot trading.** Bot trading bukan mesin profit otomatis, melainkan alat untuk menjalankan aturan trading secara otomatis. Bot tidak menciptakan edge; edge berasal dari strategy, risk management, dan disiplin operasional.

**Backtesting.** Hasil backtest yang profitable tidak menjamin profit pada live trading. Backtest hanya menunjukkan performa strategy pada data historis, dan harus dilanjutkan dengan validasi, dry-run, serta live kecil bila sudah siap.

**Dry-run.** Dry-run bukan representasi penuh dari live trading. Dry-run membantu mengamati perilaku bot, tetapi tidak selalu menangkap slippage, latency, order fill, dan tekanan psikologis penggunaan dana real.

**Winrate.** Winrate tinggi tidak otomatis berarti strategy baik. Winrate dibaca bersama average profit, average loss, drawdown, dan risk reward ratio. Strategy dengan winrate tinggi tetap dapat merugi jika satu loss besar tidak terkontrol.

**Stoploss.** Stoploss bukan tanda strategy gagal, melainkan mekanisme pembatasan kerugian yang menjaga modal tetap tersedia untuk peluang berikutnya.

**Futures.** Kemampuan futures untuk profit di kedua arah harga disertai risiko leverage, liquidation, funding fee, dan margin management. Futures tidak sesuai sebagai area belajar pertama bagi pemula.

**Hyperopt.** Hyperopt mencari parameter yang baik pada data dan objective tertentu, tetapi hasilnya dapat overfitting. Parameter hasil hyperopt divalidasi pada data lain dan dry-run.

**Strategy komunitas.** Strategy publik yang banyak digunakan tidak otomatis aman. Strategy komunitas perlu diaudit, dibacktest ulang, dan dipahami sebelum digunakan. Menjalankan strategy tanpa memahami logikanya adalah risiko yang besar.

---

## Urutan Pembelajaran untuk Pembaca Baru

Urutan konsep yang disarankan untuk dipelajari:

```text
Crypto Asset → Exchange → Pair → Order Book → Candlestick → Trading
→ Risk Management → Trading Bot → Freqtrade → Backtesting → Dry-run
→ Live dengan Risiko Kecil
```

Prioritas istilah yang dipahami lebih dahulu:

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

---

2026 - Kenshin Himura
