# Laporan Tugas Personal 2 — Blockchain Level 6–9

- **Nama:** KEVIN JULIUS SALENUSSA
- **NIM (4 digit terakhir):** 2056
- **Dibuat:** 2026-11-15 19:29  (waktu proses 0.1 detik)
- **Kode verifikasi:** `0C49DBD6BED7`

> Bagian hasil dibuat otomatis. Jangan diubah: dosen menghitung ulang semua angka dari data Anda.
> Isi bagian **Analisis** dengan kata-kata Anda sendiri.

## Level 6 — Mining data pribadi dan jejak difficulty

Data yang ditambang: `KEVIN JULIUS SALENUSSA|2056|2003-01-01`

| Difficulty | Nonce | Percobaan | Teori 16^d | Hash |
|---|---|---|---|---|
| 1 | 15 | 16 | 16 | `02f9e065a2c44f580d82…` |
| 2 | 372 | 373 | 256 | `005005fa1199d144778c…` |
| 3 | 816 | 817 | 4.096 | `000bbd2ccb2482cb8001…` |
| 4 | 62929 | 62.930 | 65.536 | `0000085175cfcbf87f9d…` |

- Difficulty awal (dari NIM): **1**, target waktu blok 10 detik
- Waktu blok simulasi (dari NIM + tanggal + bulan lahir): 8, 0, 20, 24, 0, 4, 0, 4 detik
- Jejak difficulty (Modifikasi 6): **1, 2, 2, 1, 2, 3, 4, 5**

## Level 7 — Kunci dan tanda tangan dari data pribadi

- Kunci publik (32 karakter awal): `ac1bf4a9ca463896b55d37c0a4ddf246…`
- Alamat Anda: `0239e04fd61a64a8d09b4045a178790e6ce533f3`
- Transaksi: kirim **1** ke TEMAN_A
- Tanda tangan: `8ebe4de0962b970014390e3a26227496afa750464bbc2542…`
- Tanda tangan selalu sama saat diulang: True
- Transaksi sah: **True**; setelah amount diubah: **False**
- Penipu memakai alamat Anda: tanda tangannya valid dengan kuncinya = True, transaksinya sah = **False**

## Level 8 — Jaringan dan fork pribadi

- Jumlah node: **4**; blok pribadi tiap node: [3, 1, 3, 1]
- Panjang rantai saat fork: [5, 3, 5, 3]
- Setelah resolve_conflicts: [5, 5, 5, 5], sepakat = **False**
- Setelah blok pemecah seri: [6, 6, 6, 6], sepakat = **True**
- Pemenang fork: **KEVIN-1**; blok terbuang (orphan): **5**
- Hash ujung rantai: `0008b00d86985f67578aded9…`

## Level 9 — Ekonomi dengan halving

- Hadiah awal: **36**, halving setiap **3** blok
- Hadiah blok 1–6 (Modifikasi 9): **36, 36, 36, 18, 18, 18**

| Alamat | Saldo |
|---|---|
| KEVIN JULIUS SALENUSSA | 105.0 |
| TEMAN_A | 31.0 |
| TEMAN_B | 26.0 |

- Total suplai: **162.0**
- Belanja melebihi saldo ditolak: True
- Validasi ekonomi: **True**; setelah hadiah blok 1 dicurangi: **False**

## Analisis (isi dengan kata-kata Anda sendiri)

1. **Level 6.** Bandingkan kolom Percobaan dengan Teori 16^d pada tabel Anda. Mengapa bisa berbeda jauh? Lalu jelaskan jejak difficulty Anda (1, 2, 2, 1, 2, 3, 4, 5): pada langkah mana difficulty naik, tetap, dan turun, dan mengapa?

   _Jawaban:_
   - **Perbandingan Percobaan vs Teori $16^d$:**
     Nilai Teori $16^d$ merupakan nilai harapan rata-rata matematis (*expected attempts*) dari proses Bernoulli berbasis fungsi hash SHA-256. Setiap digit heksadesimal memiliki 16 kemungkinan (0-f), sehingga peluang mendapatkan karakter '0' di depan adalah $(1/16)^d$. Pada penambangan blok tunggal secara individual, proses penemuan nonce adalah fenomena probabilistik murni:
     - Difficulty 1: Teori = 16, Percobaan = 16 (persis sama dengan nilai teoretis $16^1$).
     - Difficulty 2: Teori = 256, Percobaan = 373 (sedikit di atas rata-rata karena variasi nonce yang dibutuhkan lebih banyak).
     - Difficulty 3: Teori = 4.096, Percobaan = 817 (penambangan sangat beruntung, menemukan nonce valid hanya dalam 817 percobaan).
     - Difficulty 4: Teori = 65.536, Percobaan = 62.930 (sangat mendekati ekspektasi rata-rata teoretis).
     Secara agregat, jika penambangan diulang dalam ribuan kali percobaan, nilai rata-rata empiris akan selalu konvergen menuju $16^d$.
   - **Jejak Difficulty (1, 2, 2, 1, 2, 3, 4, 5):**
     Aturan penyesuaian difficulty dengan target waktu blok 10 detik adalah:
     - Waktu aktual < 5 detik (target / 2) $\rightarrow$ Difficulty naik (+1) karena blok ditambang terlalu cepat.
     - Waktu aktual > 20 detik (target * 2) $\rightarrow$ Difficulty turun (-1) karena penambangan terlalu lambat.
     - Waktu aktual antara 5 hingga 20 detik $\rightarrow$ Difficulty tetap.
     Dengan deret waktu simulasi pribadi saya `[8, 0, 20, 24, 0, 4, 0, 4]` detik dan difficulty awal 1:
     1. Langkah 1 (waktu 8s): Di antara 5s dan 20s $\rightarrow$ **Tetap di 1**.
     2. Langkah 2 (waktu 0s): $0 < 5$s (terlalu cepat) $\rightarrow$ **Naik menjadi 2**.
     3. Langkah 3 (waktu 20s): Di antara 5s dan 20s $\rightarrow$ **Tetap di 2**.
     4. Langkah 4 (waktu 24s): $24 > 20$s (terlalu lambat) $\rightarrow$ **Turun menjadi 1**.
     5. Langkah 5 (waktu 0s): $0 < 5$s (terlalu cepat) $\rightarrow$ **Naik menjadi 2**.
     6. Langkah 6 (waktu 4s): $4 < 5$s (terlalu cepat) $\rightarrow$ **Naik menjadi 3**.
     7. Langkah 7 (waktu 0s): $0 < 5$s (terlalu cepat) $\rightarrow$ **Naik menjadi 4**.
     8. Langkah 8 (waktu 4s): $4 < 5$s (terlalu cepat) $\rightarrow$ **Naik menjadi 5**.

2. **Level 7.** Mengapa tanda tangan Anda selalu sama setiap kali program dijalankan, padahal di Level 7 asli berubah-ubah? Mengapa transaksi penipu tidak sah walaupun tanda tangannya valid?

   _Jawaban:_
   - **Mengapa Tanda Tangan Selalu Sama:**
     Pada implementasi ECDSA standar di Level 7 asli, algoritma menggunakan generator bilangan acak sistem untuk memilih nilai rahasia sementara (*ephemeral key* $k$). Hal ini membuat tanda tangan selalu menghasilkan pasangan $(r, s)$ yang berbeda di setiap penjalanan. Namun pada Modifikasi 7, digunakan standar **RFC 6979** melalui fungsi `sign_deterministic`. Standar ini menurunkan nilai $k$ secara deterministik dari HMAC hash pesan transaksi dan kunci privat. Karena kunci privat dan data transaksi saya selalu sama di setiap eksekusi, nilai $k$ dan hasil tanda tangan digital heksadesimalnya selalu identik 100%.
   - **Mengapa Transaksi Penipu Tidak Sah:**
     Tanda tangan penipu secara matematis valid terhadap kunci publiknya sendiri (`verifikasi == True`) karena penipu memang menandatangani transaksi tersebut menggunakan kunci privat miliknya. Namun, fungsi konsensus `transaksi_sah` menerapkan verifikasi kepemilikan alamat:
     `alamat_dari_kunci_publik(kunci_publik) == tx["sender"]`
     Penipu mencantumkan alamat saya di kolom `sender`, tetapi menggunakan kunci publik miliknya. Karena penurunan kunci publik penipu menghasilkan alamat penipu dan bukan alamat saya yang tertera pada `sender`, transaksi ditolak sebagai **TIDAK SAH** (`False`).

3. **Keamanan kunci.** Kunci privat Anda dibuat dari nama, NIM, dan tanggal lahir. Mengapa cara ini SANGAT BERBAHAYA di dunia nyata? Siapa saja yang bisa mencuri dana alamat Anda? (Cari tahu istilah *brain wallet*.)

   _Jawaban:_
   - **Bahaya Brain Wallet dan Entropi Rendah:**
     Pembuatan kunci privat dari teks atau informasi yang mudah ditebak dikenal sebagai **Brain Wallet**. Cara ini sangat berbahaya karena ruang pencarian (*keyspace*) dari data identitas pribadi memiliki entropi yang sangat rendah. Kunci privat kriptografi seharusnya memiliki entropi 256 bit murni ($2^{256}$ kemungkinan acak tak tertebak).
   - **Pihak yang Berpotensi Mencuri Dana:**
     1. Siapa pun yang mengetahui data pribadi mahasiswa (teman sekelas, dosen, staf akademik kampus) atau yang mengakses basis data publik mahasiswa seperti PDDikti.
     2. Program otomatis peretas di blockchain publik (*sweeper bots*) yang menjalankan serangan kamus (*dictionary attacks*) berisi kombinasi nama umum, tanggal lahir, dan nomor induk untuk menguras saldo alamat mana pun yang cocok secara otomatis.

4. **Level 8.** Jaringan Anda punya 4 node dan pemenangnya KEVIN-1 dengan 5 blok terbuang. Jelaskan mengapa node itu menang. Mengapa jaringan Anda sempat tidak sepakat, dan bagaimana blok pemecah seri menyelesaikannya?

   _Jawaban:_
   - **Mengapa KEVIN-1 Menang:**
     Pada jaringan simulasi fork, terdapat 4 node yang menambang blok privat. Node KEVIN-1 menambang 3 blok (panjang 5), KEVIN-2 menambang 1 blok (panjang 3), KEVIN-3 menambang 3 blok (panjang 5), dan KEVIN-4 menambang 1 blok (panjang 3). Berdasarkan aturan konsensus Nakamoto (*Longest Chain Rule*), node selalu mengadopsi rantai terpanjang yang valid. Karena KEVIN-1 pada akhirnya menjadi node dengan rantai terpanjang, node-node lain mengadopsi rantainya dan membuang blok pribadi mereka. Total blok pribadi milik KEVIN-2 (1 blok), KEVIN-3 (3 blok), dan KEVIN-4 (1 blok) gugur menjadi **blok yatim (*orphan blocks*)**, yaitu total $1 + 3 + 1 = 5$ blok yang terbuang.
   - **Penyebab Jaringan Sempat Tidak Sepakat (*Tie Condition*):**
     Saat awal terjadi fork, node KEVIN-1 dan KEVIN-3 sama-sama memiliki panjang rantai 5 (`panjang_saat_fork: [5, 3, 5, 3]`). Ketika `resolve_conflicts` dijalankan, KEVIN-1 tidak mengganti rantainya ke KEVIN-3 karena panjangnya sama (tidak lebih panjang), dan sebaliknya KEVIN-3 juga mempertahankan rantainya sendiri. Akibatnya jaringan terbelah dua (*network split / tie*) sehingga `sepakat` bernilai `False`.
   - **Penyelesaian oleh Blok Pemecah Seri (*Tie-Breaker*):**
     Kondisi seri ini diselesaikan secara otomatis dengan menambang 1 blok baru di atas salah satu cabang (KEVIN-1). Penambahan blok ini membuat panjang rantai KEVIN-1 naik menjadi 6 (`[6, 6, 6, 6]`), menjadikannya definitif lebih panjang dari cabang KEVIN-3 (yang masih panjang 5). Seluruh node di jaringan langsung mematuhi *Longest Chain Rule* dan beralih mengadopsi rantai KEVIN-1, sehingga konsensus bulat 100% tercapai (`sepakat = True`).

5. **Level 9.** Dengan hadiah awal 36 dan halving setiap 3 blok, berapa batas total suplai jika mining berjalan selamanya? Tunjukkan perhitungannya, lalu kaitkan dengan batas 21 juta Bitcoin.

   _Jawaban:_
   - **Perhitungan Batas Total Suplai:**
     Dengan hadiah awal $H_0 = 36$ dan interval halving $I = 3$ blok menggunakan pembagian bilangan bulat (*integer division* `//`):
     - Periode 0 (Blok 1–3)  : Hadiah = 36 $\rightarrow 3 \times 36 = 108$ koin
     - Periode 1 (Blok 4–6)  : Hadiah = 18 ($36 // 2 = 18$) $\rightarrow 3 \times 18 = 54$ koin
     - Periode 2 (Blok 7–9)  : Hadiah = 9  ($18 // 2 = 9$)  $\rightarrow 3 \times 9 = 27$ koin
     - Periode 3 (Blok 10–12): Hadiah = 4  ($9 // 2 = 4$)   $\rightarrow 3 \times 4 = 12$ koin
     - Periode 4 (Blok 13–15): Hadiah = 2  ($4 // 2 = 2$)   $\rightarrow 3 \times 2 = 6$ koin
     - Periode 5 (Blok 16–18): Hadiah = 1  ($2 // 2 = 1$)   $\rightarrow 3 \times 1 = 3$ koin
     - Periode 6 (Blok 19–dst): Hadiah = 0  ($1 // 2 = 0$)   $\rightarrow$ Emisi koin berhenti selamanya.
     - **Total Suplai Riil Sistem:**
       $$\text{Total Suplai} = 108 + 54 + 27 + 12 + 6 + 3 = 210 \text{ koin}$$
     - **Secara Teoretis Deret Geometri Tak Hingga:**
       $$\text{Suplai Teoretis} = I \times H_0 \times \sum_{k=0}^{\infty} \left(\frac{1}{2}\right)^k = 3 \times 36 \times 2 = 216 \text{ koin}$$
       (Perbedaan 6 koin terjadi akibat pemotongan desimal bilangan bulat pada nilai ganjil 9, 3, dan 1).
   - **Kaitannya dengan Batas 21 Juta Bitcoin:**
     Prinsip ini mencerminkan mekanisme moneter Bitcoin yang menerapkan hadiah awal 50 BTC dengan interval halving setiap 210.000 blok. Melalui formula deret geometri $210.000 \times 50 \times 2 = 21.000.000$ BTC, Bitcoin menjamin kelangkaan terprogram (*digital scarcity*) dan mencegah devaluasi inflasioner.

---

## Data verifikasi (JANGAN DIUBAH)

<!-- DATA-VERIFIKASI -->
```json
{
  "tugas": "personal-2 (Level 6-9)",
  "profil": {
    "nama": "KEVIN JULIUS SALENUSSA",
    "nim4": "2056",
    "tanggal_lahir": "2003-01-01",
    "kota_asal": "Sorong",
    "hobi": "Futsal"
  },
  "level6": {
    "data_ditambang": "KEVIN JULIUS SALENUSSA|2056|2003-01-01",
    "mining": [
      {
        "difficulty": 1,
        "nonce": 15,
        "hash": "02f9e065a2c44f580d822fe97dbe1279d61c05d86c45e5f68019ce59052b6dcf",
        "percobaan": 16,
        "teori": 16
      },
      {
        "difficulty": 2,
        "nonce": 372,
        "hash": "005005fa1199d144778c7198e0068e0abbb634c08493ef35c22c8b0d9ebf4d40",
        "percobaan": 373,
        "teori": 256
      },
      {
        "difficulty": 3,
        "nonce": 816,
        "hash": "000bbd2ccb2482cb8001937cdbe0735dbe148ffaefe8d3172bb9c2c4b3297aa7",
        "percobaan": 817,
        "teori": 4096
      },
      {
        "difficulty": 4,
        "nonce": 62929,
        "hash": "0000085175cfcbf87f9da37ac1daefab81415663fcce87f0d9fffb102c39f338",
        "percobaan": 62930,
        "teori": 65536
      }
    ],
    "difficulty_awal": 1,
    "daftar_waktu_detik": [
      8,
      0,
      20,
      24,
      0,
      4,
      0,
      4
    ],
    "target_detik": 10,
    "jejak_difficulty": [
      1,
      2,
      2,
      1,
      2,
      3,
      4,
      5
    ]
  },
  "level7": {
    "kunci_publik_awal": "ac1bf4a9ca463896b55d37c0a4ddf246",
    "alamat": "0239e04fd61a64a8d09b4045a178790e6ce533f3",
    "transaksi": {
      "sender": "0239e04fd61a64a8d09b4045a178790e6ce533f3",
      "recipient": "TEMAN_A",
      "amount": 1
    },
    "tanda_tangan": "8ebe4de0962b970014390e3a26227496afa750464bbc25422c3ec422faa7ffc49ef2c3afc2ee1d140d0653a0ebb1a62876a2daa8cb91b7e3ff569490dda21451",
    "tanda_tangan_selalu_sama": true,
    "sah": true,
    "sah_setelah_amount_diubah": false,
    "ttd_penipu_valid_dengan_kuncinya": true,
    "transaksi_penipu_sah": false
  },
  "level8": {
    "jumlah_node": 4,
    "blok_pribadi_per_node": [
      3,
      1,
      3,
      1
    ],
    "panjang_saat_fork": [
      5,
      3,
      5,
      3
    ],
    "panjang_setelah_resolve": [
      5,
      5,
      5,
      5
    ],
    "sepakat_setelah_resolve": false,
    "perlu_pemecah_seri": true,
    "panjang_setelah_pemecah_seri": [
      6,
      6,
      6,
      6
    ],
    "sepakat_setelah_pemecah_seri": true,
    "pemenang_fork": "KEVIN-1",
    "blok_terbuang": 5,
    "hash_ujung": "0008b00d86985f67578aded9724982df4140aef0f27d909c014125c06cd2d032"
  },
  "level9": {
    "hadiah_awal": 36,
    "interval_halving": 3,
    "hadiah_per_blok": [
      36,
      36,
      36,
      18,
      18,
      18
    ],
    "saldo": {
      "KEVIN JULIUS SALENUSSA": 105.0,
      "TEMAN_A": 31.0,
      "TEMAN_B": 26.0
    },
    "total_suplai": 162.0,
    "belanja_melebihi_saldo_ditolak": true,
    "validasi_ekonomi": true,
    "hash_ujung": "003b526d50d9830ab27c9d5dfb197ce9e43e1859fd034a86ea6764b077100e3f",
    "validasi_setelah_hadiah_dicurangi": false
  },
  "kode_verifikasi": "0C49DBD6BED7"
}
```
