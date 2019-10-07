# Modul 7 - Polymorphism

Tujuan pembelajaran: Mahasiswa dapat memahami konsep polimorfisme dalam pemrograman berorientasi objek dan menggunakannya dengan baik

## Persiapan

- Buat project __Java Application__ baru di NetBeans

## Materi

### Polymorphism

* Polymorphism mempunyai arti "banyak bentuk", yang secara sederhananya terdapat sebuah interface yang diimplementasikan dalam banyak bentuk. Dalam dunia nyata contohnya adalah seorang lelaki pada waktu yang sama memiliki peran sebagai ayah, suami dan pekerja.
* Di Java terdapat dua macam polymorphism yaitu __Compile Time__ dan  __Runtime__ Polymorphism. Polymorphism bisa dicapai dengan menggunakan method overloading dan method overriding.

## Praktikum

* Buat Class baru dengan mengklik kanan icon __Java Source Package__ di jendela _Project_ lalu pilih __New__ -> __Java Class__. Di bagian Class Name diisi dengan nama __OperasiBilangan__ dan kemudian tekan tombol __Finish__.
* Kita akan membuat class induk dengan kode sebagai berikut:

```java
package your.app.package;

public class OperasiBilangan {
    
    protected double a;
    protected double b;
    
    protected void setA(double a) {
        this.a = a < 0 ? 1 : a;
    }
    protected void setB(double b) {
        this.b = b < 0 ? 1 : b;
    }
    public double hitung() {
        return 0;
    }
    
}
```

* Kita akan membuat static (compile time) polymorphism menggunakan class-class berikut (Penjumlahan & Pengurangan):
* Kode __Penjumlahan.java__:

```java
package your.app.package;

public class Penjumlahan extends OperasiBilangan {
    
    @Override
    public double hitung() {
        return a + b;
    }
    
}
```

* Kode __Pengurangan.java__:

```java
package myapp3;

public class Pengurangan extends OperasiBilangan {
    
    @Override
    public double hitung() {
        return a - b;
    }
    
}
```

* Selanjutnya kita akan membuktikan apakah class OperasiBilangan bisa menjadi banyak bentuk melalui runtime polymorphism dengan kode di __Main.java__ sebagai berikut:

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        
        OperasiBilangan jumlah = new Penjumlahan();
        jumlah.setA(3);
        jumlah.setB(5);
        System.out.println("Hasil 3 + 5 = " + jumlah.hitung());
        
        OperasiBilangan kurang = new Pengurangan();
        kurang.setA(3);
        kurang.setB(5);
        System.out.println("Hasil 3 - 5 = " + kurang.hitung());
        
    }
    
}
```

* Anda lihat bahwa tipe data ```jumlah``` adalah ```OperasiBilangan``` tapi diisi dengan instance dari ```Penjumlahan```. Namun Java dapat mengenali method hitung mana yang harus dieksekusi yaitu tidak menggunakan method hitung di class induk namun menggunakan method hitung di class anak.
* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan class BangunDatar yang telah kita buat

## Tugas

* Lengkapi untuk operasi bilangan __Pembagian__ dan __Perkalian__!