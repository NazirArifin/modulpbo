## SOAL

- Sebuah toko Pizza menjual 3 jenis pizza, yaitu: `Pepperoni`, `Cheese`, `Veggie`

- Setiap pizza memiliki nama, adonan, saus, topping dan harga. 
  - Topping dapat berupa: `Pepperoni`, `Cheese`, `Mushroom`, `Mozzarella`, `Cheddar`, `Parmesan`, `Asiago`
  - Adonan dapat berupa: `Regular Crust`, `Thick Crust`, `Thin Crust`
  - Saus dapat berupa: `Tomato Sauce`, `Barbeque Sauce`, `Alfredo Sauce`, `Marinara Sauce`, `Pesto Sauce`, `Ranch Sauce`, `Buffalo Sauce`

- Buatlah __`abstract class`__ Pizza yang memiliki property `nama`, `adonan`, `saus`, dan `topping` dengan constructor yang menerima nama dan adonan. Selain itu buatlah `getter` untuk masing-masing property.

- Buatlah method `panggangPizza()` yang akan memunculkan teks `Pizza [nama] dipanggang dengan adonan [adonan] selama [n] menit`. `n` adalah waktu yang dibutuhkan untuk memanggang pizza. Pizza dengan adonan `thin crust` membutuhkan waktu 10 menit, `regular crust` membutuhkan waktu 15 menit, dan `thick crust` membutuhkan waktu 20 menit.

- Buatlah abstract method `addTopping()` yang menerima parameter topping dan `addSauce()` yang menerima parameter saus.`panggangPizza`, `addTopping()` dan `addSauce()` harus mereturn objek Pizza dan dapat dipanggil berulang-ulang. Contohnya:

```php
$pizza->addTopping('Pepperoni')
  ->addTopping('Cheese')
  ->addSauce('Tomato Sauce')
  ->panggangPizza()
  ->addTopping('Mozzarella')
  ->addSauce('Barbeque Sauce');
```

- Buatlah class `PizzaPepperoni`, `PizzaCheese`, dan `PizzaVeggie` yang mewarisi class `Pizza`. 
- Setelah itu, buatlah class `PizzaStore` yang memiliki method `orderPizza()` yang menerima parameter jenis pizza dan mengembalikan objek pizza yang dipesan.

- Buatlah method `sajikanPizza(Pizza $pizza)` dan `hargaPizza(Pizza $pizza)` pada class `PizzaStore` yang menerima parameter object `Pizza` (polymorphism)
  - `sajikanPizza(Pizza $pizza)` memunculkan teks `Pizza [nama] dengan saus [saus] dan topping [topping] sudah siap dinikmati!`. 
  - `hargaPizza(Pizza $pizza)` menghitung harga pizza berdasarkan topping dan saus yang dipilih. Harga pizza:
    - 5000 (`thin crust`) + 5000 per topping + 5000 per saus
    - 10000 (`regular crust`) + 10000 per topping + 10000 per saus
    - 15000 (`thick crust`) + 15000 per topping + 15000 per saus


## JAWABAN

```php
<?php
abstract class Pizza {
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

  public function panggangPizza(): Pizza {
    $waktu = match ($this->adonan) {
      'thin crust' => 10,
      'regular crust' => 15,
      'thick crust' => 20,
    };
    echo "Pizza $this->nama dipanggang dengan adonan $this->adonan selama $waktu menit" . PHP_EOL;
    return $this;
  }
  abstract public function addTopping($topping): Pizza;
  abstract public function addSauce($saus): Pizza;
}

class PizzaPepperoni extends Pizza {
  public function addTopping($topping): Pizza {
    $this->topping[] = $topping;
    return $this;
  }

  public function addSauce($saus): Pizza {
    $this->saus[] = $saus;
    return $this;
  }
}

class PizzaCheese extends Pizza {
  public function addTopping($topping): Pizza {
    $this->topping[] = $topping;
    return $this;
  }

  public function addSauce($saus): Pizza {
    $this->saus[] = $saus;
    return $this;
  }
}

class PizzaVeggie extends Pizza {
  public function addTopping($topping): Pizza {
    $this->topping[] = $topping;
    return $this;
  }

  public function addSauce($saus): Pizza {
    $this->saus[] = $saus;
    return $this;
  }
}

class PizzaStore {
  public function orderPizza($jenis): Pizza {
    return match ($jenis) {
      'pepperoni' => new PizzaPepperoni('Pepperoni', 'thin crust'),
      'cheese' => new PizzaCheese('Cheese', 'regular crust'),
      'veggie' => new PizzaVeggie('Veggie', 'thick crust'),
    };
  }

  public function sajikanPizza(Pizza $pizza) {
    echo "Pizza {$pizza->getNama()} dengan saus {$pizza->getSaus()} dan topping {$pizza->getTopping()} sudah siap dinikmati!" . PHP_EOL;
  }

  public function hargaPizza(Pizza $pizza) {
    $harga = match ($pizza->getAdonan()) {
      'thin crust' => 5000,
      'regular crust' => 10000,
      'thick crust' => 15000,
    };
    $harga += count($pizza->getTopping()) * 5000;
    $harga += count($pizza->getSaus()) * 5000;
    echo "Harga pizza {$pizza->getNama()} adalah $harga" . PHP_EOL;
  }
}

$pizzaStore = new PizzaStore();
$pizza = $pizzaStore->orderPizza('pepperoni');
$pizza->addTopping('Pepperoni')
  ->addTopping('Cheese')
  ->addSauce('Tomato Sauce')
  ->panggangPizza()
  ->addTopping('Mozzarella')
  ->addSauce('Barbeque Sauce');
$pizzaStore->sajikanPizza($pizza);
$pizzaStore->hargaPizza($pizza);
```