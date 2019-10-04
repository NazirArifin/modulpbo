# Modul 4 - Constructor

Tujuan pembelajaran: Mahasiswa dapat memahami konsep _class constructor_ dalam pemrograman berorientasi objek dan menggunakannya dengan baik

## Persiapan

* Buat project __Java Application__ baru di NetBeans

## Materi

* Constructor adalah __method__ yang otomatis dipanggil ketika sebuah objek dibuat (*dengan menggunakan kata kunci __new()__) dan biasanya digunakan oleh Class untuk mempersiapkan segala hal agar nanti class dapat berjalan dengan baik.
* Beberapa hal mengenai pembuatan constructor adalah:
1. Nama constructor harus sama dengan nama Class serta tidak boleh memiliki __return__.
2. Constructor tidak harus selalu ada tapi jika sebuah Class tidak memiliki constructor maka Java akan membuatkan constructor default secara otomatis
3. Untuk memanggil constructor Class induk digunakan kata kunci __super__
4. Pada umumnya constructor memiliki hak akses __public__ tapi untuk beberapa kasus bisa saja constructor memiliki hak akses __private__.

## Praktikum

* Berdasarkan contoh praktikum sebelumnya, kita akan mengaplikasikan constructor pada Class BangunDatar untuk memastikan nilai panjang dan lebar terisi dengan baik sehingga ketika dipanggil method getLuas() tidak menghasilkan angka 0. Berikut ini kode dari __BangunDatar.java__:

```java
package your.app.package;

class BangunDatar {
    
    protected double panjang, lebar;
    
    // constructor tanpa parameter
    BangunDatar() {
        panjang = 10; // beri nilai default
        lebar = 10; // beri nilai default
    }
    // constructor dengan parameter
    BangunDatar(double panjang, double lebar) {
        setPanjang(panjang);
        setLebar(lebar);
    }
    
    public double getPanjang() {
        return panjang;
    }
    public void setPanjang(double panjang) {
        this.panjang = panjang < 1 ? 1 : panjang;
    }
    public double getLebar() {
        return lebar;
    }
    public void setLebar(double lebar) {
        this.lebar = lebar < 1 ? 1 : lebar;
    }
    
    public double getLuas() {
        return panjang * lebar;
    }
}
```

* Untuk contoh menggunakan class diatas, ketikkan kode berikut di file __Main.java__ (__class Main__):

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        BangunDatar kubus = new BangunDatar();
        // luas tidak akan 0 karena ada nilai default
        System.out.println("Luas kubus: " + kubus.getLuas());
        
        BangunDatar balok = new BangunDatar(5, 3);
        System.out.println("Luas balok: " + balok.getLuas());
    }
    
}
```

## Tugas

* Aplikasikan constructor pada class SegiTiga, Lingkaran, dan Trapesium yang ada di praktikum sebelumnya sehingga tanpa memasukkan panjang/lebar/tinggi/alas fungsi __getLuas()__ tidak menghasilkan angka 0!