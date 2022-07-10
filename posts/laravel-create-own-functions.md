## Создание собственных функций 

Иногда бывает нужна функция, которую можно будет вызвать из любого места в приложении.

### Создание сервис-провайдера 

```sh
php artisan make:provider HelperServiceProvider
```

В результате появится новый файл в `app\Providers` под названием `HelperServiceProvider.php`.

Метод `boot()` не будет использоваться поэтому удаляем его. 

Перепишем функцию `register()`:

```php
public function register()
{
    foreach (glob(app_path('Helpers') . '/*.php') as $file) {
        require_once $file;
    }
}
```

> Функция в цикле прокручивает все файлы, находящиеся в каталоге `app/Helpers`. Внутри данного каталога 
> можно создавать php-файлы, которые будут автоматически подгружаться в приложение. В этих файлах и будут размещаться
> ***helpers*** (вспомогательные функции), которые будут доступны в любой части кода.

### Подключение сервис-провайдера
Открываем файл `config/app.php` и добавляем `HelperServiceProvider` над `AppServiceProvider`:

```php
...

App\Providers\HelperServiceProvider::class,
App\Providers\AppServiceProvider::class,
App\Providers\AuthServiceProvider::class,
App\Providers\BroadcastServiceProvider::class

...
```
### Создание тестовой функции
Создаем новый файл с именем `Icon.php` в папке `app/Helpers` со следующим содержимым:

```php
function fa($icon)
{
    echo '<i class="fa ' . $icon . '"></i>';
}
```

Теперь можно использовать функцию `fa()` в любом месте приложения.