# Modul 9 - Database Connection (PHP Version)

Tujuan Pembelajaran: Mahasiswa dapat mengetahui cara melakukan koneksi ke database MySQL dan PostgreSQL menggunakan PHP.

## Persiapan

- Pastikan bahwa database MySQL dan PostgreSQL sudah terinstall di komputer Anda dan service MySQL dan PostgreSQL sudah berjalan. Akan lebih baik jika Anda menggunakan Docker untuk menjalankan database MySQL dan PostgreSQL.

- Pastikan PHP sudah mengaktifkan ekstensi `pdo_mysql` dan `pdo_pgsql`. Untuk mengaktifkan ekstensi tersebut Anda bisa mengunjungi [PHP Manual](https://www.php.net/manual/en/book.pdo.php).

## Materi

### MySQL

- MySQL (atau MariaDB) adalah salah satu jenis database yang paling banyak digunakan di dunia. Untuk mengunduh MySQL Anda bisa mengunjungi [MySQL Downloads](https://dev.mysql.com/downloads/mysql/). Untuk mengunduh MariaDB Anda bisa mengunjungi [MariaDB Downloads](https://downloads.mariadb.org/).

### PostgreSQL

- PostgreSQL adalah salah satu jenis database yang paling banyak digunakan di dunia selain MySQL. Untuk mengunduh PostgreSQL Anda bisa mengunjungi [PostgreSQL Downloads](https://www.postgresql.org/download/).

- Baik MySQL dan PostgreSQL dapat dijalankan di komputer Anda secara lokal atau dijalankan di Docker. Untuk menjalankan MySQL di Docker Anda bisa mengunjungi [MySQL Docker](https://hub.docker.com/_/mysql). Untuk menjalankan PostgreSQL di Docker Anda bisa mengunjungi [PostgreSQL Docker](https://hub.docker.com/_/postgres).

### PDO

PDO adalah sebuah class yang digunakan untuk melakukan koneksi ke database. PDO memiliki beberapa kelebihan dibandingkan dengan class koneksi database yang lain, yaitu:

- PDO mendukung berbagai jenis database, sehingga kita tidak perlu membuat koneksi database yang berbeda untuk setiap jenis database yang digunakan.
- PDO memiliki __fungsi yang sama__ untuk setiap jenis database, sehingga kita tidak perlu mempelajari fungsi-fungsi koneksi database yang berbeda untuk setiap jenis database yang digunakan.

### Composer

- Composer adalah sebuah package manager untuk PHP. Composer dapat digunakan untuk menginstall library PHP yang dibutuhkan oleh aplikasi kita. Composer juga dapat digunakan untuk menginstall library PHP yang dibutuhkan oleh library PHP yang kita gunakan. Composer dapat diunduh di [Composer Downloads](https://getcomposer.org/download/).

- Untuk menginstall library PHP yang dibutuhkan oleh aplikasi kita, kita perlu membuat file __composer.json__ di root folder aplikasi kita. File __composer.json__ berisi daftar library PHP yang dibutuhkan oleh aplikasi kita. File __composer.json__ dapat dibuat dengan mengetikkan kode berikut di terminal:

```bash
composer init
```

- Selanjutnya ikuti petunjuk yang ditampilkan di terminal untuk membuat file __composer.json__.

## Praktikum

- Buat database __"test"__ lalu buat tabel __"buku"__ dengan menjalankan kode sql berikut:

- MySQL:
```mysql
CREATE TABLE `buku` (
  id int(11) PRIMARY KEY NOT NULL AUTO_INCREMENT,
  judul VARCHAR(255) NOT NULL,
  tahun_terbit SMALLINT(4) NOT NULL,
  pengarang VARCHAR(255) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

- PgSQL:
```pgsql
CREATE TABLE buku(
  id SERIAL NOT NULL,
  judul character varying(255) NOT NULL,
  tahun_terbit NUMERIC(4) NOT NULL,
  pengarang character varying(255) NOT NULL,
  PRIMARY KEY(id)
);
```

- Buat file __dbconfig/mysql.ini__ dan __dbconfig/pgsql.ini__ untuk menyimpan konfigurasi database MySQL dan PostgreSQL. Isi file __dbconfig/mysql.ini__ dengan kode berikut:

```ini
DB_HOST=127.0.0.1
DB_USER=root
DB_PASS=secret
DB_NAME=test
DB_PORT=3333
```

- Isi file __dbconfig/pgsql.ini__ dengan kode berikut:

```ini
DB_HOST=127.0.0.1
DB_USER=postgres
DB_PASS=secret
DB_NAME=test
DB_PORT=5432
```

- Koneksi ke database dilakukan dengan menggunakan PDO. Buat class __DbConnection__ untuk memastikan bahwa aplikasi sudah bisa terkoneksi ke database MySQL dan PostgreSQL. Class akan akan menerapkan konsep __Singleton__ untuk memastikan bahwa hanya ada satu koneksi ke database. 

Ketikkan kode berikut di file __DbConnection.php__:

```php
<?php
class DbConnection {
  // instance db dibuat private agar tidak bisa diakses dari luar class
  private static PDO $instance;
  
  // driver database
  private static string $driver = 'mysql';

  // config database berisi DB_HOST, DB_NAME, DB_USER, DB_PASS, DB_PORT
  private static array $config = [];

  // constructor sengaja dibuat private agar tidak bisa diakses dari luar class
  private function __construct() {}

  // setter driver
  public static function setDriver(string $driver): void {
    // jika instance sudah dibuat, maka driver tidak bisa diubah
    if (! empty(self::$instance)) {
      throw new Exception('Driver tidak bisa diubah setelah instance dibuat');
    }

    // hanya bisa mengubah driver jika driver yang diubah berbeda dengan driver sebelumnya
    if (self::$driver !== $driver) {
      switch ($driver) {
        case 'mysql':
          self::$driver = 'mysql';
          break;
        case 'pgsql':
          self::$driver = 'pgsql';
          break;
        default:
          self::$driver = 'mysql';
          break;
      }
    }
  }

  // method untuk mengambil instance db
  public static function getInstance(): PDO {
    if (empty(self::$instance)) {
      // jika config database belum di set, maka set config database
      // yang ada di folder dbconfig/driver.ini
      if (empty(self::$config)) {
        self::$config = parse_ini_file('dbconfig/' . self::$driver . '.ini');
      }

      // jika instance belum dibuat, maka buat instance baru
      try {
        self::$instance = new PDO(
          self::$driver . ':host=' . self::$config['DB_HOST'] . ';dbname=' . self::$config['DB_NAME'] . ';port=' . self::$config['DB_PORT'],
          self::$config['DB_USER'],
          self::$config['DB_PASS']
        );
      } catch (PDOException $e) {
        throw new Exception('Koneksi Gagal: ' . $e->getMessage());
      }
    }

    // hanya jika instance sudah dibuat, maka kembalikan instance
    return self::$instance;
  } 
}
```

- Selanjutnya kita membuat class __DB__ untuk melakukan query ke database. Class ini akan menerapkan konsep __Facade__ untuk memudahkan kita melakukan query ke database. Class ini akan mengambil instance dari class __DbConnection__ untuk melakukan query ke database.

Ketikkan kode berikut di file __DB.php__:

```php
<?php
class DB {
  private static PDO $instance;

  public static function changeDriver(string $driver): void {
    if (empty(self::$instance)) {
      DbConnection::setDriver($driver);
    } else {
      throw new Exception('Driver tidak bisa diubah setelah instance dibuat');
    }
  }

  // method untuk menjalankan query
  private static function execute($query, $params = []) {
    if (empty(self::$instance)) {
      self::$instance = DbConnection::getInstance();
    }

    $stmt = self::$instance->prepare($query);
    $stmt->execute($params);
    return $stmt;
  }

  // method untuk menjalankan query select
  public static function select($query, $params = []) {
    // validasi select query
    if (!preg_match('/^SELECT/i', $query)) {
      throw new Exception('Query harus SELECT');
    }

    $stmt = self::execute($query, $params);
    return $stmt->fetchAll(PDO::FETCH_ASSOC);
  }

  // method untuk menjalankan query insert
  public static function insert($query, $params = []) {
    // validasi insert query
    if (!preg_match('/^INSERT/i', $query)) {
      throw new Exception('Query harus INSERT');
    }

    $stmt = self::execute($query, $params);
    return self::$instance->lastInsertId();
  }

  // method untuk menjalankan query update
  public static function update($query, $params = []) {
    // validasi update query
    if (!preg_match('/^UPDATE/i', $query)) {
      throw new Exception('Query harus UPDATE');
    }

    $stmt = self::execute($query, $params);
    return $stmt->rowCount();
  }

  // method untuk menjalankan query delete
  public static function delete($query, $params = []) {
    // validasi delete query
    if (!preg_match('/^DELETE/i', $query)) {
      throw new Exception('Query harus DELETE');
    }

    $stmt = self::execute($query, $params);
    return $stmt->rowCount();
  }

  // method untuk menjalankan query lainnya
  public static function query($query, $params = []) {
    $stmt = self::execute($query, $params);
    return $stmt;
  }
}
```

- Di class __`DB`__ kita menggunakan method __`execute()`__ untuk menjalankan query. Method ini akan mengambil instance dari class __DbConnection__ untuk melakukan query ke database. Method ini akan menerima dua parameter, yaitu query dan parameter yang bersifat opsional. Selain itu, method ini juga akan melakukan validasi query yang akan dijalankan. Jika query yang dijalankan bukan query `SELECT`, `INSERT`, `UPDATE`, atau `DELETE`, maka akan muncul pesan error.

- Untuk melakukan `query` kita menggunakan prepared statement. Prepared statement akan mempersiapkan query yang akan dijalankan dan menghindari SQL Injection. Untuk lebih jelasnya, silahkan baca artikel berikut: [PHP PDO Prepared Statement](https://www.petanikode.com/php-pdo-prepared-statement/). Untuk melakukan prepared statement, kita menggunakan method __`prepare()`__ dan __`execute()`__ yang disediakan oleh class __PDO__.

- Untuk menggunakan class __DB__, kita tinggal panggil method yang tersedia di class tersebut. Contoh penggunaan class __DB__ adalah dengan membuat file __index.php__ dan ketikkan kode berikut:

```php
<?php
spl_autoload_register(function ($class) {
  require_once 'classes/' . $class . '.php';
});

try {
  // ambil data dari tabel buku
  $buku = DB::select('SELECT * FROM buku');
  print_r($buku);

  // tambah data ke tabel buku
  $id = DB::insert('INSERT INTO buku VALUES (?, ?, ?, ?)', [NULL, 'Belajar PHP', 2022, 'Petani Kode']);

  // lihat data yang baru ditambahkan
  $buku = DB::select('SELECT * FROM buku WHERE id = ?', [$id]);
  print_r($buku);
} catch (Exception $e) {
  echo $e->getMessage();
}
```

### Menggunakan Composer

- Kita akan menggunakan composer menginstall package __faker__ sekaligus mengubah spl_autoload_register menjadi __autoload__ dari composer. __faker__ adalah package yang digunakan untuk menghasilkan data dummy. Install package __faker__ dengan perintah berikut:

```bash
composer require fakerphp/faker
```

- Tambahkan di `package.json` untuk memasukkan folder `clasess` ke dalam autoload:

```json
"autoload": {
  "classmap": [
    "classes/"
  ]
}
```

- Jalankan perintah berikut untuk mengupdate autoload:

```bash
composer dump-autoload
```

- Ubah kode __autoload__ di file __index.php__ menjadi seperti berikut:

```php
<?php
require_once 'vendor/autoload.php';

$faker = Faker\Factory::create();

try {
  $id = DB::insert('INSERT INTO buku (judul, tahun_terbit, pengarang) VALUES (?, ?, ?)', [$faker->text(), $faker->randomNumber(4, true), $faker->name()]);

  $buku = DB::select('SELECT * FROM buku');
} catch(Exception $e) {
  echo $e->getMessage();
}
...
```

## Tugas

- Buatlah table __`users`__ dengan struktur seperti berikut:

| Field | Type | Null | Key | Default | Extra |
| --- | --- | --- | --- | --- | --- |
| id | int(11) | NO | PRI | NULL | auto_increment |
| name | varchar(255) | NO | | NULL | |
| email | varchar(255) | NO | | NULL | |
| password | varchar(255) | NO | | NULL | |

- Masukkan data dummy ke table __`users`__ dengan menggunakan package __faker__ dan class __`DB`__. Data dummy yang dimasukkan adalah __name__, __email__, dan __password__. Password yang dimasukkan adalah hasil enkripsi dari __email__ yang dimasukkan.










