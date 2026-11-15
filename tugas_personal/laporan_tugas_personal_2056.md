# Laporan Tugas Personal Blockchain Level 1–5

- **Nama:** KEVIN JULIUS SALENUSSA
- **NIM (4 digit terakhir):** 2056
- **Dibuat:** 2026-11-15 19:29  (waktu proses 1.8 detik)
- **Kode verifikasi:** `5DF47ADEE8CA`

> Bagian hasil dibuat otomatis. Jangan diubah: dosen menghitung ulang semua angka dari data Anda.
> Isi bagian **Analisis** dengan kata-kata Anda sendiri.

## Level 1 — Hash data pribadi

| Hasil | Nilai |
|---|---|
| SHA-256 nama | `456c5f8e273253a6630e484d0480f8819bdd11fa074e9c5413450a4059afbdb6` |
| hash_dict profil | `7848db8a637160b8f08fb38f348ad5ed7c30b959c2285e7861026f3dc92dc77f` |
| hash_dict profil (urutan key dibalik) | `7848db8a637160b8f08fb38f348ad5ed7c30b959c2285e7861026f3dc92dc77f` |
| Bit berubah: tanggal lahir vs tanggal lahir + 1 hari | 46.88% |
| ID mahasiswa (Modifikasi 1) | `90d98552` |

## Level 2 — Blockchain personal

- Proof genesis: **2056** (dari NIM)
- Proof blok 2, 3, 4: **6312, 4001, 75128**
- Pemilik setiap blok: `90d98552`
- Rantai valid: True

## Level 3 — Pemalsuan data pribadi

- Posisi yang dipalsukan: **1**
- Data asli: `KEVIN JULIUS SALENUSSA lahir 2003-01-01`
- Data palsu: `KEVIN JULIUS SALENUSSA lahir 2003-01-01 (DIPALSUKAN)`
- Valid sebelum dipalsukan: True
- Tautan putus di indeks: **2**
- Valid setelah dipalsukan: **False**
- Valid setelah disambung ulang: **True**

## Level 4 — Transaksi dari data pribadi

| Blok | Pengirim | Penerima | Jumlah |
|---|---|---|---|
| 2 | KEVIN JULIUS SALENUSSA | TEMAN_A | 1 |
| 2 | TEMAN_A | KEVIN JULIUS SALENUSSA | 1 |
| 3 | KEVIN JULIUS SALENUSSA | DOSEN | 7 |

- Ringkasan saldo (Modifikasi 4): masuk **1**, keluar **8**, bersih **-7**
- Kirim ke diri sendiri ditolak: True
- Amount 0 ditolak: True

## Level 5 — Proof of Work dengan nonce (deterministik)

| Blok | Nonce | Hash |
|---|---|---|
| 0 | 62227 | `000080358dcce46daabb716b…` |
| 1 | 6543 | `0000fdf5b0a5201e1238f982…` |
| 2 | 3433 | `0000a6b7c4b15299ab59be21…` |
| 3 | 9723 | `00004abab8a39d243aad6d23…` |

- Valid: True → setelah blok 1 dipalsukan: **False**
- Percobaan hash untuk menambang ulang blok 1–3: **348.963**
- Valid setelah ditambang ulang: True

## Analisis (isi dengan kata-kata Anda sendiri)

1. **Level 1.** Bandingkan SHA-256 nama Anda dengan milik seorang teman. Apakah ada pola yang mirip? Jelaskan juga mengapa dua hash profil Anda sama, dan mengapa hanya menambah 1 hari pada tanggal lahir mengubah 46.88% bit.

   _Jawaban:_
   - **Perbandingan dengan milik teman:** Nilai SHA-256 nama saya (`456c5f8e273253a6630e484d0480f8819bdd11fa074e9c5413450a4059afbdb6`) sama sekali tidak menunjukkan pola kemiripan visual dengan milik teman. Fungsi hash SHA-256 menghasilkan keluaran terdistribusi merata (*uniform distribution*) secara acak semu (*pseudorandom*), sehingga tidak ada korelasi visual antara susunan huruf nama dengan nilai hash heksadesimal yang dihasilkan.
   - **Dua hash profil bernilai sama:** Nilai hash profil normal dan profil dengan urutan key terbalik bernilai identik (`7848db8a6371...`) karena implementasi fungsi `hash_dict` menggunakan serialisasi kanonik `json.dumps(data, sort_keys=True)`. Sebelum dilakukan proses hashing ke byte, seluruh key diurutkan secara alfabetis terlebih dahulu. Dengan demikian, urutan penulisan field dalam dictionary di memori tidak mengubah representasi string JSON yang dihasilkan.
   - **Perubahan 1 hari mengubah 46.88% bit:** Fenomena ini merupakan karakteristik mendasar fungsi hash kriptografis yang disebut *Avalanche Effect* (efek longsoran salju). Mengubah hanya satu digit pada tanggal lahir (selisih 1 hari) merombak keadaan internal pada setiap putaran (*round*) kompresi SHA-256, menghasilkan pembalikan bit acak sebanyak 46.88% (120 dari 256 bit), yang sangat konsisten dengan probabilitas teoretis 50%.

2. **Level 2.** Kode Anda dan teman sama persis, tetapi proof Anda (6312, 4001, 75128) berbeda. Jelaskan penyebabnya.

   _Jawaban:_
   - Perbedaan ini terjadi karena **proof blok genesis** diinisialisasi secara personal menggunakan 4 digit terakhir NIM masing-masing (`int(nim4)`). Pada blockchain saya, proof genesis bernilai `2056` (dari NIM 202255202056), sedangkan teman-teman lain memiliki nilai awal yang berbeda.
   - Pada mekanisme Proof of Work Level 2, pencarian angka pembuktian kerja baru dihitung berdasarkan relasi `proof**2 - previous_proof**2`. Karena blok ke-2 menggunakan `previous_proof = 2056`, nilai `proof` yang menghasilkan hash valid dengan awalan nol sesuai difficulty adalah `6312`.
   - Selanjutnya, blok ke-3 menjadikan `6312` sebagai dasar perhitungannya dan menghasilkan `4001`, kemudian blok ke-4 menghasilkan `75128`. Akibat perbedaan benih awal (*seed*) pada genesis, seluruh deret nilai proof blok berikutnya menjadi unik dan berbeda untuk setiap mahasiswa.

3. **Level 3.** Anda memalsukan posisi 1 dan tautan putus terdeteksi di indeks 2, sedangkan valid setelah dipalsukan = False. Jelaskan hasil ini, lalu bandingkan dengan teman yang posisi pemalsuannya berbeda.

   _Jawaban:_
   - **Penyebab Tautan Putus di Indeks 2 dan Valid = False:**
     Posisi pemalsuan dihitung dari formula `1 + int(nim4) % 4`. Untuk NIM `2056`, sisa baginya adalah `2056 % 4 = 0`, sehingga posisi yang dipalsukan adalah indeks `1 + 0 = 1`. Ketika data pada blok indeks 1 dimanipulasi dengan menambahkan teks `(DIPALSUKAN)`, hash blok 1 berubah total. Blok penerusnya (indeks 2) masih menyimpan pointer `previous_hash` yang menunjuk ke hash lama blok 1. Saat validasi memeriksa blok 2, ketidakcocokan pointer terdeteksi (`chain[2]['previous_hash'] != hash_blok(chain[1])`), sehingga sistem mendeteksi tautan putus tepat di posisi 2 dan status validitas rantai menjadi `False`.
   - **Perbandingan dengan Teman:**
     Teman-teman yang sisa bagi NIM-nya berbeda memalsukan blok pada posisi lain (posisi 2, 3, atau 4). Jika teman memalsukan blok indeks 2, maka tautan putus terdeteksi di indeks 3. Khusus teman yang memalsukan blok terakhir (posisi 4), fungsi `cari_tautan_putus` tidak mendeteksi kerusakan tautan karena tidak ada blok berikutnya yang menunjuk ke blok tersebut.
   - **Kelemahan PoW GfG:**
     Setelah dijalankan `sambung_ulang`, status rantai palsu kembali menjadi `True`. Hal ini membuktikan bahwa algoritma PoW ala GeeksforGeeks memiliki kelemahan mendasar: PoW-nya tidak mengikat data transaksi atau pointer hash, sehingga penyerang dapat memalsukan isi blok dan menyambung ulang hash pointer tanpa perlu menambang ulang proof-nya.

4. **Level 4.** Jelaskan dari mana angka masuk 1, keluar 8, dan bersih -7 berasal. Mengapa transaksi ke diri sendiri perlu ditolak?

   _Jawaban:_
   - **Asal Usul Angka Saldo:**
     1. Transaksi 1: `KEVIN JULIUS SALENUSSA` mengirim ke `TEMAN_A` sebesar tanggal lahir saya (tanggal 1 Januari), yaitu **1** koin $\rightarrow$ keluar 1 koin.
     2. Transaksi 2: `TEMAN_A` mengirim ke `KEVIN JULIUS SALENUSSA` sebesar bulan lahir saya (bulan 1 / Januari), yaitu **1** koin $\rightarrow$ masuk 1 koin.
     3. Transaksi 3: `KEVIN JULIUS SALENUSSA` mengirim ke `DOSEN` sebesar `(int(nim4) % 50) + 1` = `(2056 % 50) + 1` = `6 + 1` = **7** koin $\rightarrow$ keluar 7 koin.
     - **Total Masuk:** 1 koin.
     - **Total Keluar:** $1 + 7 = 8$ koin.
     - **Saldo Bersih:** $1 - 8 = -7$ koin.
   - **Alasan Transaksi ke Diri Sendiri Harus Ditolak:**
     1. *Mencegah Pemborosan Ruang Blok:* Transaksi ke diri sendiri tidak mengubah saldo bersih maupun kepemilikan aset, tetapi tetap mengonsumsi kapasitas blok dan membebani node jaringan.
     2. *Mencegah Manipulasi Volume (Wash Trading):* Mencegah upaya menciptakan ilusi transaksi fiktif demi memanipulasi aktivitas pasar.
     3. *Efisiensi Komputasi Validator:* Menghemat sumber daya komputasi validator jaringan dari memproses transaksi tanpa nilai ekonomi riil.

5. **Level 5.** Penyerang butuh 348.963 percobaan hash untuk memalsukan rantai Anda. Mengapa nonce Anda selalu sama setiap kali program dijalankan, padahal di demo Level 5 asli nonce berubah-ubah?

   _Jawaban:_
   - Pada simulasi Modifikasi 5, penambangan Proof of Work dijalankan secara **deterministik**: iterasi pencarian selalu dimulai dari angka tetap `nonce = 0` dan bertambah secara sekuensial (`nonce += 1`). Karena seluruh data blok (indeks, transaksi, previous_hash, dan timestamp yang diseragamkan) bernilai tetap di setiap pengujian, fungsi SHA-256 yang bersifat deterministik murni akan selalu menemukan kombinasi hash yang memenuhi awalan `'0000'` pada angka nonce yang persis sama di setiap eksekusi.
   - Pada demo Level 5 asli, timestamp diambil secara dinamis dari jam sistem riil (`time.time()`), sehingga data input blok selalu berubah setiap milidetik, menyebabkan nilai hash blok berubah dan angka nonce yang ditemukan selalu bervariasi.

6. **Etika data.** Anda baru saja menyimpan nama dan tanggal lahir di dalam blockchain. Jika ini blockchain publik, apa risikonya? Kaitkan dengan sifat immutability dan UU No. 27 Tahun 2022 tentang Pelindungan Data Pribadi, terutama hak untuk menghapus data.

   _Jawaban:_
   - **Risiko Privasi Data Pribadi:**
     Blockchain publik bersifat terbuka dan dapat diinspeksi oleh siapa saja secara global. Menyimpan Personally Identifiable Information (PII) seperti nama lengkap, NIM, dan tanggal lahir secara terbuka membuka celah eksploitasi pencurian identitas, profiling data tanpa izin, dan kejahatan siber berbasis rekayasa sosial.
   - **Benturan dengan UU PDP No. 27 Tahun 2022:**
     Sifat *immutability* (data permanen dan tidak dapat dihapus/diubah) bertentangan langsung dengan Pasal 43 UU PDP yang memberikan hak kepada pemilik data untuk meminta penghapusan atau pemusnahan data pribadi miliknya (*Right to Erasure / Right to be Forgotten*). Sekali data pribadi masuk ke blok yang tertambang, data tersebut secara teknis tidak mungkin dihapus tanpa merusak seluruh rantai blockchain.
   - **Solusi Rekayasa:**
     Data sensitif harus disimpan di luar rantai (*off-chain*) dalam media penyimpanan terenkripsi, sementara blockchain hanya digunakan untuk menyimpan bukti kriptografis berupa nilai hash atau komitmen identitas tanpa membuka data aslinya.

---

## Data verifikasi (JANGAN DIUBAH)

<!-- DATA-VERIFIKASI -->
```json
{
  "profil": {
    "nama": "KEVIN JULIUS SALENUSSA",
    "nim4": "2056",
    "tanggal_lahir": "2003-01-01",
    "kota_asal": "Sorong",
    "hobi": "Futsal"
  },
  "level1": {
    "hash_nama": "456c5f8e273253a6630e484d0480f8819bdd11fa074e9c5413450a4059afbdb6",
    "hash_profil": "7848db8a637160b8f08fb38f348ad5ed7c30b959c2285e7861026f3dc92dc77f",
    "hash_profil_urutan_terbalik": "7848db8a637160b8f08fb38f348ad5ed7c30b959c2285e7861026f3dc92dc77f",
    "avalanche_tanggal_persen": 46.88,
    "id_mahasiswa": "90d98552"
  },
  "level2": {
    "proof_genesis": 2056,
    "proof_blok": [
      6312,
      4001,
      75128
    ],
    "pemilik_semua_blok": [
      "90d98552"
    ],
    "valid": true
  },
  "level3": {
    "posisi_dipalsukan": 1,
    "data_asli": "KEVIN JULIUS SALENUSSA lahir 2003-01-01",
    "data_palsu": "KEVIN JULIUS SALENUSSA lahir 2003-01-01 (DIPALSUKAN)",
    "valid_asli": true,
    "tautan_putus_di": 2,
    "valid_setelah_dipalsukan": false,
    "valid_setelah_disambung_ulang": true
  },
  "level4": {
    "riwayat": [
      [
        2,
        {
          "sender": "KEVIN JULIUS SALENUSSA",
          "recipient": "TEMAN_A",
          "amount": 1
        }
      ],
      [
        2,
        {
          "sender": "TEMAN_A",
          "recipient": "KEVIN JULIUS SALENUSSA",
          "amount": 1
        }
      ],
      [
        3,
        {
          "sender": "KEVIN JULIUS SALENUSSA",
          "recipient": "DOSEN",
          "amount": 7
        }
      ]
    ],
    "ringkasan_saldo": {
      "masuk": 1,
      "keluar": 8,
      "bersih": -7
    },
    "kirim_ke_diri_sendiri_ditolak": true,
    "amount_nol_ditolak": true,
    "jumlah_blok": 3
  },
  "level5": {
    "nonce": [
      62227,
      6543,
      3433,
      9723
    ],
    "hash": [
      "000080358dcce46daabb716bb37f57b2360e7429125f9f426e64d7be640842ec",
      "0000fdf5b0a5201e1238f982555bd5104ceb6ffade2a2fda00f703c75be14aaa",
      "0000a6b7c4b15299ab59be2147aad645d2c0035286c3280b36a9e2f7da13543e",
      "00004abab8a39d243aad6d23008e0ee44c9e2acbf446608bcb413353bc0c36b5"
    ],
    "valid": true,
    "valid_setelah_dipalsukan": false,
    "percobaan_tambang_ulang": 348963,
    "valid_setelah_tambang_ulang": true
  },
  "kode_verifikasi": "5DF47ADEE8CA"
}
```
