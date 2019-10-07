# Modul 6 - _Abstract Class_ & _Interface_

Tujuan pembelajaran: Mahasiswa dapat mengenal _abstract class_ dan _interface_ dalam pemrograman berorientasi objek serta dapat menggunakannya  dengan baik

## Persiapan

- Buat project __Java Application__ baru di NetBeans

## Materi

### Abstract Class

* Class yang dinyatakan abstract seperti: ```abstract class NamaClass``` tidak dapat dibuat objek nya dengan perintah __new__. Dia hanya sebagai class kerangka saja dan harus diturunkan ke Class anak agar berfungsi.
* Tujuannya adalah menyembunyikan implementasi atau koding sebenarnya dari method-method yang ada sehingga orang lain dapat memahami kegunaannya tanpa harus tahu detil bagaimana dia berjalan.
* Setiap Class anak harus mengimplementasikan method-method yang ada di class induk yang abstract karena hanya berupa nama method tanpa isi kode. Namun tidak semua method di abstract class harus abstract, karena kita bisa memasukkan method yang normal di abstract class.
* Contoh pembuatan abstract class adalah sebagai berikut:

```java
abstract class OperasiBilangan {
    
    protected double a;
    protected double b;
    
    protected abstract void setA(double a);
    protected abstract void setB(double b);
    // method biasa (tidak abstract)
    protected void setAB(double a, double b) {
        this.a = a;
        this.b = b;
    }
    protected abstract double hitung();
    
}
```

* Jika class OperasiBilangan diturunkan maka semua method __setA__, __setB__, dan __hitung__ harus dideklarasikan oleh class anaknya.

### Interface

* Interface tidak diawali dengan kata kunci __class__ dan dia merupakan semua kontrak yang harus diimplementasikan oleh semua class yang mengimplementasikannya.
* Sama dengan method abstract di abstract class, maka method di interface juga hanya berupa nama tanpa implementasi.
* Dengan interface kita bisa "memaksakan" method-method apa saja yang harus tersedia dalam sebuah class ketika dia mengimplementasikan interface.
* Contoh pembuatan interface adalah sebagai berikut: (klik kanan icon __Java Source Package__ di jendela _Project_ lalu pilih __New__ -> __Java Interface__)

```java
interface KendaraanBermotor {
    
    public void bunyikanKlakson();
    public void gantiPersneling(int gigi);
    ...
    
}
```

### Perbedaan Abstract Class dan Interface

1. Abstract class menggunakan __extends__ sedangkan interface menggunakan __implements__.
2. Sebuah class bisa mengimplementasikan lebih dari satu interface tapi dia tidak bisa meng-_extends_ lebih dari satu abstract class.

## Praktikum

* Buat dua buah interface yaitu BangunDatar dan BangunRuang, ketikkan kode berikut untuk masing-masing interface:
* Kode di __BangunDatar.java__:

```java
package your.app.package;

public interface BangunDatar {
    
    public void setPanjang(double panjang);
    public void setLebar(double lebar);
    public double getLuas();
    
}
```

* Kode untuk __BangunRuang.java__:

```java
package your.app.package;

public interface BangunRuang {
    
    public void setPanjang(double panjang);
    public void setLebar(double lebar);
    public void setTinggi(double tinggi);
    public double getVolume();
    
}
```

* Selanjutnya kita akan buat abstract class __SegiEmpat.java__ dengan kode seperti berikut:

```java
package your.app.package;

// contoh class abstract
public abstract class SegiEmpat implements BangunDatar {
    
    protected double panjang;
    protected double lebar;
    
    @Override
    public abstract void setPanjang(double panjang);
    public double getPanjang() {
        return panjang;
    }
    
    @Override
    public abstract void setLebar(double lebar);
    public double getLebar() {
        return lebar;
    }
    
    @Override
    public abstract double getLuas();
    
}
```

* Class SegiEmpat diatas adalah sebuah abstract class yang mengimplementasikan interface BangunDatar. Kita tidak dapat membuat instance SegiEmpat karena dia abstract sehingga kita perlu membuat class turunan yaitu __Kotak.java__ yang meng-_extends_ class SegiEmpat ini. Kode untuk Kotak.java adalah:

```java
package your.app.package;

// contoh class meng-extends abstract class
public final class Kotak extends SegiEmpat {
    
    public Kotak() {
        setPanjang(1);
        setLebar(1);
    }
    public Kotak(double panjang, double lebar) {
        setPanjang(panjang);
        setLebar(lebar);
    }
    
    @Override
    public void setLebar(double lebar) {
        this.lebar = lebar < 0 ? 1 : lebar;
    }
    @Override
    public void setPanjang(double panjang) {
        this.panjang = panjang < 0 ? 1 : panjang;
    }
    
    @Override
    public double getLuas() {
        return panjang * lebar;
    }
    
}
```

* Selanjutnya adalah class contoh yang mengimplementasikan dua interface sekaligus. Class __Balok.java__ mengimplementasikan interface BangunDatar sekaligus mengimplementasikan interface BangunRuang dengan kode sebagai berikut:

```java
package your.app.package;

// contoh class meng-implement 2 interface
public final class Balok implements BangunDatar, BangunRuang {
    
    private double panjang;
    private double lebar;
    private double tinggi;
    
    public Balok() {
        setPanjang(1);
        setLebar(1);
        setTinggi(1);
    }
    public Balok(double panjang, double lebar, double tinggi) {
        setPanjang(panjang);
        setLebar(lebar);
        setTinggi(tinggi);
    }
    
    @Override
    public void setPanjang(double panjang) {
        this.panjang = panjang < 0 ? 1 : panjang;
    }
    @Override
    public void setLebar(double lebar) {
        this.lebar = lebar < 0 ? 1 : lebar;
    }
    @Override
    public void setTinggi(double tinggi) {
        this.tinggi = tinggi < 0 ? 1 : tinggi;
    }
    
    @Override
    public double getLuas() {
        return 2 * ((panjang * lebar) + (panjang * tinggi) + (lebar * tinggi));
    }
    @Override
    public double getVolume() {
        return panjang * lebar * tinggi;
    }
    
}
```

* Untuk beberapa contoh menggunakan class diatas, ketikkan kode berikut di file __Main.java__ (__class Main__):

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        Kotak k = new Kotak(4, 6);
        System.out.println(k.getLuas());
        
        Kotak kubus = new Kotak();
        kubus.setPanjang(4);
        kubus.setLebar(4);
        System.out.println(kubus.getLuas());
        
        Balok b = new Balok(5, 6, 7);
        System.out.println("Luas balok: " + b.getLuas());
        System.out.println("Volume balok: " + b.getVolume());
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan class BangunDatar yang telah kita buat

## Tugas

* Gunakan interface BangunDatar dan BangunRuang untuk membuat class __Lingkaran__, class __Tabung__ dan class __Kerucut__!