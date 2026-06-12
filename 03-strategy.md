# Artikel 3 — Strategy

# Pengembangan Strategy Freqtrade: Indikator, Entry/Exit, dan Backtesting

> Artikel ketiga dari seri tutorial Freqtrade. Artikel ini membahas data candle dan DataFrame, indikator teknikal dasar, struktur strategy Freqtrade, tiga contoh strategy bertingkat, konfigurasi ROI dan stoploss, backtesting beserta cara membaca hasilnya, hyperparameter optimization, overfitting, dan penggunaan strategy komunitas sebagai referensi.

---

## Daftar Isi

1. [Pendahuluan](#pendahuluan)
2. [Konsep Dasar Strategy](#konsep-dasar-strategy)
3. [Data Input Strategy](#data-input-strategy)
   * [Candlestick dan Timeframe](#candlestick-dan-timeframe)
   * [DataFrame](#dataframe)
   * [OHLCV](#ohlcv)
4. [Indikator Teknikal](#indikator-teknikal)
   * [SMA](#sma)
   * [EMA](#ema)
   * [RSI](#rsi)
   * [Volume](#volume)
5. [Struktur Strategy Freqtrade](#struktur-strategy-freqtrade)
6. [Membuat Strategy Pertama](#membuat-strategy-pertama)
7. [Contoh Strategy 1: SMA Crossover](#contoh-strategy-1-sma-crossover)
8. [Contoh Strategy 2: RSI Basic](#contoh-strategy-2-rsi-basic)
9. [Contoh Strategy 3: Trend, RSI, dan Volume](#contoh-strategy-3-trend-rsi-dan-volume)
10. [ROI, Stoploss, dan Trailing Stoploss](#roi-stoploss-dan-trailing-stoploss)
11. [Backtesting](#backtesting)
    * [Download Data Historis](#download-data-historis)
    * [Menjalankan Backtest](#menjalankan-backtest)
    * [Membaca Hasil Backtest](#membaca-hasil-backtest)
12. [Iterasi Strategy](#iterasi-strategy)
13. [Hyperparameter Optimization](#hyperparameter-optimization)
14. [Overfitting](#overfitting)
15. [Strategy Komunitas sebagai Referensi](#strategy-komunitas-sebagai-referensi)
16. [Checklist Sebelum Lanjut ke Artikel 4](#checklist-sebelum-lanjut-ke-artikel-4)
17. [Glossary](#glossary)
18. [Ringkasan](#ringkasan)
19. [Disclaimer](#disclaimer)

---

## Pendahuluan

Artikel ini adalah lanjutan dari Artikel 2 — Setup. Artikel sebelumnya membahas pembangunan environment Freqtrade; artikel ini membahas penulisan dan evaluasi strategy.

### Prasyarat

Pembaca diasumsikan sudah memahami:

* Konsep dasar cryptocurrency, exchange, pair, dan API key (Artikel 1)
* Perbedaan dry-run dan live trading (Artikel 1)
* Cara menjalankan Freqtrade di VPS dengan Docker (Artikel 2)
* Cara membaca log dan mengakses FreqUI (Artikel 2)

### Tujuan

Setelah menyelesaikan artikel ini, pembaca diharapkan dapat:

* Memahami struktur data candle dan DataFrame yang digunakan Freqtrade
* Menambahkan indikator teknikal ke dalam strategy
* Menulis aturan entry dan exit
* Mengonfigurasi ROI, stoploss, dan trailing stoploss
* Menjalankan backtesting dan membaca hasilnya secara kritis
* Memahami fungsi dan risiko hyperopt
* Mengenali tanda-tanda overfitting
* Menggunakan strategy komunitas sebagai referensi dengan prosedur audit

---

## Konsep Dasar Strategy

Strategy adalah sekumpulan aturan yang menjawab pertanyaan berikut:

```text
Kapan bot boleh masuk posisi?
Kapan bot harus keluar posisi?
Berapa risiko yang boleh diterima?
Berapa target profit yang masuk akal?
Kapan kondisi market dianggap tidak valid?
```

Strategy tidak memprediksi pergerakan harga. Strategy hanya membuat keputusan menjadi konsisten berdasarkan aturan yang ditetapkan terhadap data yang tersedia. Contoh:

```text
Jika trend naik dan momentum membaik, bot boleh mencari entry.
Jika harga bergerak melawan posisi, bot keluar berdasarkan stoploss.
Jika target profit tercapai, bot keluar berdasarkan ROI.
```

Aturan tersebut tidak menjamin harga naik setelah entry. Fungsinya adalah menghilangkan keputusan impulsif yang umum terjadi pada trading manual: masuk karena takut tertinggal (FOMO), menunda cut loss, membuka posisi balas dendam setelah loss, atau mengandalkan rumor dan satu indikator tunggal.

Kriteria strategy yang layak dikembangkan:

* Kerugian per trade terkendali melalui stoploss
* Ukuran posisi proporsional terhadap modal
* Profit tidak bergantung pada satu trade besar
* Strategy tidak hancur karena beberapa loss beruntun
* Hasil backtest dibaca secara kritis, bukan hanya dari total profit
* Strategy diuji di dry-run sebelum live

Loss adalah bagian normal dari sistem trading. Strategy yang baik bukan strategy yang selalu profit, melainkan strategy yang memiliki aturan jelas untuk kedua kondisi: ketika analisis benar dan ketika analisis salah.

Setiap strategy juga memiliki konteks market. Strategy yang profitable pada kondisi trending dapat merugi pada kondisi sideways; strategy yang terlihat baik pada market bullish dapat gagal pada market bearish. Tidak ada strategy yang profitable di seluruh kondisi market. Tujuan pengembangan strategy adalah menghasilkan strategy yang dapat dipahami, diuji, dan dikontrol risikonya — bukan strategy yang sempurna.

---

## Data Input Strategy

### Candlestick dan Timeframe

Freqtrade bekerja menggunakan data candle. Setiap candle merepresentasikan pergerakan harga dalam satu periode waktu.

| Timeframe | Periode per candle |
| --------- | ------------------ |
| `1m`      | 1 menit            |
| `5m`      | 5 menit            |
| `15m`     | 15 menit           |
| `1h`      | 1 jam              |
| `4h`      | 4 jam              |
| `1d`      | 1 hari             |

Timeframe menentukan karakter strategy:

| Karakteristik          | Timeframe kecil (mis. `5m`) | Timeframe besar (mis. `1h`) |
| ---------------------- | --------------------------- | --------------------------- |
| Frekuensi sinyal       | Sering                      | Jarang                      |
| Sensitivitas noise     | Tinggi                      | Rendah                      |
| Dampak fee terhadap hasil | Besar                    | Lebih kecil                 |
| Kecepatan reaksi       | Cepat                       | Lambat                      |
| Kemudahan analisis     | Lebih sulit untuk pemula    | Lebih mudah untuk pemula    |

Tidak ada timeframe yang selalu paling baik. Pemilihan timeframe disesuaikan dengan karakter strategy dan kapasitas monitoring pengguna.

### DataFrame

Freqtrade memproses data candle dalam bentuk DataFrame (pandas), yaitu struktur data berbentuk tabel. Setiap baris merepresentasikan satu candle; setiap kolom berisi data harga, volume, atau indikator.

Data mentah dari exchange:

| Date             | Open | High | Low | Close | Volume |
| ---------------- | ---: | ---: | --: | ----: | -----: |
| 2026-01-01 00:00 |  100 |  105 |  98 |   103 |   1200 |
| 2026-01-01 00:05 |  103 |  108 | 101 |   107 |   1500 |
| 2026-01-01 00:10 |  107 |  109 | 104 |   105 |   1300 |

Strategy menambahkan kolom indikator ke DataFrame:

| Date             | Close | Volume | SMA20 | SMA50 | RSI |
| ---------------- | ----: | -----: | ----: | ----: | --: |
| 2026-01-01 00:00 |   103 |   1200 |   101 |    98 |  55 |
| 2026-01-01 00:05 |   107 |   1500 |   102 |    99 |  61 |
| 2026-01-01 00:10 |   105 |   1300 |   103 |   100 |  58 |

Kolom indikator inilah yang dievaluasi oleh aturan entry dan exit. Contoh logika:

```text
Jika SMA20 memotong ke atas SMA50, trend jangka pendek menguat.
Jika RSI di bawah ambang tertentu, aset berada dalam kondisi oversold.
Jika volume di atas rata-rata, sinyal dianggap lebih valid.
```

### OHLCV

OHLCV adalah format data dasar yang digunakan dalam seluruh proses:

| Komponen | Arti                                    |
| -------- | --------------------------------------- |
| Open     | Harga pembukaan candle                  |
| High     | Harga tertinggi candle                  |
| Low      | Harga terendah candle                   |
| Close    | Harga penutupan candle                  |
| Volume   | Jumlah aktivitas transaksi dalam candle |

Sebagian besar indikator pada strategy sederhana dihitung dari harga close:

* SMA menggunakan rata-rata close
* EMA menggunakan close dengan bobot lebih besar pada data terbaru
* RSI menggunakan perubahan harga untuk mengukur momentum
* Volume digunakan untuk mengukur kekuatan pergerakan

Alur pemrosesannya: OHLCV adalah data mentah, indikator adalah hasil perhitungan dari data tersebut, dan entry/exit adalah keputusan berdasarkan aturan terhadap indikator.

---

## Indikator Teknikal

Indikator teknikal adalah perhitungan matematis berdasarkan data harga dan volume. Indikator digunakan untuk membaca trend, momentum, volatilitas, volume, kondisi overbought/oversold, serta potensi reversal atau continuation.

Indikator menyederhanakan informasi dari data market; indikator tidak memberikan kepastian. Kesalahan umum pemula adalah memperlakukan indikator sebagai sinyal pasti. Contoh interpretasi yang keliru:

```text
RSI di bawah 30 berarti harga pasti naik.
```

Interpretasi yang tepat:

```text
RSI di bawah 30 menunjukkan kondisi oversold berdasarkan perhitungan RSI,
tetapi harga masih dapat turun lebih jauh.
```

Dalam strategy, indikator digunakan bersama konteks lain: trend filter, volume filter, struktur market, stoploss, pemilihan pair, dan timeframe.

### SMA

SMA (Simple Moving Average) menghitung rata-rata harga dalam periode tertentu:

```text
SMA20 = rata-rata close 20 candle terakhir
SMA50 = rata-rata close 50 candle terakhir
```

SMA digunakan untuk membaca trend. SMA pendek di atas SMA panjang mengindikasikan trend jangka pendek yang menguat; SMA pendek di bawah SMA panjang mengindikasikan trend jangka pendek yang melemah.

### EMA

EMA (Exponential Moving Average) serupa dengan SMA, tetapi memberikan bobot lebih besar pada data terbaru. EMA bereaksi lebih cepat terhadap perubahan harga dibanding SMA, dengan konsekuensi lebih sensitif terhadap noise.

### RSI

RSI (Relative Strength Index) mengukur momentum dengan rentang nilai 0–100.

|         RSI | Interpretasi umum |
| ----------: | ----------------- |
| Di bawah 30 | Oversold          |
|  Sekitar 50 | Netral            |
|  Di atas 70 | Overbought        |

Pada trend yang kuat, RSI dapat bertahan lama di area overbought atau oversold sementara harga terus bergerak searah trend. Karena itu RSI digunakan bersama filter lain, bukan sebagai sinyal tunggal.

### Volume

Volume mengukur aktivitas transaksi dan digunakan untuk menilai kualitas pergerakan harga:

* Harga naik dengan volume naik mengindikasikan momentum yang lebih kuat
* Harga naik dengan volume lemah mengindikasikan kenaikan yang rapuh
* Harga turun dengan volume besar mengindikasikan tekanan jual yang kuat

Volume lebih tepat digunakan sebagai filter konfirmasi, bukan sebagai sinyal entry tunggal.

---

## Struktur Strategy Freqtrade

Strategy Freqtrade ditulis sebagai class Python yang mewarisi `IStrategy`, disimpan di `user_data/strategies/`.

Struktur dasar:

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame


class MyFirstStrategy(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"

    minimal_roi = {
        "0": 0.03
    }

    stoploss = -0.10

    startup_candle_count = 50

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        return dataframe
```

Tiga method utama yang diimplementasikan setiap strategy:

| Method                 | Fungsi                                                  |
| ---------------------- | ------------------------------------------------------- |
| `populate_indicators`  | Menghitung indikator dan menambahkan kolom ke DataFrame |
| `populate_entry_trend` | Mendefinisikan kondisi entry                            |
| `populate_exit_trend`  | Mendefinisikan kondisi exit                             |

Atribut class yang umum dikonfigurasi:

| Atribut                | Fungsi                                                        |
| ---------------------- | ------------------------------------------------------------- |
| `timeframe`            | Timeframe candle yang digunakan strategy                      |
| `minimal_roi`          | Target profit berdasarkan durasi trade                        |
| `stoploss`             | Batas kerugian maksimal per trade                             |
| `startup_candle_count` | Jumlah candle yang dibutuhkan sebelum indikator valid         |

`startup_candle_count` harus lebih besar atau sama dengan periode indikator terpanjang yang digunakan. Strategy dengan EMA200, misalnya, memerlukan nilai minimal 200 agar indikator terhitung penuh sejak awal.

---

## Membuat Strategy Pertama

Generate strategy baru dari template:

```bash
cd ~/freqtrade
docker compose run --rm freqtrade new-strategy --strategy SmaCrossStrategy
```

File strategy dibuat di:

```text
user_data/strategies/SmaCrossStrategy.py
```

Buka file untuk diedit:

```bash
nano user_data/strategies/SmaCrossStrategy.py
```

Strategy pertama sebaiknya sangat sederhana, dengan sedikit indikator. Tujuan tahap ini adalah memahami workflow, bukan menghasilkan strategy profitable:

* Memahami struktur file strategy
* Menambahkan indikator
* Menulis aturan entry dan exit
* Menjalankan backtest
* Membaca hasil backtest

Tiga contoh strategy berikut disusun bertingkat dari yang paling sederhana. Ketiganya adalah materi pembelajaran, bukan strategy siap live.

---

## Contoh Strategy 1: SMA Crossover

Logika:

```text
Entry : SMA pendek memotong ke atas SMA panjang.
Exit  : SMA pendek memotong ke bawah SMA panjang.
```

Implementasi:

```python
from freqtrade.strategy import IStrategy
from freqtrade.vendor.qtpylib import indicators as qtpylib
from pandas import DataFrame
import talib.abstract as ta


class SmaCrossStrategy(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.03,
        "60": 0.015,
        "120": 0.005
    }

    stoploss = -0.08

    startup_candle_count = 60

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["sma_fast"] = ta.SMA(dataframe, timeperiod=20)
        dataframe["sma_slow"] = ta.SMA(dataframe, timeperiod=50)

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                qtpylib.crossed_above(dataframe["sma_fast"], dataframe["sma_slow"])
                & (dataframe["volume"] > 0)
            ),
            "enter_long"
        ] = 1

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                qtpylib.crossed_below(dataframe["sma_fast"], dataframe["sma_slow"])
                & (dataframe["volume"] > 0)
            ),
            "exit_long"
        ] = 1

        return dataframe
```

Komponen:

| Bagian          | Fungsi                                               |
| --------------- | ---------------------------------------------------- |
| `sma_fast`      | Moving average periode pendek (20)                   |
| `sma_slow`      | Moving average periode panjang (50)                  |
| `crossed_above` | Sinyal saat SMA cepat memotong ke atas SMA lambat    |
| `crossed_below` | Sinyal saat SMA cepat memotong ke bawah SMA lambat   |
| `volume > 0`    | Filter validitas candle                              |
| `enter_long`    | Kolom sinyal entry long                              |
| `exit_long`     | Kolom sinyal exit long                               |

Karakteristik:

* **Kelebihan:** mudah dipahami, sesuai untuk mempelajari struktur strategy dan konsep trend.
* **Kelemahan:** entry sering terlambat karena moving average bersifat lagging, menghasilkan banyak false signal pada market sideways, dan memerlukan filter tambahan sebelum layak diuji lebih lanjut.

---

## Contoh Strategy 2: RSI Basic

Logika:

```text
Entry : RSI turun di bawah 30 (oversold).
Exit  : RSI naik di atas 70 (overbought).
```

Implementasi:

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta


class RsiBasicStrategy(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.025,
        "60": 0.01,
        "120": 0.005
    }

    stoploss = -0.07

    startup_candle_count = 30

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["rsi"] = ta.RSI(dataframe, timeperiod=14)

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (dataframe["rsi"] < 30)
                & (dataframe["volume"] > 0)
            ),
            "enter_long"
        ] = 1

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (dataframe["rsi"] > 70)
                & (dataframe["volume"] > 0)
            ),
            "exit_long"
        ] = 1

        return dataframe
```

Karakteristik:

* **Kelebihan:** mudah dipahami, sesuai untuk mempelajari konsep momentum dan backtesting awal.
* **Kelemahan:** RSI oversold tidak menjamin harga naik dan RSI overbought tidak menjamin harga turun. Pada downtrend yang kuat, RSI dapat bertahan di area rendah sementara harga terus turun, sehingga strategy ini membuka posisi melawan trend berulang kali. Strategy ini memerlukan trend filter sebelum dapat dievaluasi secara wajar.

---

## Contoh Strategy 3: Trend, RSI, dan Volume

Strategy ketiga menggabungkan tiga filter untuk menghindari entry melawan trend utama.

Logika:

```text
Entry : Harga di atas EMA200,
        RSI di bawah 40,
        volume di atas rata-rata volume 20 candle.

Exit  : RSI naik di atas 65, atau harga turun di bawah EMA200.
```

Implementasi:

```python
from freqtrade.strategy import IStrategy
from pandas import DataFrame
import talib.abstract as ta


class TrendRsiVolumeStrategy(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    minimal_roi = {
        "0": 0.03,
        "45": 0.015,
        "120": 0.005
    }

    stoploss = -0.06

    trailing_stop = True
    trailing_stop_positive = 0.01
    trailing_stop_positive_offset = 0.02
    trailing_only_offset_is_reached = True

    startup_candle_count = 220

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["ema_200"] = ta.EMA(dataframe, timeperiod=200)
        dataframe["rsi"] = ta.RSI(dataframe, timeperiod=14)
        dataframe["volume_mean_20"] = dataframe["volume"].rolling(20).mean()

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (dataframe["close"] > dataframe["ema_200"])
                & (dataframe["rsi"] < 40)
                & (dataframe["volume"] > dataframe["volume_mean_20"])
                & (dataframe["volume"] > 0)
            ),
            "enter_long"
        ] = 1

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (
                    (dataframe["rsi"] > 65)
                    | (dataframe["close"] < dataframe["ema_200"])
                )
                & (dataframe["volume"] > 0)
            ),
            "exit_long"
        ] = 1

        return dataframe
```

Komponen:

| Komponen                  | Fungsi                                            |
| ------------------------- | ------------------------------------------------- |
| `ema_200`                 | Filter trend utama                                |
| `close > ema_200`         | Entry hanya saat harga di atas trend utama        |
| `rsi < 40`                | Mencari pullback atau momentum rendah dalam uptrend |
| `volume > volume_mean_20` | Konfirmasi aktivitas market                       |
| `rsi > 65`                | Exit saat momentum tinggi                         |
| `close < ema_200`         | Exit saat trend filter tidak lagi valid           |

Perhatikan `startup_candle_count = 220`: nilai ini melebihi periode EMA200 agar indikator terhitung penuh sebelum strategy mengevaluasi sinyal.

Karakteristik:

* **Kelebihan:** memiliki trend filter, momentum filter, dan volume filter, sehingga setiap entry dapat dijelaskan secara logis.
* **Kelemahan:** entry dapat terlambat, strategy dapat melewatkan pergerakan cepat, parameter belum dioptimasi, tetap dapat merugi pada market choppy, dan harus diuji pada beberapa periode market sebelum dievaluasi lebih lanjut.

---

## ROI, Stoploss, dan Trailing Stoploss

Strategy tidak hanya terdiri dari aturan entry. Aturan exit dan risk control menentukan hasil akhir, dan untuk pemula komponen ini lebih menentukan daripada kualitas entry.

### Minimal ROI

`minimal_roi` menentukan target profit berdasarkan durasi trade:

```python
minimal_roi = {
    "0": 0.03,
    "60": 0.015,
    "120": 0.005
}
```

| Durasi trade      | Target ROI |
| ----------------- | ---------: |
| Sejak menit 0     |       3.0% |
| Setelah 60 menit  |       1.5% |
| Setelah 120 menit |       0.5% |

Target profit menurun seiring durasi posisi. Mekanisme ini mengeluarkan bot dari posisi yang tidak bergerak sesuai harapan, sehingga modal tidak tertahan terlalu lama pada trade yang stagnan.

### Stoploss

`stoploss` menentukan batas kerugian maksimal per trade:

```python
stoploss = -0.08
```

Nilai `-0.08` berarti posisi ditutup pada kerugian sekitar 8%. Stoploss adalah mekanisme pembatasan kerugian, bukan indikasi strategy yang buruk; tanpa stoploss, bot menahan posisi rugi tanpa batas dan satu posisi dapat menghapus akumulasi profit dari banyak trade.

### Trailing Stoploss

Trailing stoploss adalah stoploss yang bergerak mengikuti harga ke arah profit:

```python
trailing_stop = True
trailing_stop_positive = 0.01
trailing_stop_positive_offset = 0.02
trailing_only_offset_is_reached = True
```

Cara kerja konfigurasi di atas:

* Trailing stop baru aktif setelah profit mencapai offset 2% (`trailing_stop_positive_offset` dengan `trailing_only_offset_is_reached = True`)
* Setelah aktif, stoploss mengikuti harga pada jarak 1% di bawah harga tertinggi (`trailing_stop_positive`)
* Jika harga berbalik, posisi ditutup dengan sebagian profit terkunci

Parameter trailing yang terlalu ketat menutup posisi terlalu cepat; parameter yang terlalu longgar mengembalikan profit yang sudah terbentuk. Nilai yang tepat ditentukan melalui backtest dan diverifikasi di dry-run.

---

## Backtesting

Backtesting menguji strategy terhadap data historis untuk melihat perilakunya pada periode market sebelumnya. Backtesting tidak menjamin hasil masa depan, tetapi menjawab pertanyaan-pertanyaan berikut:

* Apakah strategy menghasilkan sinyal?
* Apakah frekuensi trading terlalu tinggi atau terlalu rendah?
* Seberapa sering stoploss terpicu?
* Apakah profit terdistribusi atau bergantung pada satu trade besar?
* Seberapa dalam drawdown?
* Apakah strategy hanya profitable pada kondisi market tertentu?
* Apakah strategy tetap masuk akal setelah fee diperhitungkan?

### Download Data Historis

Data historis diunduh sebelum backtest:

```bash
cd ~/freqtrade

docker compose run --rm freqtrade download-data \
  --exchange binance \
  --pairs BTC/USDT ETH/USDT \
  --timeframes 5m \
  --timerange 20240101-20240601
```

Sesuaikan exchange, pair, timeframe, dan timerange dengan kebutuhan pengujian.

### Menjalankan Backtest

```bash
docker compose run --rm freqtrade backtesting \
  --config user_data/config.json \
  --strategy SmaCrossStrategy \
  --timerange 20240101-20240601
```

Backtest strategy lain dilakukan dengan mengganti nilai `--strategy`:

```bash
docker compose run --rm freqtrade backtesting \
  --config user_data/config.json \
  --strategy RsiBasicStrategy \
  --timerange 20240101-20240601
```

```bash
docker compose run --rm freqtrade backtesting \
  --config user_data/config.json \
  --strategy TrendRsiVolumeStrategy \
  --timerange 20240101-20240601
```

Output backtest berisi metrik: total trades, profit, winrate, average duration, drawdown, dan detail per trade.

### Membaca Hasil Backtest

Total profit adalah metrik yang paling menonjol pada output, tetapi bukan metrik yang paling informatif. Evaluasi dilakukan terhadap seluruh metrik berikut:

**Total profit.** Hasil akhir strategy pada periode backtest. Total profit dapat menyesatkan jika sebagian besar profit berasal dari satu trade besar sementara mayoritas trade lain merugi.

**Total trades.** Jumlah transaksi. Jumlah trade yang terlalu sedikit membuat hasil tidak signifikan secara statistik — profit 20% dari 3 trade tidak cukup untuk menyimpulkan kualitas strategy. Jumlah trade yang sangat banyak juga perlu diperiksa karena fee dan slippage berdampak lebih besar pada kondisi live.

**Winrate.** Persentase trade yang profit. Winrate tinggi tidak menjamin strategy profitable: satu loss besar dapat menghapus banyak profit kecil. Sebaliknya, strategy dengan winrate rendah dapat profitable jika rata-rata profit per winning trade jauh lebih besar daripada rata-rata loss.

**Average profit.** Rata-rata profit per trade. Metrik ini dibandingkan dengan estimasi fee, spread, dan slippage; average profit yang lebih kecil dari total biaya transaksi berarti strategy merugi pada kondisi live.

**Drawdown.** Penurunan nilai akun dari titik tertinggi. Strategy dengan profit tinggi tetapi drawdown dalam berisiko tidak dapat dipertahankan: pada kondisi live, drawdown besar mendorong keputusan menghentikan bot di waktu yang salah.

**Best trade dan worst trade.** Jika profit strategy bergantung pada satu trade terbaik, strategy tergolong rapuh. Jika satu trade terburuk berukuran tidak proporsional, risk control perlu diperbaiki.

**Pair performance.** Performa per pair. Strategy yang baik di BTC/USDT belum tentu baik di pair lain; kesimpulan tidak diambil dari satu pair.

**Exit reason.** Distribusi alasan penutupan posisi:

| Exit reason       | Arti                          |
| ----------------- | ----------------------------- |
| ROI               | Target ROI tercapai           |
| Stoploss          | Batas kerugian terpicu        |
| Exit signal       | Sinyal exit strategy          |
| Trailing stoploss | Trailing stop terpicu         |
| Force exit        | Posisi ditutup paksa          |

Dominasi exit melalui stoploss mengindikasikan kualitas entry yang buruk atau stoploss yang terlalu ketat. Dominasi posisi yang keluar terlalu cepat mengindikasikan konfigurasi ROI atau trailing stop yang perlu ditinjau.

---

## Iterasi Strategy

Pengembangan strategy adalah proses iteratif. Alur kerja yang digunakan:

```text
1. Tulis strategy sederhana
2. Jalankan backtest
3. Baca seluruh metrik hasil
4. Identifikasi masalah
5. Ubah satu komponen
6. Backtest ulang
7. Bandingkan hasil
8. Uji pada periode lain
9. Jalankan dry-run
10. Evaluasi ulang
```

Prinsip iterasi: ubah satu variabel per siklus. Mengubah banyak parameter sekaligus menghilangkan kemampuan mengidentifikasi komponen mana yang memperbaiki atau memperburuk hasil.

Contoh iterasi yang terkontrol:

1. Ubah periode SMA dari 20/50 ke 10/50
2. Jalankan backtest ulang
3. Bandingkan total trades, profit, drawdown, dan exit reason
4. Catat hasil perbandingan
5. Lanjutkan ke perubahan komponen berikutnya

Contoh iterasi yang tidak terkontrol: mengubah timeframe, periode SMA, parameter RSI, stoploss, ROI, dan pair secara bersamaan, lalu menyimpulkan strategy membaik tanpa catatan pembanding. Hasil dari iterasi semacam ini tidak dapat dianalisis.

---

## Hyperparameter Optimization

Hyperparameter optimization (hyperopt) mencari kombinasi parameter strategy yang optimal terhadap objective tertentu. Parameter yang umum dioptimasi: periode dan ambang RSI, periode moving average, stoploss, ROI, trailing stoploss, serta parameter buy/sell lainnya.

Hyperopt adalah alat eksplorasi parameter. Penggunaan yang terlalu agresif pada data yang sama menghasilkan parameter yang terlalu cocok dengan data masa lalu (overfitting, dibahas pada bagian berikutnya).

### Parameter yang Dapat Dioptimasi

Parameter didefinisikan menggunakan class parameter Freqtrade seperti `IntParameter`:

```python
from freqtrade.strategy import IStrategy, IntParameter
from pandas import DataFrame
import talib.abstract as ta


class RsiHyperoptStrategy(IStrategy):
    INTERFACE_VERSION = 3

    timeframe = "5m"
    can_short = False

    buy_rsi = IntParameter(20, 40, default=30, space="buy")
    sell_rsi = IntParameter(60, 80, default=70, space="sell")

    minimal_roi = {
        "0": 0.03
    }

    stoploss = -0.08

    startup_candle_count = 30

    def populate_indicators(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe["rsi"] = ta.RSI(dataframe, timeperiod=14)

        return dataframe

    def populate_entry_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (dataframe["rsi"] < self.buy_rsi.value)
                & (dataframe["volume"] > 0)
            ),
            "enter_long"
        ] = 1

        return dataframe

    def populate_exit_trend(self, dataframe: DataFrame, metadata: dict) -> DataFrame:
        dataframe.loc[
            (
                (dataframe["rsi"] > self.sell_rsi.value)
                & (dataframe["volume"] > 0)
            ),
            "exit_long"
        ] = 1

        return dataframe
```

`IntParameter(20, 40, default=30, space="buy")` mendefinisikan parameter integer dengan rentang pencarian 20–40, nilai default 30, pada space `buy`.

### Menjalankan Hyperopt

```bash
docker compose run --rm freqtrade hyperopt \
  --config user_data/config.json \
  --strategy RsiHyperoptStrategy \
  --hyperopt-loss SharpeHyperOptLoss \
  --spaces buy sell \
  --timerange 20240101-20240601 \
  -e 100
```

| Parameter         | Fungsi                                           |
| ----------------- | ------------------------------------------------ |
| `--strategy`      | Strategy yang dioptimasi                         |
| `--hyperopt-loss` | Loss function sebagai objective penilaian hasil  |
| `--spaces`        | Space parameter yang dioptimasi                  |
| `--timerange`     | Periode data historis                            |
| `-e`              | Jumlah epoch (iterasi pencarian)                 |

Parameter hasil hyperopt yang baik pada data historis tetap divalidasi pada periode data lain dan dry-run sebelum digunakan.

---

## Overfitting

Overfitting terjadi ketika strategy terlalu cocok dengan data historis tertentu dan gagal pada data baru. Strategy yang overfit menunjukkan hasil backtest yang sangat baik pada periode pengujian, tetapi performanya runtuh saat kondisi market berbeda dari data pengujian.

Indikasi overfitting:

* Profit backtest sangat tinggi tetapi hanya pada satu periode
* Jumlah trade terlalu sedikit
* Jumlah parameter terlalu banyak relatif terhadap kompleksitas logika
* Hasil buruk saat diuji pada periode atau pair lain
* Profit bergantung pada satu trade besar
* Drawdown tidak stabil antar periode pengujian
* Hyperopt dijalankan berulang kali pada data yang sama
* Logika strategy tidak dapat dijelaskan, tetapi hasil backtest baik

Langkah mitigasi:

* Mulai dari strategy sederhana dengan sedikit parameter
* Pisahkan data menjadi periode training dan testing
* Uji pada beberapa periode market: bullish, bearish, dan sideways
* Uji pada beberapa pair likuid
* Jangan menjadikan profit backtest tertinggi sebagai satu-satunya target
* Evaluasi drawdown dan jumlah trade, bukan hanya profit
* Jalankan dry-run setelah backtest
* Catat setiap perubahan strategy beserta hasilnya

Contoh pembagian data:

| Periode              | Fungsi             |
| -------------------- | ------------------ |
| Januari sampai Maret | Eksperimen awal    |
| April sampai Mei     | Validasi           |
| Juni                 | Out-of-sample test |

Strategy yang hanya baik pada periode eksperimen tetapi buruk pada periode validasi tidak diteruskan ke tahap berikutnya. Overfitting adalah penyebab kegagalan paling umum pada bot trading: hasil backtest yang baik bukan bukti strategy siap live.

---

## Strategy Komunitas sebagai Referensi

Banyak strategy Freqtrade tersedia di repository komunitas. Strategy komunitas berguna sebagai bahan belajar untuk memahami struktur code, penggunaan indikator, pola entry/exit, penggunaan parameter, serta konfigurasi stoploss, ROI, dan filter.

Strategy komunitas tidak digunakan secara langsung tanpa audit. Praktik yang harus dihindari:

* Copy-paste langsung ke mode live
* Menjalankan strategy tanpa membaca code
* Tidak melakukan backtest ulang dengan data dan konfigurasi sendiri
* Tidak menjalankan dry-run
* Tidak memahami pair dan timeframe yang menjadi asumsi strategy
* Tidak memahami logika stoploss dan exit

Checklist audit sebelum menggunakan strategy komunitas:

| Checklist                              | Status |
| -------------------------------------- | ------ |
| Code strategy sudah dibaca seluruhnya  | Wajib  |
| Entry logic dipahami                   | Wajib  |
| Exit logic dipahami                    | Wajib  |
| Stoploss dipahami                      | Wajib  |
| ROI dipahami                           | Wajib  |
| Timeframe dipahami                     | Wajib  |
| Pair yang sesuai dipahami              | Wajib  |
| Backtest ulang dilakukan sendiri       | Wajib  |
| Dry-run dilakukan                      | Wajib  |
| Tidak ada secret di file strategy      | Wajib  |

Hasil strategy bergantung pada kondisi market, exchange, fee, pair, dan timeframe yang digunakan. Strategy publik yang profitable bagi penulisnya tidak otomatis profitable bagi pengguna lain dengan konfigurasi berbeda.

---

## Checklist Sebelum Lanjut ke Artikel 4

| Checklist                                       | Status             |
| ------------------------------------------------ | ------------------ |
| Memahami data OHLCV                              | Wajib              |
| Memahami DataFrame Freqtrade secara dasar        | Wajib              |
| Memahami fungsi `populate_indicators`            | Wajib              |
| Memahami fungsi `populate_entry_trend`           | Wajib              |
| Memahami fungsi `populate_exit_trend`            | Wajib              |
| Dapat menambahkan indikator ke strategy          | Wajib              |
| Memahami entry signal dan exit signal            | Wajib              |
| Memahami minimal ROI                             | Wajib              |
| Memahami stoploss                                | Wajib              |
| Memahami trailing stoploss                       | Wajib              |
| Dapat menjalankan backtest                       | Wajib              |
| Dapat membaca total trades, drawdown, exit reason | Wajib             |
| Memahami risiko overfitting                      | Wajib              |
| Memahami bahwa hyperopt tidak menjamin profit    | Wajib              |
| Memahami prosedur audit strategy komunitas       | Wajib              |
| Strategy sudah diuji di dry-run                  | Wajib sebelum live |

Artikel berikutnya membahas operations: pengelolaan bot pada fase mendekati live dan live.

---

## Glossary

| Istilah           | Penjelasan                                                                        |
| ----------------- | ---------------------------------------------------------------------------------- |
| Strategy          | Sekumpulan aturan entry, exit, dan risk control                                    |
| DataFrame         | Struktur data tabel (pandas) untuk memproses candle dan indikator                 |
| OHLCV             | Open, High, Low, Close, Volume                                                     |
| Indikator         | Perhitungan teknikal berbasis data harga dan volume                                |
| SMA               | Simple Moving Average                                                              |
| EMA               | Exponential Moving Average                                                         |
| RSI               | Relative Strength Index                                                            |
| Entry             | Kondisi masuk posisi                                                               |
| Exit              | Kondisi keluar posisi                                                              |
| Long              | Posisi yang profit jika harga naik                                                 |
| Short             | Posisi yang profit jika harga turun                                                |
| Minimal ROI       | Target profit minimum berdasarkan durasi trade                                     |
| Stoploss          | Batas kerugian maksimal per trade                                                  |
| Trailing Stoploss | Stoploss yang bergerak mengikuti harga ke arah profit                              |
| Lagging Indicator | Indikator yang bereaksi setelah pergerakan harga terjadi                           |
| Backtesting       | Pengujian strategy menggunakan data historis                                       |
| Timerange         | Rentang periode data yang digunakan untuk pengujian                                |
| Hyperopt          | Optimasi parameter strategy terhadap objective tertentu                            |
| Loss Function     | Objective yang digunakan hyperopt untuk menilai hasil                              |
| Epoch             | Satu iterasi pencarian parameter pada hyperopt                                     |
| Overfitting       | Kondisi strategy terlalu cocok dengan data historis dan gagal pada data baru       |
| Out-of-sample     | Data yang tidak digunakan dalam proses pengembangan, dipakai untuk validasi akhir  |
| Drawdown          | Penurunan nilai akun dari titik tertinggi                                          |
| Winrate           | Persentase trade yang profit                                                       |
| Exit Reason       | Alasan posisi ditutup                                                              |
| Pair Performance  | Performa strategy per pair                                                         |
| Dry-run           | Simulasi bot pada market real-time tanpa order sungguhan                           |
| Live Trading      | Bot menjalankan order sungguhan di exchange                                        |

---

## Ringkasan

* Strategy adalah sekumpulan aturan yang membuat keputusan trading konsisten; strategy tidak memprediksi market.
* Freqtrade memproses data candle dalam DataFrame; indikator ditambahkan sebagai kolom, dan aturan entry/exit dievaluasi terhadap kolom tersebut.
* Strategy sederhana yang dipahami sepenuhnya lebih baik sebagai titik awal daripada strategy kompleks yang tidak dipahami.
* SMA crossover sesuai untuk mempelajari konsep trend tetapi lemah pada market sideways; RSI sesuai untuk mempelajari momentum tetapi memerlukan trend filter; kombinasi trend, RSI, dan volume lebih dapat dipertanggungjawabkan tetapi tetap harus diuji.
* ROI, stoploss, dan trailing stoploss adalah komponen risk control yang setara pentingnya dengan aturan entry.
* Hasil backtest dievaluasi dari seluruh metrik — total trades, winrate, average profit, drawdown, distribusi exit reason, dan pair performance — bukan hanya total profit.
* Iterasi strategy dilakukan satu perubahan per siklus dengan pencatatan hasil pembanding.
* Hyperopt adalah alat eksplorasi parameter; hasilnya divalidasi pada data out-of-sample dan dry-run.
* Overfitting adalah penyebab kegagalan paling umum; mitigasinya adalah strategi sederhana, pemisahan data, dan pengujian lintas periode dan pair.
* Strategy komunitas digunakan sebagai referensi belajar dengan prosedur audit, bukan dijalankan langsung di mode live.

---

## Disclaimer

Artikel ini ditulis untuk tujuan edukasi. Contoh strategy dalam artikel ini bukan rekomendasi trading, bukan sinyal beli atau jual, dan bukan strategy siap live.

Crypto trading memiliki risiko tinggi. Bot trading tidak menjamin profit. Backtesting tidak menjamin hasil masa depan. Hyperopt tidak menjamin parameter yang optimal untuk kondisi market real. Jangan menjalankan strategy dalam mode live sebelum memahami logika strategy, risk management, stoploss, position sizing, fee exchange, slippage, dan risiko operasional.

Setiap keputusan trading adalah tanggung jawab masing-masing pengguna.

---

2026 - Kenshin Himura
