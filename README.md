# Урок 1. Установка СУБД, подключение к БД, просмотр и создание таблиц

## Создание таблицы mobile_phones

### Задача 1

Создайте таблицу с мобильными телефонами mobile_phones, используя графический интерфейс. Заполните БД данными.
Названия столбцов: product_name, manufacturer, product_count, price.

* DROP TABLE IF EXISTS {schema_name}.mobile_phones;
    -- создаём таблицу с мобильными телефонами (mobile_phones)
    CREATE TABLE {schema_name}.mobile_phones (
        id SERIAL PRIMARY KEY,
        product_name VARCHAR(100) NOT NULL,
        manufacturer VARCHAR(100) NOT NULL,
        product_count INT,
        price BIGINT
    );

-- Наполнение таблицы

* INSERT INTO {schema_name}.mobile_phones (product_name, manufacturer, product_count, price)
    VALUES
        ('iPhone X', 'Apple', 3, 76000),  
        ('iPhone 8', 'Apple', 2, 51000),  
        ('Galaxy S9', 'Samsung', 2, 56000),  
        ('Galaxy S8', 'Samsung', 1, 41000),
        ('P20 Pro', 'Huawei', 5, 36000);

### Задача 2

У вас есть таблица с мобильными телефонами mobile_phones.

Вывести название (product_name), производителя (manufacturer) и цену (price) для товаров из базы данных, у которых количество (product_count) больше чем 2.

* SELECT product_name, manufacturer, price FROM mobile_phones
    WHERE product_count > 2;

### Задача 3

Выведите весь ассортимент товаров марки Samsung из таблицы mobile_phones, и вывести поля id, product_name, manufacturer, product_count и price для этих записей.

* SELECT id, product_name, manufacturer, product_count, price
    FROM mobile_phones
    WHERE manufacturer = 'Samsung';
