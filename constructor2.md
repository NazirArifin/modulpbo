__\__init()\____

Dalam pembuatan object, terdapat sebuah fungsi yang akan dipanggil secara otomatis ketika object tersebut dibuat. Fungsi tersebut disebut dengan __constructor__. Pada bahasa pemrograman Python, constructor didefinisikan dengan nama __\__init__()__.

Dengan adanya fungsi __\__init\__()__, kita dapat menentukan nilai awal dari object yang dibuat. Berikut adalah contoh penggunaan __\__init__()__:

```python
class BangunDatar:
  def __init__(self, panjang, lebar = 1):
    self.panjang = panjang

    if lebar is None:
      self.lebar = panjang
    else:
      self.lebar = lebar

  def hitung_luas(self):
    """Menghitung luas bangun datar"""
    return self.panjang * self.lebar
  
  def hitung_keliling(self):
    """Menghitung keliling bangun datar"""
    return 2 * (self.panjang + self.lebar)
  
kubus = BangunDatar(4)
print(kubus.hitung_luas())

balok = BangunDatar(4, 5)
print(balok.hitung_luas())
```

** _Perhatikan bahwa di Python, atribut __boleh tidak dituliskan__. Jika atribut tidak didefinisikan, maka atribut tersebut akan dibuat secara otomatis ketika atribut tersebut diakses atau diubah_. 

Pada contoh di atas, kita membuat sebuah class __BangunDatar__ yang memiliki dua atribut yaitu __panjang__ dan __lebar__. Ketika object dari class __BangunDatar__ dibuat, maka object tersebut akan memiliki dua atribut dengan nilai awal 10.

# Tugas Studi Kasus

Kerjakan studi kasus berikut secara berkelompok. __DILARANG MENGGUNAKAN CHATGPT, Claude, Gemini, dan lainnya__. Kerjakan studi kasus ini dengan kemampuan dan pengetahuan kalian sendiri!. __Bagi yang terbukti melakukan kecurangan, maka akan mendapatkan sanksi__

>Pembagian soal: Jumlahkan tanggal lahir anggota kelompok kalian, kemudian dimodulo 2. 
>Jika hasilnya 0, maka kerjakan soal Studi Kasus dengan nomor Genap. Jika hasilnya 1, maka kerjakan soal Studi Kasus dengan nomor Ganjil.


__1. STUDI KASUS 1__

Terdapat beberapa stadion (asumsikan bahwa stadion adalah Object) yang memiliki atribut nama stadion, kapasitas penonton, dan harga tiket masuk. Buatlah class __Stadion__ yang memiliki atribut-atribut tersebut. Gunakan constructor untuk menginisialisasi nilai dari atribut-atribut tersebut. Setelah itu buatlah 3 object dengan nama stadion, kapasitas penonton, dan harga tiket masuk sbb:
    
    - Stadion A, 5000, 50000
    - Stadion B, 7500, 100000
    - Stadion C, 10000, 500000

Hitunglah total pendapatan dari penjualan tiket masuk dari masing-masing stadion tersebut dengan ketentuan pendapatan stadion dikurangi 10% pajak jika kapasitas penonton lebih dari 7000.

__2. STUDI KASUS 2__

Terdapat beberapa buku (asumsikan bahwa buku adalah Object) yang memiliki atribut nama buku, jumlah halaman, dan harga. Buatlah class __Buku__ yang memiliki atribut-atribut tersebut. Gunakan constructor untuk menginisialisasi nilai dari atribut-atribut tersebut. Setelah itu buatlah 3 object dengan nama buku, jumlah halaman, dan harga sbb:
    
    - Buku A, 100, 50000
    - Buku B, 200, 100000
    - Buku C, 300, 150000

Hitunglah total harga dari masing-masing buku tersebut dengan ketentuan harga buku dikurangi 10% pajak jika jumlah halaman lebih dari 150.


__3. STUDI KASUS 3__

- Harga 1 liter Pertalite adalah 10000, harga 1 liter Pertamax adalah 12000.
- Jarak kota Pamekasan - Malang adalah 400km, jarak kota Pamekasan - Surabaya adalah 200km.

Buatlah sebuah class __Mobil__ yang memiliki atribut nama mobil, konsumsi pertalite/km dan konsumsi pertamax/km. Gunakan constructor untuk menginisialisasi nilai dari atribut-atribut tersebut. Setelah itu buatlah 2 object dengan nama mobil, konsumsi pertalite, dan konsumsi pertamax sbb:
    
  - Innova, 3, 4
  - Ertiga, 1, 0.5

Hitunglah biaya bahan bakar Innova ke Malang dan Surabaya pulang pergi, serta biaya bahan bakar Ertiga ke Malang dan Surabaya pulang pergi dengan ketentuan bahan bakar yang digunakan adalah Pertalite jika jarak lebih dari 300km, dan Pertamax jika jarak kurang dari 300km.

__4. STUDI KASUS 4__

Seorang peternak memiliki dua jenis kendaraan: Traktor dan Sepeda Motor. Traktor menggunakan solar dan konsumsi solarnya adalah 5 liter/km, sedangkan sepeda motor menggunakan pertalite dan konsumsi pertalitenya adalah 0.1 liter/km. Harga 1 liter solar adalah Rp 15.000 dan harga 1 liter pertalite adalah Rp 10.000.

Jarak rumah peternak ke sawah adalah 50 km dan jarak rumah ke ladang adalah 75 km.

Buatlah sebuah class __Kendaraan__ dengan atribut nama_kendaraan, konsumsi_solar/km dan konsumsi_pertalite/km. Gunakan constructor untuk menginisialisasi atribut tersebut. Setelah itu, buatlah dua objek:

  - Traktor, 5
  - Sepeda Motor, 0.2

Hitunglah biaya bahan bakar untuk perjalanan pulang-pergi dari rumah ke sawah dan dari rumah ke ladang untuk masing-masing kendaraan.


__5. STUDI KASUS 5__

Gaji pokok di perusahaan U adalah 900000, jika lulusan S1 naik 400000 dan jika S2 naik 500000 (kenaikan gaji hanya untuk satu jenjang pendidikan, S2 tidak akumulatif dengan S1 meskipun lulusan S2 memiliki gelar S1 juga). Masa kerja kelipatan 10 tahun mendapatkan kenaikan gaji 100000. Gaji dikurangi BPJS dan koperasi sebesar 200000. Selain itu potongan gaji didasarkan jumlah pinjaman ke bank untuk setiap pegawai jika ada.

- Pak "tsauri" lulusan S1 dengan masa kerja 5 tahun
- Pak "roni" lulusan S2 dengan masa kerja 11 tahun
- Pak "mohammad" lulusan S2 dengan masa kerja 20 tahun memiliki pinjaman bank 500000

Berapa gaji yang diterima oleh pak tsauri, pak roni dan pak mohammad?


__6. STUDI KASUS 6__

Sebuah perusahaan taksi memiliki sistem perhitungan biaya perjalanan. Rincian biaya perjalanan adalah sebagai berikut:
- Biaya awal: Rp 5000 (semua perjalanan dikenakan biaya awal), biaya awal ini tidak termasuk dalam perhitungan biaya per kilometer.
- Biaya per kilometer: Rp 3000 (biaya ini dikenakan per kilometer perjalanan)
- Biaya waktu tunggu: Rp 50 (biaya ini dikenakan per menit waktu tunggu)
- Jika jarak perjalanan lebih dari 10 km, ada diskon 10% dari total biaya perjalanan
- Pajak 5% ditambahkan setelah dihitung diskon

Buatlah class __Perjalanan__ dengan atribut nama penumpang, jarak perjalanan (km), waktu tunggu (menit). Gunakan constructor untuk menginisialisasi atribut tersebut. Setelah itu, buatlah 2 objek:

  - Penumpang A, 15 km, 30 menit
  - Penumpang B, 5 km, 10 menit

Hitunglah biaya perjalanan Penumpang A dan Penumpang B.

__7. STUDI KASUS 7__

Setiap hero memiliki nilai HP (Health Point), armor, dan damage. Jika dipukul() maka HP hero tersebut berkurang sebanyak damage yang dimiliki lawan dikurangi armor yang dimiliki oleh hero tersebut. Jika HP hero kurang dari 50, maka hero tersebut akan mendapatkan tambahan HP sebesar 20 (hanya sekali).

Buatlah class __Hero__ dengan atribut nama hero, HP, armor, dan damage. Gunakan constructor untuk menginisialisasi atribut tersebut. Terdapat method dipukul() yang akan mengurangi HP hero tersebut. Setelah itu, buatlah 2 objek:

  - Alucard, 180, 6, 10
  - Batman, 200, 5, 15

Hitunglah siapa yang menang dari pertarungan Alucard dan Batman (HP hero yang lebih besar dari 0).

__8. STUDI KASUS 8__

Setiap hero memiliki nilai HP (Health Point), armor, dan damage. Jika dipukul() maka HP hero tersebut berkurang sebanyak damage yang dimiliki lawan dikurangi armor yang dimiliki oleh hero tersebut. Jika HP hero kurang dari 50, maka hero tersebut akan mendapatkan tambahan armor sebesar 5 (hanya sekali).

Buatlah class __Hero__ dengan atribut nama hero, HP, armor, dan damage. Gunakan constructor untuk menginisialisasi atribut tersebut. Terdapat method dipukul() yang akan mengurangi HP hero tersebut. Setelah itu, buatlah 2 objek:

  - Alucard, 180, 6, 10
  - Batman, 200, 5, 15

Hitunglah siapa yang menang dari pertarungan Alucard dan Batman (HP hero yang lebih besar dari 0).


__9. STUDI KASUS 9__

Burung memiliki atribut nama, energi, dan kecepatan. Jika burung terbang() selama 5 detik maka energi berkurang sebanyak 5 dan kecepatan bertambah sebanyak 10 dari kecepatan default. Jika burung makan() maka energi bertambah sebanyak 10 namun kecepatan menjadi 0.

Buatlah class __Burung__ dengan atribut nama burung, energi, dan kecepatan. Gunakan constructor untuk menginisialisasi atribut tersebut. Terdapat method terbang() dan makan(). Setelah itu, buatlah 2 objek:

  - Parkit, 100, 50
  - Nuri, 200, 40

Hitunglah energi dan kecepatan dari Burung A dan Burung B jika:
- Burung A terbang 10 detik tanpa makan
- Burung B terbang 5 detik dan makan kemudian terbang 20 detik lagi

__10. STUDI KASUS 10__

Bebek memiliki atribut nama, energi, dan kecepatan. Jika bebek berenang() selama 5 detik maka energi berkurang sebanyak 15 dan kecepatan bertambah sebanyak 10 dari kecepatan default. Jika bebek makan() maka energi bertambah sebanyak 10 namun kecepatan menjadi 0.

Buatlah class __Bebek__ dengan atribut nama bebek, energi, dan kecepatan. Gunakan constructor untuk menginisialisasi atribut tersebut. Terdapat method berenang() dan makan(). Setelah itu, buatlah 2 objek:

  - Donald, 100, 50
  - Daffy, 200, 40

Hitunglah energi dan kecepatan dari Bebek A dan Bebek B jika:
- Bebek A berenang 10 detik tanpa makan kemudian berenang 5 detik lagi
- Bebek B berenang 5 detik dan makan kemudian berenang 20 detik lagi

---

__SELAMAT MENCARI SOLUSI DAN MENGERJAKAN!__



