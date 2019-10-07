# Modul 9- MySQL Connection

Tujuan pembelajaran: Mahasiswa dapat menggunakan beragam class di Java untuk melakukan koneksi ke database MySQL dengan baik.

## Persiapan

- Buat project __Java Application__ baru di NetBeans
- Masukkan library __MySQL JDBC Driver__ kedalam project dengan cara klik kanan __Libraries__ lalu pilih __Add Library__, cari dan pilih __MySQL JDBC Driver__ dan kemudian klik tombol __Add Library__.
- ** Jika di library tidak ada MySQL JDBC Driver maka Anda bisa mengunduhnya di [dev.mysql.com](https://dev.mysql.com/downloads/connector/j/) kemudian pilih operating system yang Anda gunakan. Ekstrak hasil unduhan kemudian tambahkan file __mysql-connector-java-8.0.17.jar__ ke __Libraries__ (caranya sama dengan diatas tapi pilih __Add JAR/Folder__.
- Pastikan service mysql berjalan dengan membuka [phpmyadmin](http://localhost/phpmyadmin)

## Praktikum

* Buat database __"perpustakaan"__ lalu buat tabel __"buku"__ dengan menjalankan kode sql berikut:

```mysql
CREATE TABLE `buku` (
  `id_buku` int(11) PRIMARY KEY NOT NULL AUTO_INCREMENT,
  `judul_buku` varchar(80) NOT NULL,
  `pengarang_buku` varchar(60) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
```

* Untuk mempermudah pembuatan koding berikutnya kita akan membuat sebuah model dari tabel buku, yang mana model ini adalah sebuah Classs yang mencerminan struktur tabel buku di Java. Buat file dan ketikkan kode berikut di file __Buku.java__:

```java
package your.app.package;

public class Buku {
    
    private final String judul;
    private final int id;
    private final String pengarang;
    
    public Buku(int id, String judul, String pengarang) {
        this.id = id;
        this.judul = judul;
        this.pengarang = pengarang;
    }
    public int getId() {
        return id;
    }
    public String getJudul() {
        return judul;
    }
    public String getPengarang() {
        return pengarang;
    }
    
    public String tampil() {
        return "ID: " + id + ", Judul: " + judul + ", Pengarang: " + pengarang;
    }
    
}
```

* Buat class __DbConnection__ untuk memastikan bahwa aplikasi sudah bisa terkoneksi ke database MySQL. Ketikkan kode berikut di file __DbConnection.java__:

```java
package your.app.package;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DbConnection {
    
    private static Connection mysqlconfig;
    private final String database = "perpustakaan";
    private final String user = "root";
    private final String password = "";
    
    private DbConnection() {}
    public static Connection connect() throws SQLException {
        if (mysqlconfig != null) {
            return mysqlconfig;
        } else {
            try {
                DbConnection db = new DbConnection();
                String url = "jdbc:mysql://localhost:3306/" + db.database;
                // DriverManager.registerDriver(new com.mysql.jdbc.Driver());
                DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
                mysqlconfig = DriverManager.getConnection(url, db.user, db.password);
            } catch(SQLException e) {
                System.out.println("Koneksi gagal " + e.getMessage());
            }
            return mysqlconfig;
        }
    }
    
}
```

* Selanjutnya buat class __Perpustakaan__ yang mengatur data buku mulai dari menambah, mencari, mengedit, dsb. Buat file dan ketikkan kode berikut di file __Perpustakaan.java__:

```java
package your.app.package;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class Perpustakaan {
    
    private Connection conn;
    
    public Perpustakaan() {
        try {
            conn = DbConnection.connect();
        } catch (SQLException e) { 
            showQueryFail(e);
        }
    }
    
    // jumlah buku
    public int jumlahBuku() {
        int total = 0;
        String query = "SELECT COUNT(id_buku) AS total FROM buku";
        try {
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(query);
            if (rs.next()) {
                total = rs.getInt("total");
            }
        } catch (SQLException e) {
            showQueryFail(e);
        }
        return total;
    }
    // tambah buku
    public int tambahBuku(String judul, String pengarang) {
      int id = 0;
      String query = "INSERT INTO buku VALUES(NULL, ?, ?)";
      try {
          PreparedStatement pst = conn.prepareStatement(query, PreparedStatement.RETURN_GENERATED_KEYS);
          pst.setString(1, judul);
          pst.setString(2, pengarang);
          pst.execute();
          ResultSet rs = pst.getGeneratedKeys();
          if (rs.next()) {
              id = rs.getInt(1);
          }
      } catch (SQLException e) {
          showQueryFail(e);
      }
      return id;
    }
    // cari buku berdasarkan id
    public Buku lihatBuku(int id) {
        Buku buku = null;
        String query = "SELECT * FROM buku WHERE id_buku = ?";
        try {
            PreparedStatement pst = conn.prepareStatement(query);
            pst.setInt(1, id);
            pst.execute();
            ResultSet rs = pst.getResultSet();
            if (rs.next()) {
                buku = new Buku(rs.getInt("id_buku"), rs.getString("judul_buku"), rs.getString("pengarang_buku"));
            }
        } catch (SQLException e) {
            showQueryFail(e);
        }
        return buku;
    }
    // cari buku berdasarkan judul
    public Buku cariBuku(String judul) {
        Buku buku = null;
        String query = "SELECT * FROM buku WHERE judul_buku LIKE ? LIMIT 0, 1";
        try {
            PreparedStatement pst = conn.prepareCall(query);
            pst.setString(1, "%" + judul + "%");
            pst.execute();
            ResultSet rs = pst.getResultSet();
            if (rs.next()) {
                buku = new Buku(rs.getInt("id_buku"), rs.getString("judul_buku"), rs.getString("pengarang_buku"));
            }
        } catch (SQLException e) {
            showQueryFail(e);
        }
        return buku;
    }
    
    // Pesan ketika query gagal
    private void showQueryFail(SQLException e) {
        System.out.println("Query gagal " + e.getMessage());
    }
    
}
```

* Untuk beberapa contoh menggunakan class diatas, ketikkan kode berikut di file __Main.java__ (__class Main__):

```java
package your.app.package;

public class Main {

    public static void main(String[] args) {
        
        Perpustakaan perpus = new Perpustakaan();
        // mendapatkan total buku
        System.out.println("Jumlah Buku: " + perpus.jumlahBuku());
        // menambah buku
        perpus.tambahBuku("Java MySQL", "Budi Irawan");
        // menambah dan menampilkan
        int id = perpus.tambahBuku("Java Database 2", "Rina Suhadi");
        System.out.println(perpus.lihatBuku(id).tampil());
        // menambah lagi
        perpus.tambahBuku("Mahasiswa UNIRA", "Abdul Malik");
        System.out.println("Jumlah Buku: " + perpus.jumlahBuku());
        // mencari buku dengan judul
        System.out.println(perpus.cariBuku("mysql").tampil());
        
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan beragam Class yang telah kita buat. Lihat perubahan data yang ada di database Anda untuk mengecek apakah proses insert berhasil.

## Tugas

* Lanjutkan kode di class __Perpustakaan__ agar bisa mengedit (berdasarkan id) dan menghapus data (berdasarkan id)

