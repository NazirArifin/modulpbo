# Modul 3 - Pewarisan (_Inheritance_)

Tujuan pembelajaran: Mahasiswa dapat memahami konsep pewarisan dalam pemrograman berorientasi objek dan menggunakannya dengan baik

## Persiapan

* Buat project __Java Application__ baru di NetBeans

## Materi

* Property dan atau method suatu Class bisa __diturunkan__ atau __diwariskan__ ke Class lain, artinya Class turunannya akan memiliki semua property dan method dengan hak akses __public__ dan __protected__ Class induk. Hal ini berguna agar Class yang kita buat bisa "dikembangkan" atau dipakai berulang-ulang oleh Class lain.
* Kata kunci yang digunakan adalah __extends__, contohnya: ```class ClassAnak extends ClassInduk { ... }```
* Class anak diperbolehkan __mengubah__ method Class induk dengan menggunakan kata kunci __Override__ yang menandakan bahwa isi method Class induk diganti dengan method Class anak.
* Class anak hanya boleh meng-__extends__ satu class induk saja namun Class induk bisa di-__extends__ oleh banyak Class anak.

## Praktikum

* Buat Class baru dengan mengklik kanan icon __Java Source Package__ di jendela _Project_ lalu pilih __New__ -> __Java Class__. Di bagian Class Name diisi dengan nama __BangunDatar__ dan kemudian tekan tombol __Finish__.
* Selanjutnya ketikkan kode berikut di Class Bangun Datar:

```java
package package your.app.package;

class BangunDatar {
    
    // dibuat protected agar bisa diturunkan ke class anak
    protected double panjang, lebar;
    
    // getter dan setter panjang
    public double getPanjang() {
        return panjang;
    }
    public void setPanjang(double panjang) {
        this.panjang = panjang < 1 ? 1 : panjang;
    }
    // getter dan setter lebar
    public double getLebar() {
        return lebar;
    }
    public void setLebar(double lebar) {
        this.lebar = lebar < 1 ? 1 : lebar;
    }
    
    // method getLuas
    public double getLuas() {
        return panjang * lebar;
    }
}
```

* Sekarang kita akan membuat Class turunan dari BangunDatar yaitu Class SegiTiga, Lingkaran, Ellips dan Trapesium
*  Kode untuk Class __SegiTiga.java__ (* untuk membuat Class baru dapat menggunakan cara waktu membuat Class BangunDatar):

```java
package your.app.package;

class SegiTiga extends BangunDatar {
    
    public double getAlas() {
        return panjang;
    }
    public void setAlas(double alas) {
        // memanggil method class induk
        super.setPanjang(alas);
    }
    public double getTinggi() {
        return lebar;
    }
    public void setTinggi(double tinggi) {
        super.setLebar(tinggi);
    }
    
    @Override
    public double getLuas() {
        return (panjang * lebar) / 2;
    }
    
}
```

* Kode untuk Class __Lingkaran.java__:

```java
package your.app.package;

// Lingkaran hanya memiliki satu bagian yaitu diameter
class Lingkaran extends BangunDatar {
    
    public double getDiameter() {
        return panjang;
    }
    public void setDiameter(double diameter) {
        super.setPanjang(diameter);
    }
    
    @Override
    public double getLuas() {
        // rumusnya PI * (r*r) -> r = jari2
        return Math.PI * Math.pow(panjang / 2, 2);
    }
    
}
```

* Kode untuk Class __Trapesium.java__:

```java
package your.app.package;

class Trapesium extends BangunDatar {
    
    protected double panjangAtas;
    
    public double getPanjangAtas() {
        return panjangAtas;
    }
    public void setPanjangAtas(double panjangAtas) {
        this.panjangAtas = panjangAtas < 0 ? 1 : panjangAtas;
    }
    
    @Override
    public double getLuas() {
        // luas trapesium = ((p + pA) / 2) * 3
        return ((panjang + panjangAtas) / 2) * 3;
    }
    
}

```

* Untuk beberapa contoh menggunakan class diatas, ketikkan kode berikut di file __Main.java__ (__class Main__):

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        BangunDatar balok = new BangunDatar();
        balok.setLebar(5);
        balok.setPanjang(10);
        System.out.println("Luas balok: " + balok.getLuas());
        
        SegiTiga st = new SegiTiga();
        st.setAlas(5);
        st.setTinggi(7);
        System.out.println("Luas segitiga: " + st.getLuas());
        
        Lingkaran l = new Lingkaran();
        l.setDiameter(10);
        System.out.println("Luas lingkaran: " + l.getLuas());
        
        Trapesium t = new Trapesium();
        t.setPanjang(15);
        t.setPanjangAtas(10);
        t.setLebar(4);
        System.out.println("Luas trapesium: " + t.getLuas());
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan beragam Class yang telah kita buat

## Tugas

* Sebuah loket tempat wisata membuat aplikasi menghitung biaya total tiket masuk untuk beberapa kendaraan berdasarkan pada jenis (BUS, MINIBUS, MOBIL, SEPEDA MOTOR) dan berapa orang yang ada di kendaraan tersebut. Selesaikan permasalahan berikut ini dengan menggunakan inheritance!

1. Buat class Kendaraan sebagai Class induk dengan property: jenis, jumlah penumpang, biaya tiket kendaraan, dan biaya tiket perpenumpang
2. Buat beberapa class anak (BUS, MINIBUS, MOBIL, SEPEDA MOTOR) berdasarkan class Kendaraan dan tentukan berapa total yang harus dibayar oleh kendaraan ini dengan asumsi semua kendaraan penumpangnya terisi penuh 