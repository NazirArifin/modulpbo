# Modul 10 - Builder Pattern (PHP Version)

Tujuan Pembelajaran: Mahasiswa dapat memahami konsep Builder Pattern dan mampu mengimplementasikannya dalam bahasa pemrograman PHP.

## Persiapan

- Pastikan bahwa database MySQL dan PostgreSQL sudah terinstall di komputer Anda dan service MySQL dan PostgreSQL sudah berjalan. Untuk koneksi dan konfigurasi database, silakan lihat [modul sebelumnya](https://github.com/NazirArifin/modulpbo/blob/master/db_connection.md).

## Materi

![](https://refactoring.guru/images/patterns/content/builder/builder-en.png?id=617612423ea3752477dc90929115b3ee)

### Builder Pattern

- Builder Pattern adalah sebuah pattern yang digunakan untuk membangun sebuah objek yang kompleks. Pattern ini memisahkan proses pembuatan objek dengan representasinya. Pattern ini juga memisahkan proses pembuatan objek dengan cara pembuatan objek itu sendiri.

- Pattern ini digunakan untuk membangun sebuah objek memiliki banyak atribut yang kompleks. Pattern ini juga digunakan untuk membangun sebuah objek yang memiliki atribut yang bersifat opsional. Objek akan dibangun secara bertahap dengan cara memanggil method-method yang telah disediakan.

- Contoh kasus adalah pembuatan sebuah objek `Rumah`. Rumah memiliki banyak atribut seperti jumlah kamar, jumlah kamar mandi, jumlah lantai, dan lain-lain. Pembuatan sebuah objek `Rumah` akan menjadi kompleks jika kita harus mengisi semua atributnya. Jika menggunakan `constructor` biasa, maka kita harus mengisi semua atributnya. Jika menggunakan `constructor` dengan parameter opsional, maka kita harus mengisi semua atributnya dengan nilai `null` atau `default` jika tidak ingin mengisi atribut tersebut.

```php
<?php
// rumah memiliki banyak atribut (jendela, pintu, jumlah kamar, jumlah kamar mandi, garasi, kolam renang, hiasan, taman)
class Rumah {
  public function __construct(int $jendela, int $pintu, int $kamar, int $kamar_mandi, ? bool $garasi, ? bool $kolam, ? bool $hiasan, ? bool $taman)
  {
    // ...
  }
}

// rumah sederhana tanpa garasi, tanpa kolam renang, tanpa hiasan, tanpa taman
$rumah_sederhana = new Rumah(4, 2, 2, 1, null, null, null, null);

// rumah lengkap
$rumah_lengkap = new Rumah(4, 2, 2, 1, true, true, true, true);
```

- Solusi dari penggunaan `constructor` biasa atau `constructor` dengan parameter opsional adalah dengan menggunakan `Builder Pattern`. Dengan menggunakan `Builder Pattern`, kita dapat membangun sebuah objek secara bertahap dengan cara memanggil method-method yang telah disediakan.

## Praktikum

- Kita akan membuat SQLQueryBuilder untuk menghasilkan query SQL. Kita akan membuat dua versi dari SQLQueryBuilder, yaitu MySQLQueryBuilder dan PostgreSQLQueryBuilder. Kita akan membuat dua versi ini karena query SQL yang digunakan oleh MySQL dan PostgreSQL berbeda-beda. Dengan menggunakan `Builder Pattern`, kita dapat membuat sebuah query SQL secara bertahap dengan cara memanggil method-method yang telah disediakan.

- Kita akan membuat sebuah class `SQLQueryBuilder` yang merupakan `abstract class` dalam folder `builder`. Class ini akan berisi method-method yang digunakan untuk membangun sebuah query SQL. Class ini juga akan berisi method-method untuk menghasilkan query SQL yang telah dibangun dan untuk mengeksekusi query SQL tersebut.

```php
<?php
abstract class SQLQueryBuilder {
  protected array $fields = [];
  protected array $tables = [];
  protected array $where = [];
  protected int $limit = 0;
  protected int $offset = 0;
  protected array $order = [];
  protected array $group = [];
  protected array $having = [];

  abstract public function select(array $fields): SQLQueryBuilder;  
  abstract public function from(string $table, string $alias): SQLQueryBuilder;

  abstract public function where(string $field, string $value, string $operator = '='): SQLQueryBuilder;
  abstract public function limit(int $limit, int $offset = 0): SQLQueryBuilder;
  abstract public function offset(int $offset): SQLQueryBuilder;

  abstract public function orWhere(string $field, string $value, string $operator = '='): SQLQueryBuilder;
  abstract public function andWhere(string $field, string $value, string $operator = '='): SQLQueryBuilder;

  // order can be called multiple times
  abstract public function order(string $field, string $direction = 'ASC'): SQLQueryBuilder;

  abstract public function group(string $field): SQLQueryBuilder;
  abstract public function having(string $field, string $value, string $operator = '='): SQLQueryBuilder;

  abstract public function getSQL(): string;
  abstract public function execute();
  abstract public function reset(): SQLQueryBuilder;
}
```

- Sekarang kita akan membuat `MySQLQueryBuilder` dan `PostgreSQLQueryBuilder` dalam folder `builder` yang merupakan `concrete builder`. Class ini akan mengimplementasikan method-method yang telah disediakan oleh `SQLQueryBuilder`.

```php
<?php
class MySQLQueryBuilder extends SQLQueryBuilder {
  public function select(array $fields): SQLQueryBuilder {
    // panggil method reset() untuk menghapus query sebelumnya
    $this->reset();
    $this->fields = $fields;
    return $this;
  }
  
  public function from(string $table, string $alias): SQLQueryBuilder {
    $this->tables[] = [
      'table' => $table,
      'alias' => $alias
    ];
    return $this;
  }
  
  public function where(string $field, string $value, string $operator = '='): SQLQueryBuilder {
    $this->where[] = [
      'field' => $field,
      'value' => is_string($value) ? "'$value'" : $value,
      'operator' => $operator,
      'type' => 'AND'
    ];
    return $this;
  }
  public function limit(int $limit, int $offset = 0): SQLQueryBuilder {
    $this->limit = $limit;
    if ($offset) {
      $this->offset = $offset;
    }
    return $this;
  }
  public function offset(int $offset): SQLQueryBuilder {
    $this->offset = $offset;
    return $this;
  }

  public function orWhere(string $field, string $value, string $operator = '='): SQLQueryBuilder {
    $this->where[] = [
      'field' => $field,
      'value' => $value,
      'operator' => $operator,
      'type' => 'OR'
    ];
    return $this;
  }
  public function andWhere(string $field, string $value, string $operator = '='): SQLQueryBuilder {
    $this->where[] = [
      'field' => $field,
      'value' => $value,
      'operator' => $operator,
      'type' => 'AND'
    ];
    return $this;
  }

  // order can be called multiple times
  public function order(string $field, string $direction = 'ASC'): SQLQueryBuilder {
    $this->order[] = [
      'field' => $field,
      'direction' => $direction
    ];
    return $this;
  }

  public function group(string $field): SQLQueryBuilder {
    $this->group[] = $field;
    return $this;
  }
  public function having(string $field, string $value, string $operator = '='): SQLQueryBuilder {
    $this->having[] = [
      'field' => $field,
      'value' => $value,
      'operator' => $operator,
      'type' => 'AND'
    ];
    return $this;
  }

  public function getSQL(): string {
    $sql = 'SELECT ';
    $sql .= implode(', ', $this->fields);
    // susun table dan alias dari array
    $sql .= ' FROM ';
    $sql .= implode(', ', array_map(function($table) {
      // kadang ada table yang tidak punya alias
      if (empty($table['alias'])) {
        return $table['table'];
      }
      return $table['table'] . ' AS ' . $table['alias'];
    }, $this->tables));
    
    if ($this->where) {
      $sql .= ' WHERE ';
      // first where tidak perlu AND/OR, lainnya harus,
      // gunakan array_keys dan array_map
      $sql .= implode(' ', array_map(function($key, $where) {
        if ($key === 0) {
          return $where['field'] . ' ' . $where['operator'] . ' ' . $where['value'];
        }
        return $where['type'] . ' ' . $where['field'] . ' ' . $where['operator'] . ' ' . $where['value'];
      }, array_keys($this->where), $this->where));
    }
    if ($this->order) {
      $sql .= ' ORDER BY ';
      $sql .= implode(', ', array_map(function($order) {
        return $order['field'] . ' ' . $order['direction'];
      }, $this->order));
    }
    if ($this->group) {
      $sql .= ' GROUP BY ';
      $sql .= implode(', ', $this->group);
    }
    if ($this->having) {
      $sql .= ' HAVING ';
      $sql .= implode(' ', array_map(function($having) {
        return $having['type'] . ' ' . $having['field'] . ' ' . $having['operator'] . ' ' . $having['value'];
      }, $this->having));
    }

    if ($this->limit) {
      $sql .= ' LIMIT ' . $this->limit;
    }
    if ($this->offset) {
      $sql .= ' OFFSET ' . $this->offset;
    }
    return $sql;
  }

  public function execute(): array {
    $sql = $this->getSQL();
    return DB::select($sql);
  }

  public function reset(): SQLQueryBuilder {
    $this->fields = [];
    $this->tables = [];
    $this->where = [];
    $this->limit = 0;
    $this->offset = 0;
    $this->order = [];
    $this->group = [];
    $this->having = [];
    return $this;
  }
}
```

- Class `PostgreSQLQueryBuilder` hampir sama dengan `MySQLQueryBuilder`, jadi kita hanya meng`extend` class `MySQLQueryBuilder`.

```php
<?php
class PostgreSQLQueryBuilder extends MySQLQueryBuilder {
  
}
```

- Sekarang kita akan membuat `Factory` untuk membuat `builder` yang sesuai dengan database yang digunakan. Class ini akan mengimplementasikan method `create` yang akan mengembalikan `builder` yang sesuai dengan database yang digunakan. Beri nama class `QueryBuilderFactory` dalam folder `classes`.

```php
<?php
// implementasi factory method untuk MySQLQueryBuilder dan PostgreSQLQueryBuilder
class QueryBuilderFactory {
  public static function create(string $db = 'mysql'): SQLQueryBuilder {
    if ($db === 'mysql') {
      return new MySQLQueryBuilder();
    }
    if ($db === 'pgsql') {
      return new PostgreSQLQueryBuilder();
    }
    throw new Exception('Database not supported');
  }
}
```

- Untuk mengubah autoload ketikkan `composer dump-autoload` pada terminal. Berikutnya isi dari file `index.php` adalah sebagai berikut.

```php
<?php
require_once 'vendor/autoload.php';

try {
  $sqlBuilder = QueryBuilderFactory::create();
  $buku = $sqlBuilder->select(['*'])->from('buku', '')->execute();
  print_r($buku);

  $buku = $sqlBuilder->select(['*'])->from('buku', '')->where('id', 1)->execute();
  print_r($buku[0]->judul);
} catch (Exception $e) {
  echo $e->getMessage();
}
```

- Jalankan file `index.php` pada terminal dengan perintah `php index.php`. Hasilnya adalah sebagai berikut.







