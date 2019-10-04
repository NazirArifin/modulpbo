# Modul 2 - Enkapsulasi (_Encapsulation_)

Tujuan pembelajaran: Mahasiswa dapat memahami konsep enkapsulasi dalam pemrograman berorientasi objek dan menggunakannya dengan baik

## Persiapan

- Buat project __Java Application__ baru di NetBeans

## Materi

* Enkapsulasi adalah mekanisme dalam PBO untuk mencegah/mengijinkan property atau method diakses dari luar Class. Kenapa harus dilindungi? Tujuannya adalah agar Class tersebut dapat berjalan sebagaimana mestinya sesuai dengan apa yang diinginkan oleh programmer. Untuk melakukan enkapsulasi digunakan kata kunci / _modifier_ sebagai berikut:

1. __public__ : bisa diakses oleh class manapun / luar class, bisa diakses oleh anggota lain dalam class, bisa diakses oleh anggota dari class turunan
2. __protected__ : tidak bisa diakses dari luar class, bisa diakses anggota lain dalam class, bisa diakses oleh anggota dari class turunan
3. __private__ : tidak dapat diakses dari luar class, bisa diakses anggota lain dalam class, tidak bisa diakses anggota dari class turunan

* Di Java Anda bisa memberikan modifier diatas di depan kata kunci __class__ berhubungan dengan akses class dalam package, tapi di bahasa pemrograman lain tidak ada

* Terdapat beberapa hal yang harus Anda perhatikan dalam menentukan hak akses dari property atau method yaitu:

1. Usahakan semua property dan method __PRIVATE__, jika akan digunakan oleh Class turunan baru diubah ke __PROTECTED__, dan gunakan __PUBLIC__ seminimal mungkin.
2. Jika Anda lupa memberi kata kunci modifier maka Java akan menganggap property/method tersebut adalah __PUBLIC__.
3. Karena semua property umumnya private maka biasanya perlu dibuat method __GETTER__ dan __SETTER__. Getter digunakan untuk mendapatkan nilai dari property dan Setter digunakan untuk mengubah nilai property.

## Praktikum

* Buat Class baru dengan mengklik kanan icon __Java Source Package__ di jendela _Project_ lalu pilih __New__ -> __Java Class__. Di bagian Class Name diisi dengan nama __BangunDatar__ dan kemudian tekan tombol __Finish__.
* Selanjutnya ketikkan kode berikut di Class Bangun Datar:

```java
package your.app.package;

class BangunDatar {
    
    // property panjang dan lebar
    private double panjang;
    private double lebar;
    
    // getter dan setter panjang
    public double getPanjang() {
        return panjang;
    }
    public void setPanjang(double panjang) {
        // setter dengan validasi
        if (panjang < 1) {
            this.panjang = 1;
        } else {
            this.panjang = panjang;
        }
    }
    // getter dan setter lebar
    public double getLebar() {
        return lebar;
    }
    public void setLebar(double lebar) {
        // ternary operator
        this.lebar = lebar < 1 ? 1 : lebar;
    }
    
    // method getLuas
    public double getLuas() {
        return panjang * lebar;
    }
}
```

* Untuk beberapa contoh menggunakan class diatas, ketikkan kode berikut di file __Main.java__ (__class Main__):

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        BangunDatar bd1 = new BangunDatar();
        System.out.println(bd1.getPanjang()); // dapatkan nilai property
        bd1.setPanjang(10); // mengeset nilai property
        bd1.setLebar(3);
        System.out.println(bd1.getLuas());
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan class BangunDatar yang telah kita buat

## Tugas

Selesaikan permasalahan berikut ini dengan menggunakan class dan objek!

1. Buat sebuah Class Bus dengan private property jumlah penumpang dan kapasitas
2. Buat setter untuk mengeset nilai kapasitas bus
3. Buat getter untuk mengetahui jumlah penumpang saat ini dan kapasitasnya
4. Buat setter untuk menambah dan mengurangi penumpang saat ini dan validasi input agar tidak lebih dari kapasitas bus atau lebih kecil dari 0
5. Buat bus yang lebih besar dari bus sebelumnya dengan membuat objek baru