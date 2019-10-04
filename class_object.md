# Modul 1 - Pengenalan Class dan Objek

Tujuan pembelajaran: Mahasiswa dapat mengenal class dan objek dalam pemrograman berorientasi objek serta dapat menggunakannya  dengan baik

## Persiapan

* Buat project __Java Application__ baru di NetBeans

## Materi
### Class

* Sebelum dapat membuat Objek maka kita harus membuat Class terlebih dahulu. Class merupakan kerangka,  rancangan atau desain dari Objek. Di dalam Class kita mendefinisikan __property__ (variabel) serta __method__ (fungsi). 

* Beberapa catatan dalam membuat pembuatan Class adalah:

1. Pembuatan Class dimulai dengan menggunakan kata kunci __class__ dan diikuti nama Class yang diinginkan dan dibungkus dengan tanda kurung kurawal. Contohnya adalah:

```java
class Hewan {
  ...
}
```

2. Nama Class umumnya dimulai dengan huruf kapital dan jika terdiri dari lebih dari satu kata maka kata berikutnya huruf awalnya juga menggunakan huruf kapital. Contohnya adalah: ```class SegiTiga```, ```class BujurSangkar```, ```class User```, dll
3. Sebuah  Class pada umumnya memiliki _property_ dan _method_ tapi ada kalanya sebuah Class hanya memiliki property saja atau method saja. Ini berarti bahwa Class tidak harus memiliki property dan method secara bersamaan.
4. Nama __method__ biasanya dalam bentuk kata kerja karena pada umumnya method akan melakukan sesuatu hal khusus dalam Class tersebut.

### Objek

* Objek adalah _instance_ atau perwujudan dari Class dan dibuat dengan menggunakan kata kunci __new__. Contohnya adalah:

  ```java
  Hewan h = new Hewan(); // NamaKelas namavariabel = new NamaKelas();
  ```

* Beberapa catatan dari penggunaan Objek adalah:

1. Sebuah Class bisa memiliki banyak (lebih dari satu) Objek yang memiliki method dan nilai property-nya masing-masing.
2. Untuk mengakses property atau method dari Class digunakan tanda dot (titik) ```.```, contohnya adalah sebagai berikut:

```java
Hewan h = new Hewan();
h.nama = "Kitty"; // mengakses property
h.berbunyi(); // mengakses method
```

## Praktikum

* Buat Class baru dengan mengklik kanan icon __Java Source Package__ di jendela _Project_ lalu pilih __New__ -> __Java Class__. Di bagian Class Name diisi dengan nama __BangunDatar__ dan kemudian tekan tombol __Finish__.
* Selanjutnya ketikkan kode berikut di Class Bangun Datar:

```java
package your.app.package;

class BangunDatar {
    
    // property panjang dan lebar
    double panjang;
    double lebar;
    
    // method getLuas
    double getLuas() {
        return panjang * lebar;
    }
}
```

* Dengan kode diatas kita membuat sebuah rancangan Objek BangunDatar dengan membuat Class BangunDatar. Di Class tersebut terdapat dua property (variabel) yaitu __panjang__ dan __lebar__ serta terdapat satu method (fungsi) yaitu __getLuas()__.
* Untuk beberapa contoh menggunakan class diatas, ketikkan kode berikut di file __Main.java__ (__class Main__):

```java
package your.app.package;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        // Buat object dengan nama bd1
        BangunDatar bd1 = new BangunDatar();
        System.out.println(bd1.getLuas());
        
        // Mengubah nilai di property
        bd1.panjang = 10;
        bd1.lebar = 5;
        double luasBd1 = bd1.getLuas();
        System.out.println(luasBd1);
        
        // Buat object baru dengan nama bd2
        BangunDatar bd2 = new BangunDatar();
        System.out.println(bd2.getLuas());
        
        // Contoh input dari user
        Scanner cin = new Scanner(System.in);
        System.out.print("Masukkan panjang bangun datar: ");
        bd2.panjang = cin.nextDouble();
        System.out.print("Masukkan lebar bangun datar: ");
        bd2.lebar = cin.nextDouble();
        System.out.println("Luas Bangun Datar: " + bd2.getLuas());
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan class BangunDatar yang telah kita buat

## Tugas

Selesaikan permasalahan berikut ini dengan menggunakan class dan objek!

- Harga satu buah pensil 1000, harga satu buah bolpoint 2000 dan harga satu buah penghapus adalah 500 (gunakan property untuk menyimpan nilai ini)
- Pedagang __"ahmad"__ memiliki 10 bolpoint, 10 pensil dan 20 penghapus, tampilkan berapa total uang yang didapat jika laku semua! (gunakan method untuk menghitung total pendapatan)
- Pedagang __"budi"__ memiliki 5 bolpoint, 30 pensil dan 2 penghapus, tampilkan juga berapa total uang yang didapat!
