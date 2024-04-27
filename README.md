## Окружение для разработки (MAGENTO 2.3.7-p4) php-nginx-mysql-xdebug (PHP-7.4)

1. Выкачиваем себе репозиторий.
2. Папка с сайтом проекта теперь отдельно `./data/www`.
3. Выдаём права на файлы: 
	`
	chmod +x start stop build reload status
	`
	Описание команд:
	- Запускаем билд: `./build`
	- Чекаем статуc окружения: `./status`
	- Запускаем окружение: `./start`
	- Останавливаем окружение: `./stop`
	- Перезагружаем окружение: `./reload`
4. Билдим окружение: `./build`
5. Данные от БД:
      - `MYSQL_ROOT_PASSWORD=root`
      - `MYSQL_DATABASE=magento`
      - `MYSQL_USER=magento`
      - `MYSQL_PASSWORD=magento`

Чистая установка:

```
COMPOSER_MEMORY_LIMIT=-1 composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition:2.3.7-p4 magento

"repo.magento.com"
username: f6185b22e7cd50df3e0ecb9867016f1b
password: 72341b943e7690b9938abe3f7700a51f

cd magento && mv * ../

php bin/magento setup:install \
--base-url=http://magento.loc/ \
--db-host=mysql \
--db-name=magento \
--db-user=magento \
--db-password='magento' \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@example.com \
--admin-user=admin \
--admin-password='admin123' \
--language=ru_RU \
--currency=RUB \
--timezone=Europe/Moscow \
--use-rewrites=1

cp /var/www/.composer/auth.json ./var/composer_home/auth.json

php bin/magento sampledata:deploy

php bin/magento module:enable --all

php bin/magento setup:upgrade

php bin/magento deploy:mode:set developer
```