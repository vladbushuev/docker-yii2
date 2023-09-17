# Docker for development Yii2
Сборка докера для разработки на фреймворке Yii2.

## Контейнеры
* NGINX
* PHP (7 или 8)
* MARIADB 10
* MAILHOG
* ELASTICSEARCH (7 или 8)

## Установка
Клонируйте данный репозиторий
```bash
$ git clone git@github.com:vladbushuev/docker-yii2.git
```

В файле **.env** указываем свои настройки: название проекта, версию php и elasticsearch, пользователя и группу под которым работает php, свободный диапазон IP (нужно для корректной работы xdebug под PHPStorm).

В файле **docker-compose.yml** оставляем необходимые контейнеры, остальные удаляем. В папке ***config/nginx*** необходимо выбрать шаблон Yii2: basic или advanced. И переименовать файл в **site.conf**.

## Использование
В MacOS и Linux можно использовать **Makefile**:
```bash
$ make up
$ make down
$ make bash
```

Под Windows:
```bash
docker-compose up --build -d
docker-compose down --remove-orphans
docker-compose exec --user=php-fpm bash
```