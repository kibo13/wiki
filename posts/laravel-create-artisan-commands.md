## Создание собственных artisan-команд 

**Artisan** - название интерфейса командной строки, с которым поставляется Laravel. Он содержит набор полезных команд, 
помогающие при разработке приложения. Он основан на мощном компоненте Symfony Console. 

### Использование

Чтобы вывести все доступные команды, используйте команду `list`:

```sh
php artisan list 
```

Каждая команда также включает и инструкцию, которая отображает и описывает доступные аргументы и опции для команды. 
Для того, чтобы её вывести, просто добавьте слово `help` перед командой:

```sh
php artisan help migrate
```

Также можно указать среду выполнения, в которой будет выполнена команда, при помощи опции `--env`:

```sh
php artisan migrate --env=local
```

При помощи опции `--version` можно посмотреть текущую версию приложения 

```sh
php artisan --version
```

### Способы создания команд 

Существует два способа создания: 
- на основе **Класса**
- на основе **Замыкания**

Команда-Класс создается через команду `make:command` и генерирует Класс в папку `app/Console/Commands` 
и далее настраивается под необходимые требования.

Команда-Замыкание создается внутри файла `routes/console.php` приложения Laravel.

### Создание команды на основе Класса

Создадим команду для очистики `laravel.log` файла, который расположен в папке `storage/logs`.

Выполняем команду: 

```sh
php artisan make:command Log/ClearLogFile
```

Далее открываем файл `app\Console\Commands\Log\ClearLogFile.php` и редактируем код: 

```php
// Define command name
protected $signature = 'log:clear';

// Add description to your command
protected $description = 'Clear Laravel log file';

// Create your own custom command
public function handle()
{
    exec('echo "" > ' . storage_path('logs/laravel.log'));
    $this->info('Logs have been cleared');
}
```

Запускаем команду, как и другие команды:

```sh
php artisan log:clear
```
