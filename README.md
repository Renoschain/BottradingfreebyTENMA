# 🤖 BOT TRADING FREE BY TENMA (Official Repository)

Selamat datang di repository resmi **Bot Trading Free by TENMA**. Repository ini menyediakan koleksi robot trading (Expert Advisor / EA) untuk platform **MetaTrader 5 (MT5)** yang telah dikompilasi ke dalam format biner (`.ex5`).

Seluruh kode biner di repository ini **100% aman dan terkunci** dari pencurian logika/resep algoritma, namun dapat **langsung dipasang dan dijalankan secara penuh** di akun demo maupun real MetaTrader 5 Anda.

---

## ⚠️ PERHATIAN PENTING SEBELUM MENJALANKAN BOT!

> [!IMPORTANT]
> **Performa setiap bot SANGAT BERGANTUNG pada cara Anda mengatur parameter di tab INPUTS!**
> 
> 1. **Tidak Ada Settingan Ajaib Otomatis:** Performa bot di market real ditentukan oleh kesesuaian settingan *Inputs* dengan modal (*equity*), leverage, spread broker, dan jenis instrumen (misal XAUUSD / Gold vs Forex Pair).
> 2. **Karakter & Kondisi Bot Beragam:** Bot di repository ini memiliki karakter yang sangat bervariasi—mulai dari tipe *Scalper Agresif*, *Sniper SMC*, *Trend Rider*, hingga *Grid Basket*. **Beberapa bot merupakan versi eksperimental/cacat yang memiliki kelemahan tertentu** (seperti risiko *drawdown* tinggi pada mode All-In atau sideways).
> 3. **Uji Coba Wajib:** Selalu jalankan bot terlebih dahulu di **Akun Demo** atau **Strategy Tester MT5** minimal 1–2 minggu sebelum menggunakannya di akun riil.

---

## 🛠️ CARA MENGATUR INPUTS DI METATRADER 5

Sebelum bot aktif di chart, Anda dapat dan **sangat disarankan** untuk menyesuaikan pengaturannya:

1. Buka MetaTrader 5, drag salah satu file `.ex5` dari panel **Navigator** (`Ctrl + N`) ke chart yang aktif.
2. Jendela pop-up pengaturan bot akan muncul.
3. Klik tab **Inputs** di bagian atas jendela.
4. Klik dua kali pada nilai parameter yang ingin diubah, masukkan angka yang sesuai dengan ketahanan modal Anda, lalu klik **OK**.
5. Pastikan tombol **Algo Trading** di toolbar atas MetaTrader 5 berwarna hijau (aktif).

```
+-----------------------------------------------------------+
|  Expert - BOTTENMA...                               [ _ X]|
+-----------------------------------------------------------+
|  [Common]  |  [INPUTS]  |  [Dependencies]                 |
+-----------------------------------------------------------+
|  Variable              |  Value                           |
|------------------------+----------------------------------|
|  InpFixedLot           |  0.01          <-- Atur lot      |
|  InpMaxLayers          |  10            <-- Batasi layer  |
|  InpBasketLossPct      |  3.0           <-- Stop kerugian |
|  InpDailyLossPct       |  5.0           <-- Pengaman akun |
+-----------------------------------------------------------+
```

---

## 📖 KAMUS LENGKAP PARAMETER INPUTS (Kegunaan & Cara Kerja)

Sebagian besar bot dalam repository ini berbagi struktur parameter *Inputs* yang serupa. Berikut adalah penjelasan mendalam tentang fungsi masing-masing parameter:

### 1. Pengaturan Lot & Layer (Sizing Management)
* **`InpFixedLot`**: Ukuran lot tetap per order (default umumnya `0.01`). Jika Anda ingin trading aman, gunakan `0.01` untuk modal di bawah $1,000.
* **`InpAutoLot`** (`true`/`false`): Jika diaktifkan, ukuran lot akan bertambah secara otomatis seiring bertumbuhnya equity akun.
* **`InpEquityPerMinLot`**: Nilai modal dasar untuk melipatgandakan lot otomatis (contoh: isi `1000.0` artinya tiap kelipatan equity $1,000, lot bertambah 0.01).
* **`InpAllIn`** (`true`/`false`): Mode compounding super agresif yang menghitung lot dari seluruh margin yang tersisa. **Hanya untuk akun modal kecil yang siap risiko 100%!**
* **`InpAllInMarginPct`**: Batas persentase equity yang boleh dijadikan margin jika seluruh layer terbuka (contoh: `50.0`%).
* **`InpMaxLayers`**: Jumlah maksimal posisi/order terbuka yang diizinkan aktif bersamaan. *Semakin kecil angka ini (misal 5-10), semakin aman akun Anda dari Margin Call.*
* **`InpBurst`**: Jumlah order yang ditembakkan secara serentak dalam satu detik/siklus (ciri khas scalper burst Tenma).
* **`InpEntryEverySec`**: Jeda waktu tunggu (dalam detik) antar eksekusi layer baru.
* **`InpLayerGapATR`**: Jarak minimal antar layer baru berdasarkan pergerakan volatilitas ATR (mencegah penumpukan posisi di harga yang sama).

### 2. Pengaturan Exit & Pengunci Keuntungan (Take Profit, SL & Trailing)
* **`InpTP_ATR` / `InpTP`**: Target Take Profit utama yang dihitung dari kelipatan volatilitas pasar (ATR M1/M5) atau dalam points.
* **`InpSL_ATR` / `InpSL`**: Batas Stop Loss per posisi. Menutup order otomatis jika harga melawan arah sejauh kelipatan ATR yang ditentukan.
* **`InpLockArm_ATR` / `InpBE_R`**: Titik aktivasi pengunci profit (*Break-Even Lock*). Saat floating profit sudah mencapai jarak ini, SL otomatis digeser ke titik impas/profit kecil.
* **`InpTrailGivebackPct`**: Fitur *Peak Trailing*. Menutup posisi jika profit yang sudah berjalan turun sekian persen dari titik tertingginya (contoh `30.0`% artinya jika profit sempat $10 lalu mundur ke $7, posisi langsung ditutup agar keuntungan tidak hilang).
* **`InpMaxHoldSec`**: *Time Stop* (detik). Menutup paksa posisi jika sudah terbuka melebihi durasi waktu tertentu, baik untung maupun rugi (sangat penting untuk scalper agar tidak menahan posisi floating terlalu lama).

### 3. Logika Pembalik Arah (Flip Direction & Basket Management)
* **`InpFlipMinSec`**: Waktu jeda minimal (detik) sebelum bot diizinkan mengubah arah trading dari BUY ke SELL atau sebaliknya.
* **`InpFlipCloseOld`**: Penanganan order lama saat arah dibalik:
  * `0`: Biarkan order lama berjalan.
  * `1`: Tutup hanya order arah lama yang sudah dalam kondisi profit (*Recommended*).
  * `2`: Tutup seluruh order arah lama tanpa terkecuali.
* **`InpBasketLossPct`**: **Fitur Penyelamat Akun (*Basket Cut*)**. Jika total kerugian mengambang (*floating loss*) seluruh order mencapai persentase ini dari modal (misal `-3.0%` equity), bot akan menutup SEMUA posisi sekaligus, membalik arah tren, dan memulai siklus baru dari awal.
* **`InpReboundDepthATR` & `InpReboundMinTP`**: Bot mendeteksi posisi yang sempat minus cukup dalam lalu berbalik arah menjadi profit; bot akan langsung mengamankan profit tersebut dan bersiap melakukan flip.

### 4. Proteksi Risiko & Keamanan Broker (Risk Guards)
* **`InpMaxSpread`**: Batas toleransi spread tertinggi yang diizinkan (dalam points). Bot akan **menolak entry** jika spread broker sedang melebar (misal saat berita high-impact atau pergantian hari/rollover).
* **`InpMaxMarginUse`**: Batas maksimal penggunaan free margin dalam persen.
* **`InpMinMarginLevel`**: Perlindungan ketahanan akun. Bot akan mogok membuka order baru jika Margin Level akun berada di bawah persentase ini (contoh: `< 300%`).
* **`InpDailyLossPct`**: Pengaman harian. Jika total kerugian dalam 1 hari mencapai sekian persen balance (misal `10.0%`), bot berhenti trading sampai keesokan harinya.
* **`InpEmergencyDD`**: **Tombol Darurat (*Kill-Switch*)**. Jika equity akun turun mencapai batas drawdown yang ditentukan dari puncak saldo tertinggi (misal `30.0%`), bot akan menutup SEMUA transaksi dan berhenti total.

### 5. Pengaturan Teknis Indikator
* **`InpATR_Period`**: Periode indikator Average True Range untuk mengukur rentang pergerakan lilin/candle (standar: `14`).
* **`InpEMA_Fast` & `InpEMA_Slow`**: Periode Exponential Moving Average untuk penentu arah tren dasar (contoh `20` dan `50`).
* **`InpADX_Period`, `InpTrendADX`, `InpSideADX`**: Pengukur kekuatan tren. Membedakan apakah pasar sedang kencang bergerak satu arah (*Trending*) atau sedang diam bolak-balik (*Sideways*).
* **`InpMagic`**: Angka pengenal unik transaksi (*Magic Number*). Pastikan setiap bot di chart berbeda memiliki nomor Magic yang berbeda agar tidak saling bentrok.

---

## 📋 KATALOG LENGKAP BOT TRADING TENMA (BOTTENMA1001 – BOTTENMA1024)

Berikut adalah daftar 24 bot trading yang tersedia di repository ini, dikelompokkan berdasarkan karakteristik logika, tingkat risiko, dan catatan kelemahannya:

| Kode Bot | Nama File (.ex5) | Kategori & Strategi | Level Risiko | Catatan Performa & Potensi Cacat |
| :--- | :--- | :--- | :--- | :--- |
| **BOTTENMA1001** | `ALLINGILAA01.ex5` | Flip Scalper v27 (Burst 0.01) | 🔴 Sangat Tinggi | Cepat melipatgandakan saldo kecil, rentan MC jika tren panjang tanpa koreksi. |
| **BOTTENMA1002** | `ALLINGILAAA02.ex5` | Flip Scalper v27 Rev (Tight Gap) | 🔴 Sangat Tinggi | Jarak layer lebih rapat, profit cepat di market volatile, rawan saat spread melebar. |
| **BOTTENMA1003** | `RENallingila01.ex5` | Flip Scalper v28 (Margin Filter) | 🔴 Sangat Tinggi | Ditambahkan filter margin level, namun burst tetap agresif. |
| **BOTTENMA1004** | `RENOallingila02.ex5` | Flip Scalper v28 Rev (Rebound Tune) | 🔴 Sangat Tinggi | Deteksi rebound ditingkatkan, proteksi slippage lebih ketat. |
| **BOTTENMA1005** | `ALLINCONPOUNDINGGILAAA01.ex5` | Flip Scalper v30 (Full Compounding) | 🔴 Ekstrem | Menggunakan All-In auto-margin. Potensi profit ratusan %, drawdown sangat brutal. |
| **BOTTENMA1006** | `allinconpoundinggilaaa02.ex5` | Flip Scalper v30 Rev (Basket Cut) | 🔴 Ekstrem | Revisi perbaikan trailing giveback dan pembatasan basket loss. |
| **BOTTENMA1007** | `Gold Hunter V9 4.0.ex5` | Multi-Module Hybrid (Gold M5) | 🟡 Sedang | 6 modul entry (SNR, Breakout, Retest, Fib, FVG, Pinbar). Performa bagus di Gold. |
| **BOTTENMA1008** | `RENO Gold Hunter V9 4.0.ex5` | Gold Hunter v35 Tuned Edition | 🟡 Sedang | Modul anti-konflik, pengetatan Stop Loss berbasis struktur chart & clamp ATR. |
| **BOTTENMA1009** | `RENOtrident16.ex5` | Sniper Compound v16 | 🟠 Agresif | **Catatan Cacat:** Winrate tinggi (~73%) tetapi average loss terlalu besar jika SL 1.2 ATR belum disesuaikan. Butuh setting SL lebih ketat. |
| **BOTTENMA1010** | `RENOtrident17.ex5` | Smart Money SMC Brain v17 | 🟡 Sedang | Membaca Order Block, FVG, dan BOS. Bagus di sesi London/NY, lemot di sesi Asia. |
| **BOTTENMA1011** | `RENOtrident18.ex5` | Sniper Compound v16 24H Rev | 🟠 Agresif | Filter sesi trading dihapus agar jalan 24 jam. Hati-hati saat pergantian hari/spread malam. |
| **BOTTENMA1012** | `RENOtrident19.ex5` | Aggressive Two-Way Scalp v19 | 🔴 Tinggi | **Catatan Cacat:** Cooldown 0 dan entry dua arah. Rentan terjebak floating lock jika harga bergerak searah dengan cepat. |
| **BOTTENMA1013** | `RENOtrident20.ex5` | Trend Rider + Sideways Basket v20 | 🟡 Sedang | Deteksi rezim pasar M5. Mode Trend pakai trailing lebar, mode sideways pakai grid basket. |
| **BOTTENMA1014** | `RENOtrident21.ex5` | Trend Rider + Grid Safety v21 | 🟡 Sedang | Peningkatan jarak step grid saat kondisi sideways agar tidak boros margin. |
| **BOTTENMA1015** | `RENOtrident22.ex5` | Pyramiding Trend Rider v22 | 🟠 Agresif | Menambah layer saat posisi trend sudah profit dan aman (SL di atas BE). |
| **BOTTENMA1016** | `RENOtrident23.ex5` | Lean Trident v23 (Low DD) | 🟢 Moderat | Versi yang disederhanakan untuk memangkas drawdown maksimal. |
| **BOTTENMA1017** | `RENOtrident24.ex5` | Hybrid Volatility Gate v24 | 🟢 Moderat | Dilengkapi pintu filter volatilitas: mematikan grid saat ATR melonjak drastis. |
| **BOTTENMA1018** | `RENOtrident25.ex5` | Strict Filter Trident v25 | 🟢 Konservatif | Mengutamakan keamanan akun, trade lebih jarang namun akurasi lebih terukur. |
| **BOTTENMA1019** | `RENOtrident32.02.ex5` | Confirmed-Direction Scalper v32 | 🟢 Konservatif | Sizing berbasis anggaran risiko (risk budget), pembalikan arah menggunakan State Machine. |
| **BOTTENMA1020** | `RENOtrident33.ex5` | Modern Flip Scalper v33 | 🟠 Agresif | Burst scalper modern dengan trailing TP dan eksekusi ultra cepat pada TF M1. |
| **BOTTENMA1021** | `RENOtrident35.ex5` | Dynamic ATR Flip Scalper v35 | 🟠 Agresif | Penyesuaian TP/SL adaptif mengikuti besarnya candle ATR real-time. |
| **BOTTENMA1022** | `RENOtrident36.ex5` | Multi-Timeframe Scalper v36 | 🟡 Sedang | Konfirmasi tren ganda (M1 + M5). Mengurangi salah arah pada saat pembalikan tren. |
| **BOTTENMA1023** | `RENOtrident37.ex5` | Spread Guard Flip Scalper v37 | 🟡 Sedang | Pengetatan filter spread dan time-stop untuk membatasi order basi. |
| **BOTTENMA1024** | `RENOtrident38.ex5` | Peak-Trailing Flip Scalper v38 | 🟡 Sedang | Versi paling mutakhir dari seri Flip Scalper: perbaikan proteksi drawdown & trailing puncak. |

*(Keterangan: Total bot biner yang tersedia di repository saat ini berjumlah 24 file `.ex5`. Jumlah 50 file pada commit awal adalah kalkulasi seluruh source code dan compiled file sebelum diamankan).*

---

## 💡 TIPS & REKOMENDASI PENGGUNAAN INPUTS

1. **Untuk Akun Modal Kecil ($50 – $200):**
   * Gunakan `BOTTENMA1016` (v23 Lean), `BOTTENMA1018` (v25), atau `BOTTENMA1019` (v32.02).
   * Set `InpFixedLot = 0.01`.
   * Matikan `InpAllIn` (`false`).
   * Batasi `InpMaxLayers = 3` sampai `5`.
   * Pasang `InpBasketLossPct = 5.0`% untuk mencegah kerugian fatal.

2. **Untuk Pecinta Scalping Cepat / Akun Sen (Compounding):**
   * Gunakan seri Flip Scalper (`BOTTENMA1005`, `BOTTENMA1020`, `BOTTENMA1024`).
   * Pastikan akun Anda bertipe **HEDGE** (bukan Netting).
   * Pasang di pair yang spread-nya sangat tipis (misal XAUUSD pada broker ECN / Zero Spread, EURUSD, atau GBPUSD).
   * Selalu aktifkan `InpBasketLossPct` (disarankan 3% – 5%) agar bot segera memotong kerugian jika arah salah.

3. **Untuk Trading XAUUSD (Gold):**
   * Gunakan `BOTTENMA1007` atau `BOTTENMA1008` (Gold Hunter).
   * Atur `InpMaxSpread` yang realistis untuk Gold (misal `400` – `600` points tergantung broker).
   * Gunakan timeframe **M5** atau **M15**.

---

## 🚀 CARA INSTALASI KE METATRADER 5

1. **Unduh** file `.ex5` pilihan Anda dari repository ini (klik file $\rightarrow$ klik tombol **Download raw file**).
2. Buka aplikasi **MetaTrader 5** di PC/Laptop/VPS Anda.
3. Klik menu **File** $\rightarrow$ pilih **Open Data Folder**.
4. Masuk ke folder: `MQL5` $\rightarrow$ `Experts`.
5. **Paste (tempel)** file `.ex5` ke dalam folder tersebut.
6. Kembali ke MetaTrader 5, buka panel **Navigator** (tekan `Ctrl + N`).
7. Klik kanan pada folder **Expert Advisors** $\rightarrow$ pilih **Refresh**.
8. Buka chart instrumen (misal XAUUSD M5), lalu drag bot dari Navigator ke chart.
9. Sesuaikan settingan di tab **Inputs**, lalu klik **OK**.
10. Pastikan tombol **Algo Trading** di bagian atas MetaTrader 5 menyala hijau.

---

## ⚖️ DISCLAIMER & PERINGATAN RISIKO

Trading instrumen Forex, Emas (Gold/XAUUSD), dan CFD menggunakan bantuan robot/Expert Advisor memiliki potensi keuntungan sekaligus risiko kerugian modal yang tinggi. Seluruh file dalam repository ini dibagikan secara gratis sebagai materi riset dan pembelajaran. 

Pengembang (TENMA) tidak bertanggung jawab atas segala bentuk kerugian finansial yang timbul akibat kesalahan pengaturan parameter (*inputs*), kelalaian manajemen modal (*money management*), ketidakstabilan koneksi VPS, atau volatilitas pasar yang ekstrem. **Gunakan risiko yang dapat Anda tanggung secara bijak!**
