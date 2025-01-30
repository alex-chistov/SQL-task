`Статический курсор stock_cursor используется для получения количества товара в магазине`

`Параметризованный курсор age_cursor принимает параметр good_name для определения минимального возраста для покупки товара`
```sql
CREATE OR REPLACE FUNCTION can_purchase_in_shop(shop_name TEXT, item_name TEXT, quantity INT, buyer_age INT)
RETURNS TEXT AS $$
DECLARE
    stock_cursor CURSOR FOR 
        SELECT gs.count
        FROM Good_Shop gs
        JOIN Good g ON gs.id_good = g.id
        JOIN Shop s ON gs.id_shop = s.id
        WHERE g.name = item_name AND s.name = shop_name;
        
    age_cursor CURSOR (good_name TEXT) FOR 
        SELECT gc.ageMin
        FROM Good g
        JOIN GoodCategory gc ON g.id_goodCategory = gc.id
        WHERE g.name = good_name;

    available_stock INT;
    required_age INT;
BEGIN
    OPEN stock_cursor;
    FETCH stock_cursor INTO available_stock;
    CLOSE stock_cursor;

    IF available_stock IS NULL OR available_stock < quantity THEN
        RETURN 'Недостаточно товара в магазине';
    END IF;

    OPEN age_cursor(item_name);
    FETCH age_cursor INTO required_age;
    CLOSE age_cursor;

    IF buyer_age < required_age THEN
        RETURN 'Покупка запрещена по возрастным ограничениям';
    END IF;

    RETURN 'Покупка возможна';
END;
$$ LANGUAGE plpgsql;
```
**Пример вызова**
```sql
SELECT can_purchase_in_shop('МВидео', 'Apple iPhone 14', 1, 18);
```
**Результат:**  
`Покупка возможна`

![image](https://github.com/user-attachments/assets/daa2a5fb-9beb-4c29-a40e-8b82e51720ce)




**Пример вызова**
```sql
SELECT can_purchase_in_shop('МВидео', 'Apple iPhone 14', 10, 20);
```
**Результат:**  
`Недостаточно товара в магазине`





**Пример вызова**
```sql
SELECT can_purchase_in_shop('МВидео', 'Apple iPhone 14', 1, 15);
```
**Результат:**  
`Покупка запрещена по возрастным ограничениям`

![image](https://github.com/user-attachments/assets/d930509b-6fbc-4824-adb0-c0e490a922c3)

