# Modul 5 - Static & Final

Tujuan pembelajaran: Mahasiswa dapat memahami konsep final dan static dalam pemrograman berorientasi objek dan menggunakannya dengan baik

## Persiapan

- Buat project __Java Application__ baru di NetBeans

## Materi

### Static

* Property atau method dalam sebuah Class dapat dinyatakan __static__ yang mana nilainya akan sama untuk semua _instance_ atau objek yang dibuat dan nilainya jarang diganti-ganti. Hal ini berguna untuk menampung nilai yang sama untuk semua objek yang akan dibuat.
* Property / Method yang dinyatakan static adalah milik Class dan untuk mengaksesnya tidak perlu menggunakan perintah __new__ seperti contoh berikut:

```java
System.print.out(); // static method di class System, tanpa new
Math.PI; // static property PI di Class Math, tanpa new Math()
```

* Pada umumnya __static property__ digunakan untuk menyimpan konstanta (nilai yang tetap) dan __static method__ digunakan untuk fungsi yang cepat dan ringkas tanpa butuh bantuan method lain.

### Final

* Kata kunci final di property dan method digunakan untuk mencegah programmer lain mengubah definisi atau nilai dari property/method yang kita buat
* Final Class tidak bisa diturunkan, sedangkan final method bisa diturunkan ke Class anak tapi tidak bisa di Override
* Kata static yang ditambah final biasanya bernilai tetap, tidak akan berubah dan tidak bisa diubah, contohnya pada ```Math.PI```, ```Math.pow()```, dsb.

## Praktikum

* Buat Class baru dengan mengklik kanan icon __Java Source Package__ di jendela _Project_ lalu pilih __New__ -> __Java Class__. Di bagian Class Name diisi dengan nama __Prodi__ dan kemudian tekan tombol __Finish__.
* Ketikkan kode berikut di Class Prodi untuk membuat beberapa constant kode Prodi:

```java
package your.app.package;

// class dibuat final agar tidak bisa di extends
public final class Prodi {
    
    public static final int INFORMATIKA = 52;
    public static final int TEKNIK_SIPIL = 51;
    public static final int TEKNIK_INDUSTRI = 53;
    
    public static String getNamaProdi(int kode) {
        switch (kode) {
            case 52:
                return "INFORMATIKA";
            case 51:
                return "TEKNIK SIPIL";
            case 53:
                return "TEKNIK INDUSTRI";
        }
        return "";
    }
    
}
```

* Buat Class baru dengan nama class __Mahasiswa__ dan ketikkan kode berikut:

```java
package your.app.package;

public class Mahasiswa {
    
    private final String npm;
    private final String nama;
    private int prodi = 0;
    public static String universitas = "Universitas Madura";
    
    public Mahasiswa(String npm, String nama) {
        this.npm = npm;
        this.nama = nama;
    }
    public void setProdi(int prodi) {
        this.prodi = prodi;
    }
    public static void ubahKampus(String kampus) {
        universitas = kampus;
    }
    
    public void tampil() {
        System.out.println(npm + " - " + nama + " - " + Prodi.getNamaProdi(prodi) + " - " + universitas);
    }
    
}
```

* Untuk contoh penggunaan static property / method Anda ketikkan kode berikut di __Main.java__:

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        Mahasiswa ridho = new Mahasiswa("2019520001", "Ridho");
        ridho.setProdi(Prodi.INFORMATIKA);
        ridho.tampil();
        Mahasiswa najib = new Mahasiswa("2019520002", "Najeeb");
        najib.setProdi(Prodi.TEKNIK_SIPIL);
        najib.tampil();
        
        // ubah static property, maka semua objek langsung berubah
        Mahasiswa.universitas = "UNIRA";
        ridho.tampil();
        najib.tampil();
        
        // ubah dengan static method
        Mahasiswa.ubahKampus("Univ. Madura");
        ridho.tampil();
        najib.tampil();
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan beragam Class yang telah kita buat

## Tugas

* Buat aplikasi sederhana dengan input dari user untuk memasukkan data mahasiswa dengan ketentuan:

1. NIM, Nama dan Prodi harus dibuat final
2. Alamat dan email tidak boleh final
3. Buat constant kode Prodi dengan data dari tabel berikut: 

| ID PRODI | NAMA PRODI                  |
| -------- | --------------------------- |
| 11       | HUKUM                       |
| 21       | MANAJEMEN                   |
| 22       | AKUNTANSI                   |
| 31       | ADMINISTRASI PUBLIK         |
| 41       | PETERNAKAN                  |
| 51       | TEKNIK SIPIL                |
| 52       | INFORMATIKA                 |
| 53       | TEKNIK INDUSTRI             |
| 61       | PENDIDIKAN BAHASA INDONESIA |
| 62       | PENDIDIKAN MATEMATIKA       |
| 63       | PENDIDIKAN BAHASA INGGRIS   |

* Setiap kali selesai input data mahasiswa langsung tampilkan sebagai output, kemudian kembali minta data mahasiswa lagi (perulangan)