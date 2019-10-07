# Modul 8- Java Collections - ArrayList

Tujuan pembelajaran: Mahasiswa dapat mengenal dan menggunakan collections di Java salah satunya ArrayList dengan baik

## Persiapan

- Buat project __Java Application__ baru di NetBeans

## Materi

### ArrayList

* ArrayList adalah array yang bisa berubah-ubah ukurannya tergantung isi elemen yang ada didalamnya (dinamis) dan untuk menggunakan ArrayList Anda harus mengimport ```java.util.ArrayList```.
* Ada banyak method yang bisa digunakan dalam ArrayList seperti menambah item, menghapus, mengganti dan lain sebagainya.
* Elemen dari ArrayList harus berupa objek dan tidak dapat menerima data primitif seperti int, double, boolean, dll, sehingga Anda perlu menggunakan class wrapper data primitif seperti Integer, Double, Boolean, dll.
* Contoh sederhana membuat ArrayList adalah sebagai berikut:

```java
ArrayList<String> nama = new ArrayList<>(); // membuat ArrayList baru
nama.add("Ani"); // menambah elemen
nama.add("Budi");
nama.add("Cici");
System.out.println("ArrayList berisi: " + nama.size() + " yaitu: " + nama);
nama.clear(); // mengosongkan list
```

## Praktikum

* Buat Class baru dengan nama class __Buku__ dan ketikkan kode berikut:

```java
package your.app.package;

public class Buku {
    
    private final String judul;
    private final String pengarang;
    
    Buku(String judul, String pengarang) {
        this.judul = judul;
        this.pengarang = pengarang;
    }
    public String getJudul() {
        return judul;
    }
    public String getPengarang() {
        return pengarang;
    }
    
    public String tampil() {
        return "Judul: " + judul + ", Pengarang: " + pengarang;
    }
    
}
```

* Selanjutnya kita buat Class __Perpustakaan__ yang mengatur dan melakukan manajemen buku. Kode di __Perpustakaan.java__ adalah sebagai berikut:

```java
package your.app.package;

import java.util.ArrayList;

public class Perpustakaan {
    
    private ArrayList<Buku> koleksiBuku = new ArrayList<>();
    
    // jumlah buku
    public int jumlahBuku() {
        return koleksiBuku.size();
    }
    // tambah buku
    public void tambahBuku(String judul, String pengarang) {
        koleksiBuku.add(new Buku(judul, pengarang));
    }
    // edit buku baru
    public void gantiBuku(int index, String judul, String pengarang) {
        koleksiBuku.set(index, new Buku(judul, pengarang));
    }
    // edit buku dengan buku lama
    public void gantiBuku(int index, Buku baru) {
        koleksiBuku.set(index, baru);
    }
    // hapus buku dari array
    public void hapusBuku(int index) {
        koleksiBuku.remove(index);
    }
    // cari buku berdasarkan index
    public Buku lihatBuku(int index) {
        return koleksiBuku.get(index);
    }
    // cari buku berdasarkan judul
    public Buku cariBuku(String judul) {
        for (Buku buku : koleksiBuku) {
            if (buku.getJudul().contains(judul)) {
                return buku;
            }
        }
        return null;
    }
    // cari index buku berdasarkan objek Buku
    public int cariIndexBuku(Buku buku) {
        for (int i = 0; i < koleksiBuku.size(); i++) {
            if (koleksiBuku.get(i).equals(buku)) {
                return i;
            }
        }
        return -1;
    }
    
}
```

* Untuk mengecek class-class yang telah dibuat diatas, ketikkan kode berikut di class __Main.java__:

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        
        Perpustakaan perpus = new Perpustakaan();
        perpus.tambahBuku("Belajar JAVA", "Umar Bakrie");
        perpus.tambahBuku("Java Untuk Pemula", "Amir Mahmud");
        perpus.tambahBuku("Java untuk Ahli", "Salim Iklim");
        System.out.println("Jumlah buku: " + perpus.jumlahBuku()); // 3
        perpus.tambahBuku("Java untuk Mahasiswa", "Farid Akbar");
        
        // cari buku
        String cari = "Pemula";
        System.out.println("Cari buku dengan kata kunci: " + cari);
        Buku buku = perpus.cariBuku(cari);
        if (buku == null) {
            System.out.println("Buku tidak ditemukan");
        } else {
            System.out.println("Buku ditemukan dengan judul: " + buku.getJudul() + ", pengarang: " + buku.getPengarang());
        }
        
        // ganti buku
        perpus.gantiBuku(0, "Belajar JAVA Baru", "Umar Bakrie Salim");
        buku = perpus.lihatBuku(0);
        System.out.println("Buku diganti dengan judul: " + buku.getJudul() + ", pengarang: " + buku.getPengarang());
        
        // hapus buku
        perpus.hapusBuku(0);
        System.out.println("Jumlah buku: " + perpus.jumlahBuku());
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan class BangunDatar yang telah kita buat

## Tugas

* Buat aplikasi sederhana menggunakan data mahasiswa. Beberapa hal yang harus dikerjakan adalah:

1. Buat menu aplikasi: Tambah Data, Cari Mahasiswa, Edit Data dan Hapus Data
2. Buat class Mahasiswa dengan property NPM, Nama dan Alamat
3. Usahakan buat class _perantara_ seperti class __Perpustakaan__ diatas, sehingga class Main tidak terlalu banyak terisi kode