# Artikel 3 — Strategy

# Meracik Build Strategy Freqtrade: Dari Candlestick ke Backtesting

> Artikel ini membahas cara memahami data candle, dataframe, indikator teknikal, struktur strategy Freqtrade, contoh strategy sederhana, ROI, stoploss, trailing stoploss, backtesting, hyperopt, overfitting, dan penggunaan strategy komunitas sebagai referensi.

---

## Daftar Isi

1. [Tujuan Artikel](#tujuan-artikel)
2. [Mindset Dasar Strategy](#mindset-dasar-strategy)
3. [Strategy Bukan Ramalan Market](#strategy-bukan-ramalan-market)
4. [Memahami Data Candlestick](#memahami-data-candlestick)
5. [Memahami Dataframe](#memahami-dataframe)
6. [OHLCV sebagai Bahan Dasar Strategy](#ohlcv-sebagai-bahan-dasar-strategy)
7. [Apa Itu Indikator Teknikal](#apa-itu-indikator-teknikal)
8. [Indikator Dasar yang Perlu Dipahami](#indikator-dasar-yang-perlu-dipahami)
9. [Struktur Dasar Strategy Freqtrade](#struktur-dasar-strategy-freqtrade)
10. [Membuat Strategy Pertama](#membuat-strategy-pertama)
11. [Contoh Strategy 1: SMA Crossover](#contoh-strategy-1-sma-crossover)
12. [Contoh Strategy 2: RSI Basic](#contoh-strategy-2-rsi-basic)
13. [Contoh Strategy 3: Trend, RSI, dan Volume](#contoh-strategy-3-trend-rsi-dan-volume)
14. [ROI, Stoploss, dan Trailing Stoploss](#roi-stoploss-dan-trailing-stoploss)
15. [Backtesting](#backtesting)
16. [Membaca Hasil Backtest](#membaca-hasil-backtest)
17. [Iterasi Strategy](#iterasi-strategy)
18. [Hyperparameter Optimization](#hyperparameter-optimization)
19. [Menghindari Overfitting](#menghindari-overfitting)
20. [Menggunakan Strategy Komunitas sebagai Referensi](#menggunakan-strategy-komunitas-sebagai-referensi)
21. [Checklist Sebelum Lanjut ke Operations](#checklist-sebelum-lanjut-ke-operations)
22. [Glossary](#glossary)
23. [Key Takeaways](#key-takeaways)
24. [Disclaimer](#disclaimer)

---

## Tujuan Artikel

Artikel ini adalah lanjutan dari Artikel 2 — Setup.

Jika artikel sebelumnya membahas cara membangun environment Freqtrade, artikel ini membahas cara mulai menulis dan mengevaluasi strategy.

Pada tahap ini, pembaca sudah diasumsikan memahami:

* Apa Itu Cryptocurrency.
* Apa Itu Exchange.
* Apa Itu Pair.
* Apa Itu API Key.
* Apa Itu Dry-run.
* Cara Menjalankan Freqtrade di VPS.
* Cara Membaca Log Dasar.
* Cara Mengakses FreqUI.

Target artikel ini adalah membuat pembaca memahami proses berpikir di balik strategy:

* Membaca Data Candle.
* Mengolah Dataframe.
* Menggunakan Indikator.
* Membuat Aturan Entry.
* Membuat Aturan Exit.
* Mengatur ROI.
* Mengatur Stoploss.
* Menjalankan Backtesting.
* Membaca Hasil Backtest.
* Menghindari Overfitting.
* Menggunakan Strategy Komunitas Secara Sehat.

Dalam konteks gamer, artikel ini mirip proses meracik build karakter. Build yang baik harus diuji dalam banyak kondisi. Build juga harus punya batasan, kelemahan, dan aturan penggunaan. Strategy trading juga begitu.

---

## Mindset Dasar Strategy

Strategy adalah sekumpulan aturan.

Strategy menjawab pertanyaan:

```text id="xw6toj"
Kapan bot boleh masuk posisi?
Kapan bot harus keluar posisi?
Berapa risiko yang boleh diterima?
Berapa target profit yang masuk akal?
Kapan kondisi market dianggap tidak valid?
```

Strategy yang baik tidak harus selalu benar. Strategy yang baik harus memiliki aturan yang jelas ketika benar dan ketika salah.

Pemula sering berpikir bahwa strategy bagus adalah strategy yang selalu menang. Ini keliru. Dalam trading, loss adalah bagian dari sistem. Yang lebih penting adalah:

* Kerugian Terkendali.
* Ukuran Posisi Masuk Akal.
* Profit Tidak Hanya Bergantung pada Satu Trade.
* Strategy Tidak Hancur Karena Beberapa Loss Beruntun.
* Hasil Backtest Tidak Dibaca Secara Buta.
* Strategy Tetap Diuji di Dry-run Sebelum Live.

Di dunia game, build yang baik tidak selalu menang dalam semua situasi. Ada build yang kuat untuk farming, ada build yang kuat untuk boss, ada build yang kuat untuk PvP, dan ada build yang hanya kuat karena meta tertentu.

Strategy trading juga punya konteks.

Ada strategy yang cocok untuk trend market. Ada strategy yang cocok untuk sideways market. Ada strategy yang terlihat bagus saat market bullish, tetapi buruk saat market bearish.

Karena itu, jangan mencari strategy sempurna. Cari strategy yang dapat dipahami, diuji, dan dikontrol risikonya.

---

## Strategy Bukan Ramalan Market

Strategy bukan alat untuk mengetahui masa depan. Strategy hanya membuat aturan berdasarkan data yang tersedia.

Contoh sederhana:

```text id="wco2dr"
Jika trend naik dan momentum membaik, bot boleh mencari entry.
Jika harga bergerak melawan posisi, bot harus keluar berdasarkan stoploss.
Jika target profit tercapai, bot boleh keluar berdasarkan ROI.
```

Aturan tersebut tidak menjamin harga akan naik. Aturan tersebut hanya membuat keputusan menjadi lebih konsisten. Tanpa strategy, trader mudah masuk karena emosi:

* Takut Ketinggalan.
* Panik Melihat Candle Hijau.
* Takut Cut Loss.
* Ingin Balas Dendam Setelah Loss.
* Terlalu Percaya Rumor.
* Terlalu Percaya Satu Indikator.

Bot membantu mengurangi emosi, tetapi bot tidak menghapus risiko. Bot hanya menjalankan aturan yang ditulis. Jika aturan buruk, hasilnya juga buruk.

---

## Memahami Data Candlestick

Freqtrade bekerja menggunakan data candle. Setiap candle merepresentasikan pergerakan harga dalam satu periode waktu tertentu. Contoh timeframe:

| Timeframe | Arti                          |
| --------- | ----------------------------- |
| `1m`      | Satu candle mewakili 1 menit  |
| `5m`      | Satu candle mewakili 5 menit  |
| `15m`     | Satu candle mewakili 15 menit |
| `1h`      | Satu candle mewakili 1 jam    |
| `4h`      | Satu candle mewakili 4 jam    |
| `1d`      | Satu candle mewakili 1 hari   |

Timeframe sangat mempengaruhi karakter strategy.

Strategy `5m` biasanya:

* Memberi Sinyal Lebih Sering.
* Lebih Sensitif terhadap Noise.
* Lebih Terpengaruh Fee.
* Lebih Membutuhkan Monitoring.

Strategy `1h` biasanya:

* Memberi Sinyal Lebih Jarang.
* Lebih Lambat Bereaksi.
* Lebih Mudah Dibaca oleh Pemula.
* Lebih Sedikit Terpengaruh Noise Kecil.

Tidak ada timeframe yang selalu paling baik. Timeframe harus sesuai dengan karakter strategy dan kemampuan monitoring.

---

## Memahami Dataframe

Dalam Freqtrade, data candle diproses dalam bentuk dataframe. Dataframe dapat dipahami sebagai tabel data.

Setiap baris merepresentasikan satu candle. Setiap kolom berisi informasi seperti open, high, low, close, volume, dan indikator tambahan.

Contoh sederhana:

| Date             | Open | High | Low | Close | Volume |
| ---------------- | ---: | ---: | --: | ----: | -----: |
| 2026-01-01 00:00 |  100 |  105 |  98 |   103 |   1200 |
| 2026-01-01 00:05 |  103 |  108 | 101 |   107 |   1500 |
| 2026-01-01 00:10 |  107 |  109 | 104 |   105 |   1300 |

Strategy akan menambahkan kolom baru ke dataframe.

Contoh:

| Date             | Close | Volume | SMA20 | SMA50 | RSI |
| ---------------- | ----: | -----: | ----: | ----: | --: |
| 2026-01-01 00:00 |   103 |   1200 |   101 |    98 |  55 |
| 2026-01-01 00:05 |   107 |   1500 |   102 |    99 |  61 |
| 2026-01-01 00:10 |   105 |   1300 |   103 |   100 |  58 |

Kolom tambahan tersebut digunakan untuk membuat aturan entry dan exit.

Contoh logika:

```text id="dzm25u"
Jika SMA20 memotong ke atas SMA50, maka trend jangka pendek mulai menguat.
Jika RSI terlalu rendah, maka aset mungkin oversold.
Jika volume naik, maka sinyal dianggap lebih valid.
```

Indikator hanya alat bantu membaca data.

---

## OHLCV sebagai Bahan Dasar Strategy

OHLCV adalah format data dasar dalam trading.

| Komponen | Arti                                    |
| -------- | --------------------------------------- |
| Open     | Harga pembukaan candle                  |
| High     | Harga tertinggi candle                  |
| Low      | Harga terendah candle                   |
| Close    | Harga penutupan candle                  |
| Volume   | Jumlah aktivitas transaksi dalam candle |

Dalam banyak strategy sederhana, harga close sering digunakan sebagai basis perhitungan indikator.

Contoh:

* SMA Menggunakan Rata-Rata Close.
* EMA Menggunakan Close dengan Bobot Lebih Besar pada Data Terbaru.
* RSI Menggunakan Perubahan Harga untuk Membaca Momentum.
* Volume Digunakan untuk Melihat Kekuatan Pergerakan.

OHLCV adalah raw material. Indikator adalah hasil olahan. Entry dan exit adalah keputusan berdasarkan aturan.

---

## Apa Itu Indikator Teknikal

Indikator teknikal adalah perhitungan matematis berdasarkan data market. Indikator digunakan untuk membantu membaca:

* Trend.
* Momentum.
* Volatilitas.
* Volume.
* Kondisi Overbought.
* Kondisi Oversold.
* Potensi Reversal.
* Potensi Continuation.

Indikator tidak memberi jaminan. Indikator hanya menyederhanakan informasi dari harga dan volume. Kesalahan umum pemula adalah menganggap indikator sebagai sinyal yang pasti. Contoh keliru:

```text id="u1rv3g"
RSI di bawah 30 berarti harga pasti naik.
```

Interpretasi yang lebih sehat:

```text id="6grf8e"
RSI di bawah 30 menunjukkan kondisi oversold berdasarkan perhitungan RSI, tetapi harga masih bisa turun lebih jauh.
```

Dalam strategy, indikator sebaiknya digunakan bersama konteks lain.

Misalnya:

* Trend Filter.
* Volume Filter.
* Market Structure.
* Stoploss.
* Pair Selection.
* Timeframe.

---

## Indikator Dasar yang Perlu Dipahami

### SMA

SMA adalah Simple Moving Average. SMA menghitung rata-rata harga dalam periode tertentu. Contoh:

```text id="sj9d0g"
SMA20 = Rata-rata close 20 candle terakhir.
SMA50 = Rata-rata close 50 candle terakhir.
```

SMA membantu membaca trend secara sederhana. Jika SMA pendek berada di atas SMA panjang, trend jangka pendek cenderung lebih kuat. Jika SMA pendek berada di bawah SMA panjang, trend jangka pendek cenderung lebih lemah.

### EMA

EMA adalah Exponential Moving Average. EMA mirip SMA, tetapi memberikan bobot lebih besar pada data terbaru. EMA biasanya lebih cepat bereaksi terhadap perubahan harga dibanding SMA. Namun, karena lebih cepat, EMA juga bisa lebih sensitif terhadap noise.

### RSI

RSI adalah Relative Strength Index. RSI digunakan untuk membaca momentum. Nilai RSI biasanya bergerak dari 0 sampai 100. Interpretasi umum:

|         RSI | Interpretasi Umum |
| ----------: | ----------------- |
| Di bawah 30 | Oversold          |
|  Sekitar 50 | Netral            |
|  Di atas 70 | Overbought        |

Namun, RSI tidak boleh digunakan secara buta. Dalam trend yang kuat, RSI bisa bertahan lama di area overbought atau oversold.

### Volume

Volume menunjukkan aktivitas transaksi. Volume membantu menilai apakah pergerakan harga didukung aktivitas market yang cukup.

Contoh:

* Harga Naik dengan Volume Naik Dapat Menunjukkan Momentum Lebih Kuat.
* Harga Naik dengan Volume Lemah Dapat Menunjukkan Kenaikan Rapuh.
* Harga Turun dengan Volume Besar Dapat Menunjukkan Tekanan Jual Kuat.

Volume bukan indikator satu-satunya untuk melakukan entry. Volume lebih baik digunakan sebagai filter.

---

## Struktur Dasar Strategy Freqtrade

Strategy Freqtrade ditulis sebagai file Python. File strategy biasanya disimpan di:

```text id="spvj83"
user_data/strategies/
```

Struktur dasar strategy:

```python id="lttkn8"
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

Tiga function utama:

| Function               | Fungsi                                                  |
| ---------------------- | ------------------------------------------------------- |
| `populate_indicators`  | Menghitung indikator dan menambahkan kolom ke dataframe |
| `populate_entry_trend` | Menentukan kondisi entry                                |
| `populate_exit_trend`  | Menentukan kondisi exit                                 |

Dalam pendekatan sederhana:

```text id="tgs30b"
populate_indicators = Membuat data tambahan.
populate_entry_trend = Menentukan kapan masuk.
populate_exit_trend = Menentukan kapan keluar.
```

Analogi gamer:

```text id="7c3qxj"
populate_indicators = Membaca stat dan kondisi arena.
populate_entry_trend = Memutuskan kapan menyerang.
populate_exit_trend = Memutuskan kapan mundur.
```

---

## Membuat Strategy Pertama

Buat strategy baru dengan command:

```bash id="ji5cw5"
cd ~/freqtrade
docker compose run --rm freqtrade new-strategy --strategy SmaCrossStrategy
```

File strategy akan dibuat di:

```text id="bd1c0j"
user_data/strategies/SmaCrossStrategy.py
```

Buka file tersebut:

```bash id="5aqwz4"
nano user_data/strategies/SmaCrossStrategy.py
```

Untuk belajar, lebih baik mulai dari strategy yang sangat sederhana. Jangan langsung membuat strategy dengan terlalu banyak indikator. Strategy pertama harus mudah dipahami. Tujuannya untuk:

* Memahami Struktur File.
* Memahami Cara Menambahkan Indikator.
* Memahami Cara Membuat Entry.
* Memahami Cara Membuat Exit.
* Memahami Cara Menjalankan Backtest.
* Memahami Cara Membaca Hasil.

---

## Contoh Strategy 1: SMA Crossover

SMA crossover adalah contoh klasik. Logika dasarnya:

```text id="vm2xzg"
Entry:
Jika SMA pendek memotong ke atas SMA panjang.

Exit:
Jika SMA pendek memotong ke bawah SMA panjang.
```

Contoh strategy:

```python id="ftdbgs"
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

Penjelasan:

| Bagian          | Fungsi                                               |
| --------------- | ---------------------------------------------------- |
| `sma_fast`      | Moving average cepat                                 |
| `sma_slow`      | Moving average lambat                                |
| `crossed_above` | Sinyal ketika SMA cepat memotong ke atas SMA lambat  |
| `crossed_below` | Sinyal ketika SMA cepat memotong ke bawah SMA lambat |
| `volume > 0`    | Filter agar candle valid                             |
| `enter_long`    | Sinyal entry long                                    |
| `exit_long`     | Sinyal exit long                                     |

Kelebihan strategy ini:

* Mudah Dipahami.
* Cocok untuk Belajar Struktur Strategy.
* Cocok untuk Melihat Konsep Trend.

Kelemahan strategy ini:

* Sering Terlambat Masuk.
* Bisa Banyak False Signal di Market Sideways.
* Tidak Cukup Kuat Tanpa Filter Tambahan.
* Tidak Boleh Dipakai Live Tanpa Pengujian Lanjutan.

---

## Contoh Strategy 2: RSI Basic

RSI sering digunakan untuk membaca kondisi oversold dan overbought. Logika sederhana:

```text id="2ugkao"
Entry:
Jika RSI turun di bawah 30, aset dianggap oversold.

Exit:
Jika RSI naik di atas 70, aset dianggap overbought.
```

Contoh strategy:

```python id="fem3te"
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

Kelebihan strategy ini:

* Mudah Dipahami.
* Cocok untuk Belajar Momentum.
* Sederhana untuk Backtesting Awal.

Kelemahan strategy ini:

* RSI Oversold Tidak Selalu Berarti Harga Akan Naik.
* RSI Overbought Tidak Selalu Berarti Harga Akan Turun.
* Bisa Buruk Saat Downtrend Kuat.
* Butuh Trend Filter agar Lebih Rasional.

Dalam market bearish, RSI bisa terus berada di area rendah sementara harga tetap turun. Karena itu, RSI sebaiknya tidak digunakan sendirian.

---

## Contoh Strategy 3: Trend, RSI, dan Volume

Strategy ketiga menggabungkan beberapa filter. Logika dasarnya:

```text id="l53rth"
Entry:
Harga berada di atas EMA200.
RSI berada di bawah 40.
Volume lebih tinggi dari rata-rata volume.

Exit:
RSI naik di atas 65 atau harga turun di bawah EMA200.
```

Tujuan strategy ini adalah menghindari entry melawan trend besar. Contoh strategy:

```python id="f3z78g"
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

Penjelasan:

| Komponen                  | Fungsi                                            |
| ------------------------- | ------------------------------------------------- |
| `ema_200`                 | Filter trend besar                                |
| `rsi < 40`                | Mencari pullback atau momentum rendah             |
| `volume > volume_mean_20` | Memastikan ada aktivitas market                   |
| `close > ema_200`         | Menghindari entry saat harga di bawah trend utama |
| `rsi > 65`                | Exit saat momentum mulai tinggi                   |
| `close < ema_200`         | Exit jika trend filter tidak valid                |

Strategy ini lebih realistis daripada dua contoh sebelumnya. Strategy ini hanya contoh untuk pembelajaran.

Kelebihan:

* Memiliki Trend Filter.
* Memiliki Momentum Filter.
* Memiliki Volume Filter.
* Lebih Mudah Dijelaskan Secara Logis.

Kelemahan:

* Bisa Terlambat Entry.
* Bisa Tidak Masuk Saat Market Bergerak Cepat.
* Parameter Belum Dioptimasi.
* Tetap Bisa Rugi di Market Choppy.
* Harus Diuji pada Banyak Periode Market.

---

## ROI, Stoploss, dan Trailing Stoploss

Strategy tidak hanya berisi entry. Exit dan risk control sama pentingnya. Bahkan, untuk pemula, risk control lebih penting daripada entry.

### Minimal ROI

`minimal_roi` menentukan target profit berdasarkan durasi trade.

Contoh:

```python id="abrtw0"
minimal_roi = {
    "0": 0.03,
    "60": 0.015,
    "120": 0.005
}
```

Interpretasi sederhana:

| Waktu Trade       | Target ROI |
| ----------------- | ---------: |
| 0 Menit           |       3.0% |
| Setelah 60 Menit  |       1.5% |
| Setelah 120 Menit |       0.5% |

Artinya, semakin lama posisi terbuka, target profit bisa diturunkan. Ini membantu bot keluar dari posisi yang sudah tidak bergerak sesuai harapan.

### Stoploss

Stoploss adalah batas kerugian maksimal per trade berdasarkan konfigurasi strategy.

Contoh:

```python id="iypc0f"
stoploss = -0.08
```

Artinya, stoploss berada di sekitar minus 8%. Yang perlu dipahami adalah: stoploss bukan tanda strategy buruk. Stoploss adalah mekanisme bertahan hidup.

Tanpa stoploss, bot bisa menahan posisi buruk terlalu lama. Dalam game, stoploss mirip keputusan mundur dari raid ketika party sudah jelas tidak mampu menyelesaikan boss.

Mundur bukan gagal total. Mundur adalah cara menjaga resource untuk percobaan berikutnya.

### Trailing Stoploss

Trailing stoploss adalah stoploss yang bergerak mengikuti profit.

Contoh:

```python id="kn47rw"
trailing_stop = True
trailing_stop_positive = 0.01
trailing_stop_positive_offset = 0.02
trailing_only_offset_is_reached = True
```

Interpretasi sederhana:

* Trailing Stop Diaktifkan.
* Trailing Baru Aktif Setelah Profit Tertentu Tercapai.
* Jika Harga Berbalik Setelah Profit, Bot Bisa Mengunci Sebagian Profit.
* Trailing Stop Membantu Saat Market Bergerak Kuat Lalu Berbalik.

Jika terlalu ketat, posisi bisa keluar terlalu cepat. Jika terlalu longgar, profit bisa hilang kembali. Trailing stoploss harus diuji melalui backtest dan dry-run.

---

## Backtesting

Backtesting adalah proses menguji strategy menggunakan data historis. Tujuannya adalah melihat bagaimana strategy bekerja jika dijalankan pada periode market sebelumnya.

Backtesting tidak menjamin hasil masa depan. Namun, backtesting membantu menjawab pertanyaan penting:

* Apakah Strategy Pernah Memberi Sinyal?
* Apakah Strategy Terlalu Sering Trading?
* Apakah Strategy Terlalu Jarang Trading?
* Apakah Stoploss Terlalu Sering Kena?
* Apakah Profit Hanya Berasal dari Satu Trade Besar?
* Apakah Drawdown Terlalu Dalam?
* Apakah Strategy Hanya Bagus di Market Bullish?
* Apakah Strategy Masih Masuk Akal Setelah Fee?

### Download Data Historis

Sebelum backtest, download data historis.

Contoh:

```bash id="zn7jty"
cd ~/freqtrade

docker compose run --rm freqtrade download-data \
  --exchange binance \
  --pairs BTC/USDT ETH/USDT \
  --timeframes 5m \
  --timerange 20240101-20240601
```

Sesuaikan exchange, pair, timeframe, dan timerange.

### Menjalankan Backtest

Contoh backtest strategy:

```bash id="tw7hm4"
docker compose run --rm freqtrade backtesting \
  --config user_data/config.json \
  --strategy SmaCrossStrategy \
  --timerange 20240101-20240601
```

Contoh backtest strategy RSI:

```bash id="1ixtvm"
docker compose run --rm freqtrade backtesting \
  --config user_data/config.json \
  --strategy RsiBasicStrategy \
  --timerange 20240101-20240601
```

Contoh backtest strategy trend, RSI, dan volume:

```bash id="qmewe8"
docker compose run --rm freqtrade backtesting \
  --config user_data/config.json \
  --strategy TrendRsiVolumeStrategy \
  --timerange 20240101-20240601
```

Hasil backtest biasanya berisi metrik seperti total trades, profit, winrate, average duration, drawdown, dan detail trade.

Jangan hanya melihat total profit. Total profit adalah angka paling menggoda, tetapi bukan satu-satunya angka yang penting.

---

## Membaca Hasil Backtest

Saat membaca hasil backtest, gunakan pendekatan yang kritis.

### Total Profit

Total profit menunjukkan hasil akhir strategy pada periode backtest.

Namun, total profit bisa menipu.

Strategy bisa terlihat profit karena satu trade besar, sementara banyak trade lain buruk.

### Total Trades

Total trades menunjukkan jumlah transaksi. Jika jumlah trade terlalu sedikit, hasil backtest kurang meyakinkan.

Contoh:

```text id="ghp28k"
Profit 20% dari 3 trade tidak cukup kuat untuk menyimpulkan strategy bagus.
```

Jumlah trade yang terlalu banyak juga perlu diperiksa karena fee dan slippage dapat mempengaruhi hasil live.

### Winrate

Winrate menunjukkan persentase trade yang profit. Winrate tinggi tidak selalu berarti strategy bagus.

Strategy dengan winrate tinggi tetap bisa rugi jika satu loss besar menghapus banyak profit kecil.

Sebaliknya, strategy dengan winrate rendah bisa tetap profit jika profit per trade jauh lebih besar daripada loss.

### Average Profit

Average profit menunjukkan rata-rata profit per trade. Metrik ini membantu melihat kualitas trade secara umum. Namun, tetap perlu dibandingkan dengan fee, spread, dan slippage.

### Drawdown

Drawdown menunjukkan penurunan nilai akun dari titik tertinggi ke titik lebih rendah. Drawdown sangat penting.

Strategy dengan profit tinggi tetapi drawdown terlalu besar bisa tidak layak digunakan.

Dalam live trading, drawdown besar dapat merusak psikologi dan meningkatkan risiko stop bot di waktu yang salah.

### Best Trade dan Worst Trade

Periksa trade terbaik dan terburuk.

Jika profit strategy hanya bergantung pada satu trade terbaik, strategy mungkin rapuh.

Jika satu trade terburuk terlalu besar, risk control perlu diperbaiki.

### Pair Performance

Lihat performa per pair. Strategy mungkin bagus di BTC/USDT tetapi buruk di coin lain.

Jangan menyimpulkan strategy universal hanya dari satu pair.

### Exit Reason

Periksa alasan exit.

Contoh exit reason:

| Exit Reason       | Arti                          |
| ----------------- | ----------------------------- |
| ROI               | Keluar karena target ROI      |
| Stoploss          | Keluar karena batas kerugian  |
| Exit Signal       | Keluar karena sinyal strategy |
| Trailing Stoploss | Keluar karena trailing stop   |
| Force Exit        | Keluar karena dipaksa         |

Jika terlalu banyak exit karena stoploss, strategy mungkin entry terlalu buruk atau stoploss terlalu ketat.

Jika terlalu banyak posisi keluar terlalu cepat, ROI atau trailing stop mungkin perlu diperiksa.

---

## Iterasi Strategy

Strategy development adalah proses iterasi.

Jangan berharap strategy pertama langsung bagus.

Proses yang sehat:

```text id="hm4f91"
Tulis strategy sederhana.
Jalankan backtest.
Baca hasil.
Identifikasi masalah.
Ubah satu bagian.
Backtest ulang.
Bandingkan hasil.
Uji di periode lain.
Jalankan dry-run.
Evaluasi ulang.
```

Perubahan harus dilakukan secara terkontrol. Jangan mengubah terlalu banyak parameter sekaligus.

Jika semua parameter diubah bersamaan, sulit mengetahui bagian mana yang memperbaiki atau merusak hasil.

Contoh iterasi yang sehat:

* Ubah Periode SMA dari 20/50 ke 10/50.
* Jalankan Backtest Ulang.
* Bandingkan Total Trades, Profit, Drawdown, dan Exit Reason.
* Catat Hasil.
* Baru Ubah Komponen Lain.

Contoh iterasi yang buruk:

* Mengubah Timeframe.
* Mengubah SMA.
* Mengubah RSI.
* Mengubah Stoploss.
* Mengubah ROI.
* Mengubah Pair.
* Menyimpulkan Strategy Lebih Baik Tanpa Catatan Jelas.

Strategy development harus seperti testing build secara disiplin. Jangan seperti random reroll stat tanpa log.

---

## Hyperparameter Optimization

Hyperparameter optimization, atau hyperopt, adalah proses mencari parameter strategy yang lebih optimal berdasarkan objective tertentu.

Contoh parameter yang bisa dioptimasi:

* Periode RSI.
* Batas RSI Entry.
* Batas RSI Exit.
* Periode Moving Average.
* Stoploss.
* ROI.
* Trailing Stoploss.
* Parameter Buy.
* Parameter Sell.

Hyperopt dapat membantu eksplorasi parameter, tetapi sangat mudah disalahgunakan.

Jika hyperopt dilakukan terlalu agresif pada data yang sama, hasilnya bisa terlalu cocok dengan masa lalu.

Itu disebut overfitting.

### Contoh Konsep Parameter

Dalam strategy, parameter bisa dibuat seperti ini:

```python id="d3ggci"
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

### Contoh Menjalankan Hyperopt

Contoh command:

```bash id="uvbqps"
docker compose run --rm freqtrade hyperopt \
  --config user_data/config.json \
  --strategy RsiHyperoptStrategy \
  --hyperopt-loss SharpeHyperOptLoss \
  --spaces buy sell \
  --timerange 20240101-20240601 \
  -e 100
```

Penjelasan sederhana:

| Parameter         | Fungsi                                       |
| ----------------- | -------------------------------------------- |
| `--strategy`      | Strategy yang akan dioptimasi                |
| `--hyperopt-loss` | Objective yang digunakan untuk menilai hasil |
| `--spaces`        | Area parameter yang dioptimasi               |
| `--timerange`     | Periode data historis                        |
| `-e`              | Jumlah epoch atau percobaan                  |

Hyperopt adalah alat pencari parameter. Parameter yang bagus di data masa lalu tetap harus diuji pada data lain dan dry-run.

---

## Menghindari Overfitting

Overfitting terjadi ketika strategy terlalu cocok dengan data historis tertentu, tetapi gagal di data baru.

Dalam analogi gamer, overfitting seperti build yang dibuat khusus untuk satu boss, satu arena, satu patch, dan satu kondisi. Build itu terlihat sangat kuat di simulasi tertentu, tetapi gagal ketika kondisi berubah sedikit.

Tanda-tanda overfitting:

* Profit Backtest Sangat Tinggi tetapi Hanya pada Satu Periode.
* Jumlah Trade Terlalu Sedikit.
* Parameter Terlalu Banyak.
* Strategy Terlalu Kompleks.
* Hasil Buruk Saat Diuji di Periode Lain.
* Hasil Buruk Saat Diuji di Pair Lain.
* Profit Bergantung pada Satu Trade Besar.
* Drawdown Tidak Stabil.
* Hyperopt Dilakukan Terlalu Lama pada Data yang Sama.
* Strategy Tidak Masuk Akal Secara Logika, tetapi Hasil Backtest Bagus.

Cara mengurangi risiko overfitting:

* Gunakan Strategy Sederhana Dulu.
* Pisahkan Data Training dan Testing.
* Uji pada Beberapa Periode Market.
* Uji pada Market Bullish, Bearish, dan Sideways.
* Uji pada Beberapa Pair Likuid.
* Jangan Terlalu Banyak Parameter.
* Jangan Mengejar Profit Backtest Tertinggi.
* Perhatikan Drawdown.
* Perhatikan Jumlah Trade.
* Jalankan Dry-run Setelah Backtest.
* Catat Perubahan Strategy Secara Rapi.

Contoh pembagian data:

| Periode              | Fungsi             |
| -------------------- | ------------------ |
| Januari sampai Maret | Eksperimen awal    |
| April sampai Mei     | Validasi           |
| Juni                 | Out-of-sample test |

Jika strategy hanya bagus di periode eksperimen tetapi buruk di validasi, jangan langsung dipakai.

Overfitting adalah salah satu jebakan terbesar dalam bot trading. Backtest dengan indikator hijau tidak otomatis berarti strategy siap live.

---

## Menggunakan Strategy Komunitas sebagai Referensi

Banyak strategy Freqtrade beredar di komunitas. Strategy komunitas dapat berguna untuk belajar, tetapi tidak boleh langsung dipercaya.

Gunakan strategy komunitas sebagai referensi untuk memahami:

* Struktur Code.
* Cara Menggunakan Indikator.
* Cara Membuat Entry.
* Cara Membuat Exit.
* Cara Menggunakan Parameter.
* Cara Mengatur Stoploss.
* Cara Mengatur ROI.
* Cara Membuat Filter.

Jangan menggunakan strategy komunitas dengan cara:

* Copy-Paste Langsung ke Live.
* Mengabaikan Risiko.
* Tidak Membaca Code.
* Tidak Backtest Ulang.
* Tidak Dry-run.
* Tidak Memahami Pair dan Timeframe.
* Tidak Memahami Stoploss.
* Tidak Memahami Exit Logic.

Sebelum menggunakan strategy komunitas, lakukan checklist:

| Checklist                         | Status |
| --------------------------------- | ------ |
| Code Strategy Sudah Dibaca        | Wajib  |
| Entry Logic Dipahami              | Wajib  |
| Exit Logic Dipahami               | Wajib  |
| Stoploss Dipahami                 | Wajib  |
| ROI Dipahami                      | Wajib  |
| Timeframe Dipahami                | Wajib  |
| Pair yang Cocok Dipahami          | Wajib  |
| Backtest Sendiri Dilakukan        | Wajib  |
| Dry-run Dilakukan                 | Wajib  |
| Tidak Ada Secret di File Strategy | Wajib  |

Strategy komunitas bisa menjadi bahan belajar yang baik.

Namun, setiap market condition, exchange, fee, pair, dan timeframe dapat menghasilkan hasil yang berbeda.

Tidak ada strategy publik yang otomatis cocok untuk semua pengguna.

---

## Checklist Sebelum Lanjut ke Operations

Sebelum lanjut ke Artikel 4 — Operations, pastikan poin berikut sudah dipahami.

| Checklist                                   | Status             |
| ------------------------------------------- | ------------------ |
| Memahami Data OHLCV                         | Wajib              |
| Memahami Dataframe Freqtrade Secara Dasar   | Wajib              |
| Memahami Fungsi `populate_indicators`       | Wajib              |
| Memahami Fungsi `populate_entry_trend`      | Wajib              |
| Memahami Fungsi `populate_exit_trend`       | Wajib              |
| Memahami Cara Menambahkan Indikator         | Wajib              |
| Memahami Entry Signal                       | Wajib              |
| Memahami Exit Signal                        | Wajib              |
| Memahami Minimal ROI                        | Wajib              |
| Memahami Stoploss                           | Wajib              |
| Memahami Trailing Stoploss                  | Wajib              |
| Bisa Menjalankan Backtest                   | Wajib              |
| Bisa Membaca Total Trades                   | Wajib              |
| Bisa Membaca Drawdown                       | Wajib              |
| Bisa Membaca Exit Reason                    | Wajib              |
| Memahami Risiko Overfitting                 | Wajib              |
| Memahami Hyperopt Bukan Mesin Profit        | Wajib              |
| Memahami Strategy Komunitas Hanya Referensi | Wajib              |
| Strategy Sudah Diuji di Dry-run             | Wajib Sebelum Live |

Jika belum memahami poin di atas, jangan lanjut ke live trading.

Artikel berikutnya akan membahas operations, yaitu cara mengelola bot ketika sudah masuk fase live atau mendekati live.

---

## Glossary

| Istilah           | Penjelasan                                                                        |
| ----------------- | --------------------------------------------------------------------------------- |
| Strategy          | Sekumpulan aturan untuk entry, exit, dan risk control                             |
| Dataframe         | Struktur data berbentuk tabel yang digunakan untuk memproses candle dan indikator |
| OHLCV             | Open, High, Low, Close, Volume                                                    |
| Indicator         | Perhitungan teknikal berbasis data market                                         |
| SMA               | Simple Moving Average                                                             |
| EMA               | Exponential Moving Average                                                        |
| RSI               | Relative Strength Index                                                           |
| Entry             | Kondisi masuk posisi                                                              |
| Exit              | Kondisi keluar posisi                                                             |
| Long              | Posisi yang berharap harga naik                                                   |
| Short             | Posisi yang berharap harga turun                                                  |
| ROI               | Return on Investment                                                              |
| Minimal ROI       | Target profit minimum berdasarkan durasi trade                                    |
| Stoploss          | Batas kerugian maksimal                                                           |
| Trailing Stoploss | Stoploss yang bergerak mengikuti profit                                           |
| Backtesting       | Pengujian strategy menggunakan data historis                                      |
| Hyperopt          | Optimasi parameter strategy                                                       |
| Overfitting       | Strategy terlalu cocok dengan data lama dan gagal di data baru                    |
| Drawdown          | Penurunan nilai akun dari titik tertinggi                                         |
| Winrate           | Persentase trade yang profit                                                      |
| Exit Reason       | Alasan posisi ditutup                                                             |
| Pair Performance  | Performa strategy per pasangan aset                                               |
| Dry-run           | Simulasi bot tanpa order real                                                     |
| Live Trading      | Bot menjalankan order sungguhan di exchange                                       |

---

## Key Takeaways

* Strategy adalah sekumpulan aturan, bukan ramalan market.
* Freqtrade menggunakan dataframe untuk memproses candle, indikator, entry, dan exit.
* Strategy sederhana lebih baik untuk belajar daripada strategy kompleks yang tidak dipahami.
* SMA crossover cocok untuk memahami trend, tetapi lemah di market sideways.
* RSI cocok untuk memahami momentum, tetapi tidak boleh digunakan sendirian.
* Kombinasi trend, RSI, dan volume lebih masuk akal, tetapi tetap harus diuji.
* ROI, stoploss, dan trailing stoploss adalah bagian penting dari strategy.
* Backtesting membantu membaca performa historis, tetapi tidak menjamin hasil masa depan.
* Hyperopt dapat membantu mencari parameter, tetapi dapat memperbesar risiko overfitting.
* Strategy komunitas boleh dipelajari, tetapi tidak boleh langsung digunakan live tanpa audit, backtest, dan dry-run.
* Strategy yang baik harus bisa dijelaskan secara logis, bukan hanya terlihat hijau di backtest.

---

## Disclaimer

Artikel ini hanya untuk tujuan edukasi.

Contoh strategy dalam artikel ini bukan rekomendasi trading, bukan sinyal beli atau jual, dan bukan strategy siap live.

Crypto trading memiliki risiko tinggi. Bot trading tidak menjamin profit. Backtesting tidak menjamin hasil masa depan. Hyperopt tidak menjamin parameter terbaik untuk market real.

Jangan menjalankan strategy dalam mode live sebelum memahami logic, risk management, stoploss, position sizing, exchange fee, slippage, dan risiko operasional.

Setiap keputusan trading adalah tanggung jawab pribadi masing-masing pengguna.

---

2026 - Kenshin Himura
