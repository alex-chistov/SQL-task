# Вариант 26

`1.	Реализовать хранимую процедуру, возвращающую текстовую строку, содержащую информацию о предприятии(идентификатор, название, область, дата, название подстанции и сумма к оплате для последнего потребления). Обработать ситуацию, когда предприятие ничего не потребляло.`

![image](https://github.com/user-attachments/assets/c32de931-0330-4318-8e73-bfbcbecf3100)


*Примеры использования:*

`1) Предприятие есть и оно делало покупки:`
![image-1](https://github.com/user-attachments/assets/12443780-b65c-43cd-8ed3-fca8e374d4ec)


`2) Предприятие есть, но покупок не делало:`

![image-2](https://github.com/user-attachments/assets/c1333ad9-6f72-4328-abd9-7f31b86b6e77)


`3) Предприятия нет:`

![image-3](https://github.com/user-attachments/assets/83389af9-b348-4750-889f-b2dfef6154cc)


`2. Добавить таблицу, содержащую списки подстанций, с которыми работают предприятия. При вводе потребление проверять предприятие (можно ли ему работать с данной подстанцией).`

![image-4](https://github.com/user-attachments/assets/9c68169a-ce1f-4bae-a27d-d327c7fa8673)


![image-21](https://github.com/user-attachments/assets/b4dad3fe-b574-4865-9679-5c00ddbb461b)


*Примеры использования*

`Успешная вставка:`

![image-6](https://github.com/user-attachments/assets/93f6e335-21fb-430b-8099-6de30d057907)


`Попытка вставить запись, когда предприятие не может работать с данной подстанцией:`

![image-7](https://github.com/user-attachments/assets/43250795-a492-4cc7-bebc-e878f367a4a6)


`3. Реализовать триггер такой, что при вводе строки в таблице потребления, если стоимость не указана, то она вычисляется.`

![image-8](https://github.com/user-attachments/assets/950b50cf-d186-4e57-9d3c-08e8cbe7d295)


*Примеры использования:*

`1) Вставка с неуказанным payment:`

![image-9](https://github.com/user-attachments/assets/73122f37-0113-4b01-8062-9c149d7cc8e9)


`2) Вставка с указанным вручную payment:`

![image-10](https://github.com/user-attachments/assets/02efd142-7d79-4b42-9ced-5342f946e20b)


`3) Вставка с отсутствием данных для расчёта:`

![image-11](https://github.com/user-attachments/assets/f905825d-3ff1-4d93-8311-ae70577fa20d)



`4. Создать представление (view), содержащее поля: № и дата потребления, название предприятия и подстанции, временной интервал, скидка и стоимость к оплате. Обеспечить возможность изменения скидки. При этом должна быть пересчитана стоимость к оплате.`

![image-12](https://github.com/user-attachments/assets/ce048869-e4ff-4c2f-b495-870747c30084)


![image-13](https://github.com/user-attachments/assets/05eeef84-d5f1-4955-a80c-2d7d6b0c1711)


![image-14](https://github.com/user-attachments/assets/a2830894-9d24-467a-8186-8695d703a90f)


*Примеры использования:*


![image-16](https://github.com/user-attachments/assets/b3a26c33-7c73-4e5c-9dce-092075c35ec6)


`Изменение скидки через представление`

![image-17](https://github.com/user-attachments/assets/129459e5-803a-4fe5-899b-e2c0f2e86bad)


![image-18](https://github.com/user-attachments/assets/7053f4fd-c21a-451e-984a-e0e547b78635)


![image-19](https://github.com/user-attachments/assets/cec23de9-863d-4f1d-91d7-bd87639a7223)


`Если попытаться обновить запись без указания нового значения скидки, сработает проверка:`

![image-20](https://github.com/user-attachments/assets/27d221d8-4b84-43d3-81dd-be1bc2e72f81)
