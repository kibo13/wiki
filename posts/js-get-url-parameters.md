## Получение параметров из адресной строки

Параметры URL (также называемые параметрами строки запроса или переменными URL) 
используются для передачи небольших объемов данных со страницы на страницу или с клиента на сервер 
через URL. Они могут содержать всевозможную полезную информацию, 
например, поисковые запросы, ссылки, информацию о продукте, предпочтения пользователя и многое другое. 

В современных браузерах это стало намного проще благодаря интерфейсу URLSearchParams. 
Он определяет множество вспомогательных методов для работы со строкой запроса URL.

Рассмотрим пример со следующим URL: 

```sh
https://example.com/?product=shirt&color=blue
```

Получаем строку запроса с помощью `window.location.search`:

```js
const queryString = window.location.search;

console.log(queryString);   // ?product=shirt&color=blue
```

Разбираем параметры строки запроса с помощью `URLSearchParams`:

```js
const urlParams = new URLSearchParams(queryString);
```

Затем можно воспользоваться методом, например, `get()` для получения данных: 

```js
const product = urlParams.get('product')
const size    = urlParams.get('size')

console.log(product);   // shirt
console.log(size);      // empty string
```

