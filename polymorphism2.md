## SOAL

- Sebuah toko Pizza menjual 3 jenis pizza, yaitu: `Pepperoni`, `Cheese`, `Veggie`

- Setiap pizza memiliki nama, adonan, saus, topping dan harga. 
  - Topping dapat berupa: `Pepperoni`, `Cheese`, `Mushroom`, `Mozzarella`, `Cheddar`, `Parmesan`, `Asiago`
  - Adonan dapat berupa: `Regular Crust`, `Thick Crust`, `Thin Crust`
  - Saus dapat berupa: `Tomato Sauce`, `Barbeque Sauce`, `Alfredo Sauce`, `Marinara Sauce`, `Pesto Sauce`, `Ranch Sauce`, `Buffalo Sauce`

- Buatlah __`abstract class`__ Pizza yang memiliki property `nama`, `adonan`, `saus`, dan `topping` dengan constructor yang menerima nama dan adonan. Selain itu buatlah `getter` untuk masing-masing property.

- Buatlah method `panggangPizza()` yang akan memunculkan teks `Pizza [nama] dipanggang dengan adonan [adonan] selama [n] menit`. `n` adalah waktu yang dibutuhkan untuk memanggang pizza. Pizza dengan adonan `thin crust` membutuhkan waktu 10 menit, `regular crust` membutuhkan waktu 15 menit, dan `thick crust` membutuhkan waktu 20 menit.

- Buatlah method `addTopping()` yang menerima parameter topping dan `addSauce()` yang menerima parameter saus.`panggangPizza`, `addTopping()` dan `addSauce()` harus mereturn objek Pizza dan dapat dipanggil berulang-ulang. Contohnya:

```php
$pizza->addTopping('Pepperoni')
  ->addTopping('Cheese')
  ->addSauce('Tomato Sauce')
  ->panggangPizza()
  ->addTopping('Mozzarella')
  ->addSauce('Barbeque Sauce');
```

- Buatlah abstract method `hitungHarga(Pizza $pizza)` yang jika dioverride akan menghitung harga pizza berdasarkan adonan, topping dan saus yang dipilih. Harga pizza:
  - Pizza `Pepperoni`:
    - 5000 + 5000 per topping + 5000 per saus
  - Pizza `Cheese`:
    - 10000 + 10000 per topping + 10000 per saus
  - Pizza `Veggie`:
    - 15000 + 15000 per topping + 15000 per saus

- Buatlah class `PizzaPepperoni`, `PizzaCheese`, dan `PizzaVeggie` yang mewarisi class `Pizza`. 

- Setelah itu, buatlah class `PizzaStore` yang memiliki method `orderPizza()` yang menerima parameter jenis pizza dan adonan lalu mengembalikan objek pizza yang dipesan. Tambahkan method `sajikanPizza(Pizza $pizza)` dan `hargaPizza(Pizza $pizza)` menerima parameter object `Pizza` (polymorphism).
  - `sajikanPizza(Pizza $pizza)` memunculkan teks `Pizza [nama] dengan saus [saus] dan topping [topping] sudah siap dinikmati!`.
  - `hargaPizza(Pizza $pizza)` memunculkan teks `Harga pizza [nama] adalah [harga]`.

## JAWABAN

```php
<?php
class Pizza {
  protected string $nama;
  protected string $adonan;
  protected array $saus = [];
  protected array $topping = [];

  public function __construct($nama, $adonan) {
    $this->nama = $nama;
    $this->adonan = $adonan;
  }

  public function getNama() {
    return $this->nama;
  }

  public function getAdonan() {
    return $this->adonan;
  }

  public function getSaus() {
    return $this->saus;
  }

  public function getTopping() {
    return $this->topping;
  }

  public function addTopping($topping): Pizza {
    $this->topping[] = $topping;
    return $this;
  }

  public function addSauce($saus): Pizza {
    $this->saus[] = $saus;
    return $this;
  }

  public function panggangPizza(): Pizza {
    // gunakan match
    $waktu = match($this->adonan) {
      'thin crust' => 10,
      'regular crust' => 15,
      'thick crust' => 20,
    };
    echo "Pizza {$this->nama} dipanggang dengan adonan {$this->adonan} selama {$waktu} menit" . PHP_EOL;
    return $this;
  }

  abstract public function hitungHarga(Pizza $pizza);
}

class PizzaPepperoni extends Pizza {
  public function hitungHarga(Pizza $pizza): int {
    $harga = 5000;
    $harga += count($pizza->getTopping()) * 5000;
    $harga += count($pizza->getSaus()) * 5000;
    return $harga;
  }
}

class PizzaCheese extends Pizza {
  public function hitungHarga(Pizza $pizza): int {
    $harga = 10000;
    $harga += count($pizza->getTopping()) * 10000;
    $harga += count($pizza->getSaus()) * 10000;
    return $harga;
  }
}

class PizzaVeggie extends Pizza {
  public function hitungHarga(Pizza $pizza): int {
    $harga = 15000;
    $harga += count($pizza->getTopping()) * 15000;
    $harga += count($pizza->getSaus()) * 15000;
    return $harga;
  }
}

class PizzaStore {
  public function orderPizza($nama, $adonan): Pizza {
    $pizza = match($nama) {
      'Pepperoni' => new PizzaPepperoni($nama, $adonan),
      'Cheese' => new PizzaCheese($nama, $adonan),
      'Veggie' => new PizzaVeggie($nama, $adonan),
    };
    return $pizza;
  }

  public function sajikanPizza(Pizza $pizza) {
    echo "Pizza {$pizza->getNama()} dengan saus {$pizza->getSaus()} dan topping {$pizza->getTopping()} sudah siap dinikmati!" . PHP_EOL;
  }

  public function hargaPizza(Pizza $pizza) {
    echo "Harga pizza {$pizza->getNama()} adalah {$pizza->hitungHarga($pizza)}" . PHP_EOL;
  }
}

$pizzaStore = new PizzaStore();
$pizza = $pizzaStore->orderPizza('Pepperoni', 'thin crust');
$pizza->addTopping('mushroom')
  ->addTopping('olive')
  ->addSauce('tomato sauce')
  ->panggangPizza()
  ->addTopping('pepperoni')
  ->addSauce('barbecue sauce');
$pizzaStore->sajikanPizza($pizza);
$pizzaStore->hargaPizza($pizza);
```

## SOAL

- Sebuah toko NasiGoreng menyediakan 3 jenis nasi goreng, yaitu `nasi goreng jawa`, `nasi goreng bali`, dan `nasi goreng kalimantan`.

- Setiap nasi goreng memiliki nama, bumbu, saus, topping dan harga
  - Topping dapat berupa: `telur`, `ayam`, `sosis`, `seafood` dan `keju`
  - Saus dapat berupa: `kacang`, `kecap`, `sambal`, `kacang ijo`, `kacang merah`, `kacang hitam`, `saus pedas`
  - Bumbu dapat berupa: `Bumbu pedas`, `Bumbu hitam` dan `Bumbu kacang`

- Buatlah `interface` `NasiGoreng` yang memiliki method `getNama()`, `getBumbu()`, `getSaus()`, `getTopping()`, dan `getHarga()`

- Tambahkan method `goreng()` pada `interface` `NasiGoreng` yang mereturn `NasiGoreng` dan memunculkan teks `Nasi goreng [nama] dengan bumbu [bumbu], saus [saus], dan topping [topping] sedang digoreng selama [n] menit`. Nilai `n` adalah waktu yang dibutuhkan untuk menggoreng nasi goreng tersebut. Waktu yang dibutuhkan untuk menggoreng nasi goreng adalah: 10 menit untuk `nasi goreng jawa`, 15 menit untuk `nasi goreng bali`, dan 20 menit untuk `nasi goreng kalimantan`

- Tambahkan method `addTopping($topping)` pada `interface` `NasiGoreng` yang mereturn `NasiGoreng` dan menambahkan topping pada nasi goreng tersebut. Selanjutnya juga tambahkan method `addSauce($saus)` pada `interface` `NasiGoreng` yang mereturn `NasiGoreng` dan menambahkan saus pada nasi goreng tersebut. Contoh pemanggilan:

```php
$nasiGorengJawa->addTopping('telur')
  ->addTopping('ayam')
  ->addToping('sosis')
  ->addSauce('kacang')
  ->addSauce('kecap')
  ->goreng();
```

- Buatlah `class` `NasiGorengJawa`, `NasiGorengBali`, dan `NasiGorengKalimantan` yang mengimplementasikan `interface` `NasiGoreng`

- Buatlah `class` `NasiGorengStore` yang memiliki method `orderNasiGoreng($jenis, $bumbu)`, `sajikanNasiGoreng(NasiGoreng $nasiGoreng)`, dan `hargaNasiGoreng(NasiGoreng $nasiGoreng)` (polymorphism)

- Tambahkan method `sajikanNasiGoreng(NasiGoreng $nasiGoreng)` pada `class` `NasiGorengStore` yang memunculkan teks `Nasi goreng [nama] dengan bumbu [bumbu], saus [saus], dan topping [topping] sudah siap dinikmati!`

- Tambahkan method `hargaNasiGoreng(NasiGoreng $nasiGoreng)` pada `class` `NasiGorengStore` yang memunculkan teks `Harga nasi goreng [nama] adalah [harga]`. Harga nasi goreng adalah: 10000 untuk `nasi goreng jalwa`, 15000 untuk `nasi goreng bali`, dan 20000 untuk `nasi goreng kalimantan`. Selain itu, tambahkan harga topping dan saus. Harga topping adalah 5000 dan harga saus adalah 3000
