# Tinkoff bank library
Простая библиотека для приема платежей через интернет для Тинькофф банк.

### Возможности

 * Генерация URL для оплаты товаров
 * Подтверждение платежа
 * Просмотр статуса платежа
 * Отмена платежа

### Установка

С помощью [Composer](https://getcomposer.org/):

```bash
composer require erriourru/laravel-tinkoff
```

Подключение в контроллере:

```php
use ErriourRU\Tinkoff;
```

## Примеры использования
### 1. Инициализация

```php
$api_url    = 'https://securepay.tinkoff.ru/v2/';
$terminal   = '152619634343';
$secret_key = 'terminal_secret_password';

$tinkoff = new Tinkoff($api_url, $terminal, $secret_key);
```

### 2. Получить URL для оплаты
```php
//Подготовка массива с данными об оплате
$payment = [
    'OrderId'       => '123456',        //Ваш идентификатор платежа
    'Amount'        => '100',           //сумма всего платежа в рублях
    'Language'      => 'ru',            //язык - используется для локализации страницы оплаты
    'Description'   => 'Some buying',   //описание платежа
    'Email'         => 'user@email.com',//email покупателя
    'Phone'         => '89099998877',   //телефон покупателя
    'Name'          => 'Customer name', //Имя покупателя
    'Taxation'      => 'usn_income'     //Налогооблажение
];

//подготовка массива с покупками
$items[] = [
    'Name'  => 'Название товара',
    'Price' => '100',    //цена товара в рублях
    'NDS'   => 'vat20',  //НДС
];

//Получение url для оплаты
$paymentURL = $tinkoff->paymentURL($payment, $items);

//Контроль ошибок
if(!$paymentURL){
  echo($tinkoff->error);
} else {
  $payment_id = $tinkoff->payment_id;
  return redirect($result['payment_url']);
}
```

### 3. Получить статус платежа
```php
//$payment_id Идентификатор платежа банка (полученый в пункте "2 Получить URL для оплаты")

$status = $tinkoff->getState($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

### 4. Отмена платежа
```php
$status = $tinkoff->cencelPayment($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

### 5. Подтверждение платежа
```php
$status = $tinkoff->confirmPayment($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```