1. Реализовать хранимую функцию/процедуру, возвращающую общее количество остатков заданного наименования товара в магазинах и на складах.
```sql
CREATE OR REPLACE FUNCTION get_total_stock(item_name TEXT)
RETURNS INT AS $$
DECLARE
    shop_count INT;
    warehouse_count INT;
BEGIN
    SELECT COALESCE(SUM(gs.count), 0)
    INTO shop_count
    FROM Good g
    JOIN Good_Shop gs ON g.id = gs.id_good
    WHERE g.name = item_name;
    SELECT COALESCE(SUM(gw.count), 0)
    INTO warehouse_count
    FROM Good g
    JOIN Good_Warehouse gw ON g.id = gw.id_good
    WHERE g.name = item_name;
    RETURN shop_count + warehouse_count;
END;
$$ LANGUAGE plpgsql;
```
Пример вызова:

```sql
SELECT get_total_stock('Apple iPhone 14');
```

**Результат:**  
`9`

![image](https://github.com/user-attachments/assets/ddbf9fad-8f11-4c93-82fa-8d91214218cf)


2. Реализовать хранимую функцию/процедуру, которая по наименованию товара и возрасту покупателя выводит сообщение о возможности приобретения данного товара данным покупателем.
```sql
CREATE OR REPLACE FUNCTION can_buy_good(item_name TEXT, buyer_age INT)
RETURNS TEXT AS $$
DECLARE
    required_age INT;
BEGIN
    SELECT gc.ageMin
    INTO required_age
    FROM Good g
    JOIN GoodCategory gc ON g.id_goodCategory = gc.id
    WHERE g.name = item_name;
    IF required_age IS NULL THEN
        RETURN 'Товар не найден';
    ELSIF buyer_age >= required_age THEN
        RETURN 'Покупка возможна';
    ELSE
        RETURN 'Покупка запрещена по возрастным ограничениям';
    END IF;
END;
$$ LANGUAGE plpgsql;
```
Пример вызова:

```sql
SELECT can_buy_good('Apple iPhone 14', 16);
```


**Результат:**  
`Покупка возможна`

![image-1](https://github.com/user-attachments/assets/931b01cb-9072-4cff-8132-abe6f7056168)


Пример вызова:

```sql
SELECT can_buy_good('Apple iPhone 14', 13);
```
**Результат:**  
`Покупка запрещена по возрастным ограничениям`

![image-2](https://github.com/user-attachments/assets/0e2c925f-788a-4565-951c-3c850deec720)


3. Реализовать хранимую функцию/процедуру, которая оценивает возможность покупки заданного количества товара в магазине (название магазина и товара задаются пользователем). При этом необходимо также проверить возрастные ограничения покупателя (используя функцию из п.2).

```sql
CREATE OR REPLACE FUNCTION can_purchase_in_shop(shop_name TEXT, item_name TEXT, quantity INT, buyer_age INT)
RETURNS TEXT AS $$
DECLARE
    available_stock INT;
    required_age INT;
    age_check TEXT;
BEGIN
    SELECT gs.count
    INTO available_stock
    FROM Good_Shop gs
    JOIN Good g ON gs.id_good = g.id
    JOIN Shop s ON gs.id_shop = s.id
    WHERE g.name = item_name AND s.name = shop_name;

    IF available_stock IS NULL OR available_stock < quantity THEN
        RETURN 'Недостаточно товара в магазине';
    END IF;

    SELECT gc.ageMin
    INTO required_age
    FROM Good g
    JOIN GoodCategory gc ON g.id_goodCategory = gc.id
    WHERE g.name = item_name;

    IF buyer_age < required_age THEN
        RETURN 'Покупка запрещена по возрастным ограничениям';
    END IF;

    RETURN 'Покупка возможна';
END;
$$ LANGUAGE plpgsql;
```
Пример вызова:
```sql
SELECT can_purchase_in_shop('МВидео', 'Apple iPhone 14', 1, 18);
```
**Результат:**  
`Покупка возможна`
![image-3](https://github.com/user-attachments/assets/68e29207-f3d7-466a-949e-c455804b10c3)


Пример вызова:
```sql
SELECT can_purchase_in_shop('МВидео', 'Apple iPhone 14', 10, 20);
```
**Результат:**  
`Недостаточно товара в магазине`

![image-4](https://github.com/user-attachments/assets/1f8f8ffe-c095-4e2c-97c3-bc4420a10459)


Пример вызова:
```sql
SELECT can_purchase_in_shop('МВидео', 'Apple iPhone 14', 1, 15);
```
**Результат:**  
`Покупка запрещена по возрастным ограничениям`
![image-5](https://github.com/user-attachments/assets/ac85d426-85dc-4262-861a-2a7bb48cd631)

