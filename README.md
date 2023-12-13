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

## Урок 2. SQL – создание объектов, простые запросы выборки

### Задача 1

Создать таблицу sales с тремя столбцами: id, order_date (дата заказа) и count_product (количество продуктов в заказе). Затем заполнить эту таблицу данными, включая информацию о дате заказа и количестве продуктов в каждом заказе.
Названия столбцов: order_date, count_product. Заполните ее данными.

* DROP TABLE IF EXISTS itresume587820.sales;
    CREATE TABLE itresume587820.sales (
        id SERIAL PRIMARY KEY,
        order_date DATE NOT NULL,
        count_product INTEGER NOT NULL
    );
    INSERT INTO itresume587820.sales (order_date, count_product) VALUES
    ('2022-01-01', 156),
    ('2022-01-02', 180),
    ('2022-01-03', 21),
    ('2022-01-04', 124),
    ('2022-01-05', 341);

### Задача 2

Для данных таблицы sales укажите тип заказа в зависимости от кол-ва :

меньше 100 - Маленький заказ,
от 100 до 300 - Средний заказ,
больше 300 - Большой заказ.
Выведите таблицу с названиями столбцов Номер заказа, Количество продукта, Тип

* SELECT * ,
 CASE
  WHEN count_product < 100 THEN "Маленький заказ"
        WHEN count_product > 100 and count_product < 300 THEN "Средний заказ"
        WHEN count_product > 300 THEN "Большой заказ"
        ELSE "Нет в наличии"
 END AS type
FROM sales;

### Задача 3

* Используя операторы языка SQL, создайте таблицу orders, заполните ее значениями.
Названия столбцов: employee_id, amount, order_status.
'e03', '15.00', 'OPEN'
'e01', '25.50', 'OPEN'
'e05', '100.70', 'CLOSED'
'e02', '22.18', 'OPEN'
'e04', '9.50', 'CANCELLED'
Важно: Чтобы проверка прошла успешно, перед нажатием кнопки Проверить студент должен написать запрос и нажать кнопку Выполнить.

### Задача 4

Имеется база данных – социальная сеть.

База данных содержит сущности:

* users – пользователи;
* messages – сообщения;
* friend_requests – заявки на дружбу;
* communities – сообщества;
* users_communities – пользователи сообществ;
* media_types – типы медиа;
* media – медиа;
* likes – лайки;
* profiles – профили пользователя.

У сущности «заявки на дружбу» имеются следующие поля(атрибуты):

* initiator_user_id – инициатор;
* target_user_id – получатель;
* status - статус;
* requested_at - дата создания;
* updated_at - дата последнего обновления.

У сущности «пользователи» имеются следующие поля(атрибуты):

* id – идентификатор;
* firstname – имя;
* lastname - фамилия;
* email - адрес электронной почты.

1. Найдите общее количество лайков, которые получили пользователи женского пола.

* SELECT COUNT(likes.media_id ) AS sum_likes FROM profiles  
LEFT JOIN likes ON profiles.user_id = likes.user_id;

2. Найдите количество лайков, которые поставили пользователи женского пола и мужского пола.
Выведите название пола (преобразовав значение атрибута f в женский, а значение ‘m` - мужской) и количество, поставленных лайков, применив к количеству лайков сортировку по убыванию.

* SELECT
  CASE profiles.gender
  WHEN 'f' THEN 'женский'
  WHEN 'm' THEN 'мужской'
END AS "пол пользователя",
COUNT(likes.user_id) as "количество лайков"
FROM likes LEFT JOIN profiles
ON likes.user_id = profiles.user_id
GROUP BY profiles.gender;

3. Выведите идентификаторы пользователей, которые не отправляли ни одного сообщения.

* SELECT profiles.user_id FROM profiles LEFT JOIN messages
ON profiles.user_id = messages.from_user_id
WHERE messages.from_user_id IS NULL
ORDER BY from_user_id;

4. Друзья — это пользователи у которых имеется соответствующая запись (заявка) в сущности «заявки на дружбу» и в атрибуте status данной сущности указано значение 'approved'.
Найдите количество друзей у каждого пользователя. Выведите для каждого пользователя идентификатор пользователя, фамилию, имя и количество друзей. Сортировка выводимых записей в порядке возрастания идентификатора пользователя.

* SELECT users.id, users.firstname, users.lastname,
SUM(friend_requests.status = 'approved') AS 'CSA'
FROM users LEFT JOIN friend_requests ON users.id = friend_requests.initiator_user_id
    or users.id = friend_requests.target_user_id
GROUP BY users.id
ORDER BY users.id;

### Задача 5

База такая же как в 4 дз

1. Найти количество сообщений, отправленных каждым пользователей.
В зависимости от количества отправленных сообщений рассчитать ранг пользователей, первое место присвоив пользователю(ям) с наибольшим количеством отправленных сообщений.
Вывести полученный ранг, имя, фамилия, пользователя и кол-во отправленных сообщений. Выводимый список необходимо отсортировать в порядке возрастания ранга.

* WITH count_message as
(
  SELECT
  users.firstname,
  users.lastname,
  COUNT(messages.to_user_id ) as "cnt"
  from users LEFT JOIN messages ON users.id = messages.from_user_id
  GROUP BY users.id
)
SELECT
DENSE_RANK() OVER(ORDER BY count_message.cnt DESC) as "rank_message",
count_message.firstname,
count_message.lastname,
count_message.cnt
from count_message;

2. Получите список сообщений, отсортированных по возрастанию даты отправки.
Вычислите разность между соседними значениями дат отправки. Разности выразите в минутах.
Выведите идентификатор сообщения, дату отправки, дату отправки следующего сообщения и разницу даты отправки соседних сообщений.

* WITH list_message as(
  SELECT
  messages.id,
  messages.created_at,
  LEAD(messages.created_at, 1, "None")OVER(ORDER BY messages.created_at) AS "lead_time"
  FROM messages
)
SELECT *,
TIMESTAMPDIFF(minute,list_message.created_at, list_message.lead_time) as "minute_lead_diff"
FROM list_message
ORDER BY list_message.id;

### Задача 6

1. Создайте функцию, которая принимает кол-во сек и формат их в кол-во дней часов. Пример: 123456 ->'1 days 10 hours 17 minutes 36 seconds '

* DELIMITER $$
CREATE PROCEDURE times(seconds INT)
BEGIN
    DECLARE days INT default 0;
    DECLARE hours INT default 0;
    DECLARE minutes INT default 0;

    WHILE seconds >= 84600 DO
    SET days = seconds / 84600;
    SET seconds = seconds % 84600;
    END WHILE;

    WHILE seconds >= 3600 DO
    SET hours = seconds / 3600;
    SET seconds = seconds % 3600;
    END WHILE;

    WHILE seconds >= 60 DO
    SET minutes = seconds / 60;
    SET seconds = seconds % 60;
    END WHILE;

SELECT days, hours, minutes, seconds;
END $$
DELIMITER ;

CALL times(123456);

2. Выведите только четные числа от 1 до 10. Пример: 2,4,6,8,10 Данная промежуточная аттестация оценивается по системе "зачет" / "не зачет" "Зачет" ставится, если слушатель успешно выполнил 1 или 2 задачи "Незачет" ставится, если слушатель успешно выполнил 0 задач Критерии оценивания: 1 - слушатель верно создал функцию, которая принимает кол-во сек и формат их в кол-во дней часов. 2 - слушатель выведили только четные числа от 1 до 10.

* DELIMITER $$
CREATE PROCEDURE numbers()
BEGIN
    DECLARE n INT default 0;
    DROP TABLE IF EXISTS nums;
    CREATE TABLE nums (n INT);

    WHILE n < 10 DO
    SET n = n + 2;
    INSERT INTO nums VALUES(n);
    END WHILE;

SELECT * FROM nums;
END $$
DELIMITER ;

CALL numbers();
