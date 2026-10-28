# 🤖 BOT TRADING FREE BY TENMA (Official Repository)

Selamat datang di repository resmi **Bot Trading Free by TENMA**. Repository ini menyediakan koleksi robot trading (Expert Advisor / EA) untuk platform **MetaTrader 5 (MT5)**.

Seluruh file di repository ini telah diberi penamaan rapi dan berurutan dari **`BOTTENMA1001.ex5`** sampai **`BOTTENMA1024.ex5`**. File-file ini berada dalam format biner terkompilasi (`.ex5`) yang **100% aman dan terkunci** dari pencurian resep/logika strategi, namun **dapat langsung disalin, dipasang, dan dijalankan secara penuh** di MetaTrader 5 Anda.

---

## ⚠️ PERHATIAN PENTING SEBELUM MENJALANKAN BOT!

> [!IMPORTANT]
> **Performa setiap bot SANGAT BERGANTUNG pada cara Anda mengatur parameter di tab INPUTS!**
> 
> 1. **Kesesuaian Nama File & Dokumentasi:** Nama file yang Anda unduh di repository ini (`BOTTENMA1001.ex5` s/d `BOTTENMA1024.ex5`) sama persis dengan tabel katalog dan penjelasan di bawah ini.
> 2. **Tidak Ada Settingan Default yang Cocok untuk Semua Akun:** Karakteristik akun setiap trader berbeda (modal/equity, leverage, jenis spread broker, dan pair yang ditradingkan). Anda **wajib menyesuaikan tab Inputs** sebelum menyalakan bot.
> 3. **Beberapa Bot Memiliki Karakter Ekstrem & Cacat Desain Tertentu:** Bot dalam repository ini berasal dari berbagai generasi pengembangan Tenma:
>    - Ada yang bertipe **All-In Compounding (High Risk / Ekstrem)** yang bisa menggandakan modal kecil dalam hitungan jam namun berisiko Margin Call seketika jika tren searah tanpa jeda.
>    - Ada versi **Sniper / Smart Money (SMC)** dengan winrate tinggi namun average loss besar jika Stop Loss tidak diperketat.
>    - Ada versi **Grid Sideways** yang rawan terseret floating jika pasar tiba-tiba breakout kencang.
>    - Ada versi **Confirmed-Direction & Modern Scalper** yang dilengkapi pengaman drawdown darurat (*kill switch*).
> 4. **Wajib Uji Demo Dahulu:** Selalu jalankan bot pilihan Anda di **Akun Demo** atau **Strategy Tester MT5** minimal 1–2 minggu sebelum digunakan dengan uang riil!

---

## 🛠️ PANDUAN MENGATUR INPUTS DI METATRADER 5

Sebelum mengaktifkan bot ke chart, Anda wajib memeriksa dan menyesuaikan parameternya:

1. Buka aplikasi **MetaTrader 5**.
2. Masukkan file bot (misal: `BOTTENMA1001.ex5`) ke folder `MQL5/Experts` Anda.
3. Di panel **Navigator** (`Ctrl + N`), klik kanan **Expert Advisors** $\rightarrow$ pilih **Refresh**.
4. Drag bot dari Navigator ke chart pair yang diinginkan (misal: XAUUSD M5).
5. Pada pop-up yang muncul, pilih tab **Inputs**.
6. Klik ganda pada nilai parameter yang ingin diubah (misal: sesuaikan `InpFixedLot`, `InpMaxLayers`, atau `InpBasketLossPct`), lalu klik **OK**.
7. Pastikan tombol **Algo Trading** di bar atas MetaTrader 5 berwarna hijau (aktif).

```
+-----------------------------------------------------------+
|  Expert - BOTTENMA1001                              [ _ X]|
+-----------------------------------------------------------+
|  [Common]  |  [INPUTS]  |  [Dependencies]                 |
+-----------------------------------------------------------+
|  Variable              |  Value                           |
|------------------------+----------------------------------|
|  InpFixedLot           |  0.01          <-- Sesuaikan lot |
|  InpMaxLayers          |  10            <-- Batasi layer  |
|  InpBasketLossPct      |  3.0           <-- Cut keranjang |
|  InpDailyLossPct       |  5.0           <-- Pengaman akun |
+-----------------------------------------------------------+
```

---

## 📖 KAMUS LENGKAP PARAMETER INPUTS (Fungsi & Cara Kerja)

Hampir seluruh bot TENMA berbagi parameter *Inputs* yang sejenis. Berikut fungsi mendalam dari setiap parameter agar Anda tidak salah menyetel:

### 1. Pengaturan Lot & Sizing Management
* **`InpFixedLot`**: Ukuran lot tetap per posisi (rekomendasi: `0.01` untuk modal di bawah $1,000).
* **`InpAutoLot`** (`true`/`false`): Mode otomatis di mana ukuran lot bertambah seiring bertambahnya saldo/equity akun.
* **`InpEquityPerMinLot`**: Nilai modal dasar untuk penambahan 1 lot minimum (contoh: `1000.0` artinya tiap kelipatan equity $1,000, lot bertambah 0.01).
* **`InpAllIn`** (`true`/`false`): **MODE EKSTREM**. Lot dihitung dari seluruh margin yang tersisa untuk melipatgandakan akun secara agresif. *Hanya untuk akun uji coba / modal kecil yang siap risiko 100%!*
* **`InpAllInMarginPct`**: Batas persentase equity yang boleh dipakai sebagai margin jika seluruh layer terisi (contoh: `50.0%`).
* **`InpMaxLayers`**: Batas maksimum jumlah order terbuka secara bersamaan. *Semakin kecil angka ini (misal: 3–10), akun Anda semakin aman dari Margin Call.*
* **`InpBurst`**: Jumlah posisi yang ditembakkan secara serentak dalam satu detik/siklus (ciri khas burst scalper Tenma).
* **`InpEntryEverySec`**: Jeda waktu tunggu (detik) sebelum bot menembakkan layer berikutnya.
* **`InpLayerGapATR`**: Jarak minimal antar layer baru berdasarkan volatilitas ATR (mencegah penumpukan posisi di titik harga yang sama).

### 2. Pengaturan Exit, TP, SL & Trailing Stop
* **`InpTP_ATR` / `InpTP`**: Target Take Profit berbasis kelipatan Average True Range (ATR M1/M5) atau dalam points.
* **`InpSL_ATR` / `InpSL`**: Batas Cut Loss / Stop Loss per posisi. Menutup posisi secara otomatis jika pergerakan harga melawan sejauh kelipatan ATR yang ditentukan.
* **`InpLockArm_ATR` / `InpBE_R`**: Titik aktivasi pengunci profit (*Break-Even Lock*). Jika harga sudah bergerak profit sejauh ini, SL otomatis digeser ke titik aman / impas.
* **`InpTrailGivebackPct`**: Fitur *Peak Trailing*. Menutup posisi jika profit yang sedang berjalan turun sekian persen dari titik puncaknya (contoh: `40.0%` artinya jika profit sempat $10 lalu mundur ke $6, posisi langsung ditutup agar hijau tidak berbalik menjadi merah).
* **`InpMaxHoldSec` / `InpMaxHoldMin`**: *Time-Stop*. Menutup paksa posisi jika sudah terbuka melebihi batas waktu tertentu, baik untung maupun rugi (mencegah scalper tersangkut order basi).

### 3. Logika Pembalik Arah (Flip Direction & Basket Cut)
* **`InpFlipMinSec`**: Waktu jeda minimal (detik) sebelum bot diizinkan membalik arah trading dari BUY ke SELL atau sebaliknya.
* **`InpFlipCloseOld`**: Penanganan posisi arah lama saat arah dibalik:
  * `0`: Biarkan posisi lama tetap berjalan.
  * `1`: Tutup hanya posisi arah lama yang sedang profit (*Sangat direkomendasikan*).
  * `2`: Tutup seluruh posisi arah lama tanpa pandang bulu.
* **`InpBasketLossPct`**: **Fitur Penyelamat Akun (*Basket Cut*)**. Jika total kerugian mengambang (*floating loss*) seluruh order mencapai persentase modal ini (misal: `-3.0%` equity), bot akan menutup SEMUA posisi serentak, membalik arah tren, dan memulai siklus baru dari awal.
* **`InpReboundDepthATR` & `InpReboundMinTP`**: Mendeteksi posisi yang sempat minus cukup dalam lalu berbalik arah menjadi profit; bot akan langsung melikuidasi profit tersebut dan membalik arah trading.

### 4. Filter Pengaman Akun (Risk Guards)
* **`InpMaxSpread` / `InpMaxSpreadPts`**: Batas spread maksimal (points) yang diizinkan. Bot akan **menolak membuka posisi** jika spread broker sedang melebar (misal saat berita high-impact atau rollover tengah malam).
* **`InpMaxMarginUse`**: Batas persentase maksimal penggunaan margin dari Free Margin akun Anda.
* **`InpMinMarginLevel`**: Perlindungan ketahanan margin. Bot menolak order baru jika Margin Level akun (%) berada di bawah batas aman (contoh: `< 300%`).
* **`InpDailyLossPct`**: Batas rugi harian. Jika kerugian dalam 1 hari mencapai persentase ini dari balance (misal: `5.0%` atau `10.0%`), bot berhenti beroperasi sampai keesokan harinya.
* **`InpEmergencyDD` / `InpEmergencyDDPct`**: **Tombol Darurat (*Kill-Switch*)**. Jika equity akun turun mencapai batas drawdown yang ditentukan dari puncak saldo tertinggi (misal: `15.0%` atau `30.0%`), bot akan menutup SEMUA order dan mematikan diri (*HALT*).

### 5. Filter Teknis & Indikator
* **`InpATR_Period`**: Periode indikator Average True Range untuk mengukur rentang volatilitas pasar (standar: `14`).
* **`InpEMA_Fast` & `InpEMA_Slow`**: Periode Exponential Moving Average sebagai penentu arah tren dasar (contoh: `20` dan `50`).
* **`InpADX_Period`, `InpTrendADX`, `InpSideADX`**: Pengukur kekuatan tren untuk membedakan apakah pasar sedang Trending kuat atau Sideways/Konsolidasi.
* **`InpMagic`**: Nomor identifikasi unik transaksi robot agar tidak bentrok dengan robot lain atau transaksi manual.
* **`InpSlippage`**: Batas toleransi pergeseran harga eksekusi dari broker.

---

## 📋 KATALOG LENGKAP & ANALISIS KODE SETIAP BOT (BOTTENMA1001 – BOTTENMA1024)

Berikut adalah daftar lengkap 24 file bot biner `.ex5` di repository ini beserta hasil analisis langsung dari kode sumber aslinya:

| Nama File Bot | Basis Asli Kode | Tipe Strategi & Karakter | Level Risiko | Catatan Performa & Potensi Cacat |
| :--- | :--- | :--- | :--- | :--- |
| **`BOTTENMA1001.ex5`** | `RENallingila01` (v26.0) | Fast Scalper M1 (Quick Cut) | 🔴 Sangat Tinggi | Generasi awal scalper burst. Cut loss cepat, namun jika pasar M1 bolak-balik tajam (*whipsaw*), sering terjadi cut loss beruntun. |
| **`BOTTENMA1002.ex5`** | `ALLINGILAA01` (v27.0) | Flip Scalper (Mass 0.01 Burst) | 🔴 Sangat Tinggi | Menggunakan 1 arah aktif, layer rapat (gap 0.05 ATR). Jika spread melebar, target TP tertahan dan floating cepat membengkak. |
| **`BOTTENMA1003.ex5`** | `RENOallingila02` (v27.0 Rev) | Flip Scalper Re-Tuned | 🔴 Sangat Tinggi | Disetel ulang untuk broker ECN/STP. Tetap rentan akumulasi posisi jika Basket Cut tidak diaktifkan. |
| **`BOTTENMA1004.ex5`** | `ALLINGILAAA02` (v28.0) | Flip Scalper EMA Filter | 🔴 Sangat Tinggi | Ditambahkan filter arah EMA 20/50 M5. Pada pasar flat tanpa volume, filter EMA sering terlambat mendeteksi transisi arah. |
| **`BOTTENMA1005.ex5`** | `ALLINCONPOUNDINGGILAAA01` (v30.0) | Full Compounding All-In Margin | 🔴 Ekstrem | **Risiko Ekstrem:** Margin terpakai hingga 50% equity. Potensi melipatgandakan akun sangat cepat, tapi rawan MC seketika saat news besar jika DD guard mati. |
| **`BOTTENMA1006.ex5`** | `allinconpoundinggilaaa02` (v31.0) | Compounding All-In v31 Rev | 🔴 Ekstrem | Revisi pengetatan trailing giveback & lock arm ATR. Karakter risiko tetap tergolong ekstrem. |
| **`BOTTENMA1007.ex5`** | `Gold Hunter V9 4.0` | Multi-Module Hybrid (Gold M5) | 🟡 Sedang | 6 modul mandiri (SNR, Breakout, Retest, Fib, SMC FVG, Pinbar). Performa optimal di XAUUSD M5. Rawan jika banyak modul kena false breakout berurutan. |
| **`BOTTENMA1008.ex5`** | `RENO Gold Hunter V9 4.0` | Gold Hunter Tuned Edition | 🟡 Sedang | Optimasi filter spread dan penjepit Stop Loss struktur untuk pair XAUUSDm / Gold. Karakter sama dengan BOTTENMA1007. |
| **`BOTTENMA1009.ex5`** | `RENOtrident16` (v16.0) | Sniper Compound (Staged SL) | 🟠 Agresif | **Catatan Cacat Internal:** Winrate tinggi (~73%) tapi average loss terlalu besar jika SL 1.2 ATR tidak diturunkan ke 0.8 ATR. Di pasar sideways, staged SL sering tersenggol. |
| **`BOTTENMA1010.ex5`** | `RENOtrident17` (v17.0) | Smart Money SMC + AI Neural Brain | 🟡 Sedang | Membaca BOS, Order Block, dan FVG dengan bobot AI adaptif. **Cacat:** Terlalu banyak filter konfirmasi sehingga sangat jarang membuka posisi (*low frequency*). |
| **`BOTTENMA1011.ex5`** | `RENOtrident18` (v16.0 Rev 24H) | Sniper Compound 24 Jam | 🟠 Agresif | Filter sesi trading dihapus agar aktif 24 jam. **Cacat:** Rawan tersenggol floating saat jam pergantian hari (rollover tengah malam) ketika spread melebar. |
| **`BOTTENMA1012.ex5`** | `RENOtrident19` (v19.0) | Aggressive Two-Way Scalp | 🔴 Tinggi | **Cacat Desain:** Cooldown 0 bar & entry dua arah (Buy di oversold, Sell di overbought). Sangat mudah terjebak *floating lock* saat pasar trending kuat satu arah. |
| **`BOTTENMA1013.ex5`** | `RENOtrident20` (v20.0) | Trend Rider + Sideways Basket | 🟡 Sedang | Deteksi rezim M5. Mode Trend pakai trailing lebar tanpa TP; mode Sideways pakai grid martingale. **Cacat:** Transisi ke tren besar sering membuat keranjang grid terseret drawdown. |
| **`BOTTENMA1014.ex5`** | `RENOtrident21` (v21.0) | Trend Rider + Grid Safety v21 | 🟡 Sedang | Peningkatan jarak step grid saat sideways untuk mengurangi pemborosan margin. Kelemahan mendasar grid tetap ada. |
| **`BOTTENMA1015.ex5`** | `RENOtrident22` (v22.0) | Pyramiding Trend Rider v22 | 🟠 Agresif | Menambah layer saat posisi tren sudah profit dan aman (SL di atas BE). Rawan jika terjadi pembalikan tren tajam berbentuk huruf V (*V-reversal*). |
| **`BOTTENMA1016.ex5`** | `RENOtrident23` (v23.0) | Two-Sided Grid & Pair Recovery | 🟢 Moderat | Modul trend rider dihilangkan, murni grid dua arah dengan fitur "Pair-Close Recovery" (order profit dipakai untuk menutup order loss terjauh). Butuh modal stabil. |
| **`BOTTENMA1017.ex5`** | `RENOtrident24` (v24.0) | Grid with Volatility Gate | 🟢 Moderat | Dilengkapi filter volatilitas: otomatis mematikan penambahan grid jika ada candle spike atau lonjakan ATR mendadak. |
| **`BOTTENMA1018.ex5`** | `RENOtrident25` (v25.0) | Scalp Harvest & Profit Bank | 🟢 Konservatif | Mengumpulkan profit mikro ke "bank profit", lalu saldo bank dipakai melikuidasi order merugi secara berkala. Akumulasi relatif lambat di pasar sepi. |
| **`BOTTENMA1019.ex5`** | `RENOtrident32.02` (v32.0) | Confirmed-Direction Scalper | 🟢 Konservatif | **Versi Paling Disiplin:** Sizing berbasis anggaran risiko (bukan margin), Reversal State Machine (butuh bukti N bar konfirmasi lawan), Emergency DD Halt. Sangat selektif. |
| **`BOTTENMA1020.ex5`** | `RENOtrident33` (v33.0) | Modern Flip Scalper M1 | 🟠 Agresif | Perpaduan kecepatan burst scalper M1 dengan filter tren awal EMA M5. Sangat membutuhkan broker dengan eksekusi instan dan spread tipis. |
| **`BOTTENMA1021.ex5`** | `RENOtrident35` (v35.0) | Dynamic ATR Flip Scalper | 🟠 Agresif | Penyesuaian TP/SL adaptif mengikuti besarnya candle ATR real-time. Jika ATR menyempit, target profit menjadi sangat kecil terpotong komisi. |
| **`BOTTENMA1022.ex5`** | `RENOtrident36` (v36.0) | Multi-Timeframe Scalper M1+M5 | 🟡 Sedang | Konfirmasi tren ganda (M1 + M5 simultan). Mengurangi risiko salah arah pada false reversal, meski kadang kehilangan momen di awal tren. |
| **`BOTTENMA1023.ex5`** | `RENOtrident37` (v37.0) | Spread Guard & Time-Stop Clamp | 🟡 Sedang | Dilengkapi Time-Stop ketat (menutup paksa order basi setelah 90-120 detik) dan filter penolak spread lebar. Sering cut rugi tipis karena batas waktu. |
| **`BOTTENMA1024.ex5`** | `RENOtrident38` (v38.0) | Peak Trailing & Freeze Guard | 🟡 Sedang | **Versi Puncak Seri Flip Scalper:** Penyempurnaan fitur Peak Trailing Giveback (profit mundur 40% langsung tutup), batas Freeze Loss, dan Basket Cut darurat. |

---

## 💡 REKOMENDASI PENGATURAN INPUT BERDASARKAN KONDISI TRADER

1. **Untuk Akun Modal Kecil ($50 – $200) / Akun Belajar:**
   * **Pilihan Bot:** Gunakan `BOTTENMA1016.ex5` (v23 Grid Recovery), `BOTTENMA1018.ex5` (v25 Scalp Harvest), atau `BOTTENMA1019.ex5` (v32.02 Confirmed Direction).
   * **Settingan:**
     * `InpFixedLot = 0.01`
     * `InpAutoLot = false`
     * `InpAllIn = false`
     * `InpMaxLayers = 3` sampai `5`
     * `InpBasketLossPct = 3.0`% sampai `5.0`%

2. **Untuk Pecinta Scalping Cepat / Akun Sen (Compounding Challenge):**
   * **Pilihan Bot:** Gunakan `BOTTENMA1005.ex5`, `BOTTENMA1020.ex5`, atau `BOTTENMA1024.ex5`.
   * **Syarat Akun:** Akun wajib bertipe **HEDGE** (bukan Netting) dengan spread rendah (ECN / Zero Spread).
   * **Settingan Pengaman Wajib:**
     * `InpMaxLayers = 10` sampai `20`
     * `InpBasketLossPct = 3.0`% (wajib aktif agar memotong kerugian jika tren melawan)
     * `InpDailyLossPct = 10.0`%

3. **Untuk Trading Khusus Emas (XAUUSD / Gold M5):**
   * **Pilihan Bot:** Gunakan `BOTTENMA1007.ex5` atau `BOTTENMA1008.ex5` (Gold Hunter).
   * **Settingan:**
     * Pasang di chart timeframe **M5**.
     * Atur `InpMaxSpread` = `400` s/d `600` points (sesuaikan dengan spread Gold broker Anda).
     * `InpSlippage = 50`.

---

## 🚀 CARA INSTALASI KE METATRADER 5

1. Buka halaman repository ini di browser, lalu klik file bot yang Anda inginkan (misal: `BOTTENMA1001.ex5`).
2. Klik tombol **Download raw file** di pojok kanan atas tampilan file GitHub.
3. Buka aplikasi **MetaTrader 5** di PC / Laptop / VPS Anda.
4. Klik menu **File** $\rightarrow$ pilih **Open Data Folder**.
5. Masuk ke folder: `MQL5` $\rightarrow$ `Experts`.
6. **Paste (tempel)** file `BOTTENMA....ex5` ke dalam folder `Experts` tersebut.
7. Kembali ke aplikasi MetaTrader 5, buka jendela **Navigator** (`Ctrl + N`).
8. Klik kanan pada bagian **Expert Advisors** $\rightarrow$ klik **Refresh**.
9. Buka chart instrumen yang diinginkan (misal: XAUUSD timeframe M5).
10. Drag bot dari Navigator ke chart, sesuaikan nilai pada tab **Inputs**, lalu klik **OK**.
11. Pastikan tombol **Algo Trading** di bar atas MetaTrader 5 aktif menyala hijau.

---

## ⚖️ DISCLAIMER & PERINGATAN RISIKO

Trading instrumen Forex, Emas (Gold / XAUUSD), dan CFD menggunakan bantuan robot (Expert Advisor) mengandung risiko kerugian finansial yang signifikan. Seluruh file dalam repository ini disediakan secara gratis untuk keperluan riset dan edukasi.

TENMA tidak bertanggung jawab atas kerugian finansial yang diakibatkan oleh kelalaian manajemen modal (*money management*), kesalahan konfigurasi parameter *Inputs*, koneksi VPS yang terputus, atau lonjakan volatilitas pasar yang ekstrem. **Gunakan risiko yang dapat Anda kelola secara bijak!**
