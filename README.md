Задание 1
Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.
Требования к результаты
Прикрепите в файл README.md скриншот авторизации в админке.


Решение 1

![ALT TEXT](https://github.com/mezhibo/Home-Work-Zabbix1/blob/344f6c6dbaae651c0a3a6a9562a7ae5087d02631/IMG/1.jpg)

Список комманд для установки 
sudo su
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_all.deb

dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list

sudo apt update
sudo apt install postgresql-13
systemctl status postgresql
sudo -u postgres createuser --pwprompt zabbix

sudo -u postgres createdb -O zabbix zabbix

zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

nano /etc/zabbix/zabbix_server.conf - прописываем пароль от базза данных в DBPassword=

sudo apt remove apache2
nano /etc/zabbix/nginx.conf 
 listen 80;
server_name 158.160.135.179; 

systemctl restart zabbix-server zabbix-agent nginx php7.4-fpm
systemctl enable zabbix-server zabbix-agent nginx php7.4-fpm




Задание 2
Установите Zabbix Agent на два хоста.

Процесс выполнения
Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.
Требования к результаты
Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.


Решение 2

![ALT TEXT](https://github.com/mezhibo/Home-Work-Zabbix1/blob/7b31d95ffec3912731fcd9c5a4d93c74e38cb935/IMG/2.jpg)

![ALT TEXT](https://github.com/mezhibo/Home-Work-Zabbix1/blob/7b31d95ffec3912731fcd9c5a4d93c74e38cb935/IMG/3.jpg)

![ALT TEXT](https://github.com/mezhibo/Home-Work-Zabbix1/blob/7b31d95ffec3912731fcd9c5a4d93c74e38cb935/IMG/4.jpg)

![ALT TEXT](https://github.com/mezhibo/Home-Work-Zabbix1/blob/7b31d95ffec3912731fcd9c5a4d93c74e38cb935/IMG/5.jpg)

![ALT TEXT](https://github.com/mezhibo/Home-Work-Zabbix1/blob/7b31d95ffec3912731fcd9c5a4d93c74e38cb935/IMG/6.jpg)


Устанавливаем sudo apt-get install zabbix-agent
Добавляем в автозапуск sudo systemctl enable zabbix-agent
Прописываем адрес нашего заббикс-сервера в этом конфиге sudo nano /etc/zabbix/zabbix_agentd.conf
И перезапускаем службу агента sudo systemctl restart zabbix-agent



