## Оператор JOIN

**JOIN** - операция соединения, которая используется для извлечения данных из нескольких таблиц.

Для примера работы оператора будут использоваться следующие таблицы:

Таблица `suppliers`:

| SUPPLIER_ID | SUPPLIER_NAME |
| ----------- | ------------- |
| 100         | ASUS          |
| 101         | ACER          |
| 102         | HP            |
| 103         | LENOVO        |

Таблица `orders`:

| ORDER_ID | SUPPLIER_ID | ORDER_DATE |
| -------- | ----------- | ---------- |
| 3125     | 100         | 10.05.2022 |
| 3126     | 101         | 07.03.2022 |
| 3127     | 104         | 05.01.2022 |

### INNER JOIN (внутреннее соединение)

Этот режим объединения результатов поиска в базах данных `SQL` включается автоматически.
Если не указывать тип `JOIN`, то сработает именно `INNER JOIN`. 
С помощью него можно указать сразу два критерия (две таблицы) и по ним отсеять контент.

```sql
SELECT suppliers.supplier_id,
       suppliers.supplier_name,
       orders.order_date
FROM suppliers
INNER JOIN orders ON suppliers.supplier_id = orders.supplier_id
```

Запрос вернет все строки из таблиц `suppliers` и `orders`, 
где имеются соответствующие значение поля `supplier_id` в обоих таблицах.

| SUPPLIER_ID | SUPPLIER_NAME | ORDER_DATE |
| ----------- | ------------- | ---------- |
| 100         | ASUS          | 10.05.2022 |
| 101         | ACER          | 07.03.2022 |

> Строки для `HP` и `LENOVO` из таблицы `suppliers` будут опущены, 
так как значения `supplier_id` 102 и 103 не существует в обеих таблицах. 

> Строка `order_id` 3127 из таблицы `orders` будет опущена, 
так как `supplier_id` 104 не существует в таблице `suppliers`.

### OUTER JOIN (внешнее соединение)

Соединение двух таблиц, в результат которого обязательно входят все строки либо одной, либо обеих таблиц.

Внешние запросы существуют в нескольких вариациях:

- `LEFT OUTER JOIN` (`LEFT JOIN`)
- `RIGHT OUTER JOIN` (`RIGHT JOIN`)
- `FULL OUTER JOIN` (`FULL JOIN`)

Каждый вариант по-своему обрабатывает информацию и в итоге выдает разные результаты.

### LEFT OUTER JOIN (LEFT JOIN)

Этот тип соединения возвращает все строки из левой таблицы, для которых не нашлось пары в правой таблице.

```sql
SELECT suppliers.supplier_id,
       suppliers.supplier_name,
       orders.order_date
FROM suppliers -- Левая таблица
LEFT OUTER JOIN orders -- Правая таблица 
     ON suppliers.supplier_id = orders.supplier_id
```

Запрос вернет все строки из таблицы `suppliers`, и только те строки из таблицы `orders`, где объединяемые поля равны.
Если значение `supplier_id` не существует в таблице `orders`, все поля таблицы `orders` 
будут отображаться в результирующем наборе как `NULL`.

| SUPPLIER_ID | SUPPLIER_NAME | ORDER_DATE |
| ----------- | ------------- | ---------- |
| 100         | ASUS          | 10.05.2022 |
| 101         | ACER          | 07.03.2022 |
| 102         | HP            | NULL       |
| 103         | LENOVO        | NULL       |

### RIGHT OUTER JOIN (RIGHT JOIN)

Этот тип соединения будет работать в обратную сторону и вернет все строки из правой таблицы, 
для которых не нашлось пары в левой.

```sql
SELECT suppliers.supplier_id,
       suppliers.supplier_name,
       orders.order_id,
       orders.order_date
FROM suppliers -- Левая таблица
RIGHT OUTER JOIN orders -- Правая таблица 
     ON suppliers.supplier_id = orders.supplier_id
```

Запрос вернет все строки из таблицы `orders`, и только те строки из таблицы `suppliers`, где объединяемые поля равны.
Если значение `supplier_id` не существует в таблице `suppliers`, все поля в таблице `suppliers` 
будут отображаться в результирующем наборе как `NULL`.

| SUPPLIER_ID | SUPPLIER_NAME | ORDER_ID | ORDER_DATE |
| ----------- | ------------- | -------- | ---------- |
| 100         | ASUS          | 3125     | 10.05.2022 |
| 101         | ACER          | 3126     | 07.03.2022 |
| NULL        | NULL          | 3127     | 05.01.2022 |

### FULL OUTER JOIN (FULL JOIN)

Этот тип соединения возвращает все строки из левой таблицы и правой таблицы с `NULL` - значениями в месте, 
где условие объединения не выполняется.

```sql
SELECT suppliers.supplier_id,
       suppliers.supplier_name,
       orders.order_id,
       orders.order_date
FROM suppliers -- Левая таблица
FULL OUTER JOIN orders -- Правая таблица 
     ON suppliers.supplier_id = orders.supplier_id
```

Запрос вернет все строки из таблиц `suppliers` и `orders` и всякий раз, когда условие соединения не выполняется, 
то поля в результирующем наборе будут принимать значения `NULL`. 

| SUPPLIER_ID | SUPPLIER_NAME | ORDER_ID | ORDER_DATE |
| ----------- | ------------- | -------- | ---------- |
| 100         | ASUS          | 3125     | 10.05.2022 |
| 101         | ACER          | 3126     | 07.03.2022 |
| 102         | HP            | NULL     | NULL       |
| 103         | LENOVO        | NULL     | NULL       |
| NULL        | NULL          | 3127     | 05.01.2022 |
