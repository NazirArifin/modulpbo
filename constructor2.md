__\__init()\____

Dalam pembuatan object, terdapat sebuah fungsi yang akan dipanggil secara otomatis ketika object tersebut dibuat. Fungsi tersebut disebut dengan __constructor__. Pada bahasa pemrograman Python, constructor didefinisikan dengan nama __\__init__()__.

Dengan adanya fungsi __\__init\__()__, kita dapat menentukan nilai awal dari object yang dibuat. Berikut adalah contoh penggunaan __\__init__()__:

```python
class BangunDatar:
  def __init__(self):
    self.panjang = 10
    self.lebar = 10

  def getLuas(self):
    return self.panjang * self.lebar
```

** _Perhatikan bahwa di Python, atribut __boleh tidak dituliskan__. Jika atribut tidak didefinisikan, maka atribut tersebut akan dibuat secara otomatis ketika atribut tersebut diakses atau diubah_. 

Pada contoh di atas, kita membuat sebuah class __BangunDatar__ yang memiliki dua atribut yaitu __panjang__ dan __lebar__. Ketika object dari class __BangunDatar__ dibuat, maka object tersebut akan memiliki dua atribut dengan nilai awal 10.

# Tugas Studi Kasus

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