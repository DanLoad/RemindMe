# DomoPhone

## Установка статического IP

Заходим в файл конфигурации:

| $ | sudo nano /etc/dhcpcd.conf |
|---|-------------:|

В конце файла дописываем следующую строчку, чтобы игнорировать DHCP сервера, и назначить нужные нам настройки:
```
nodhcp
```
После этой строки назначим статический адрес для проводного подключения:
```
interface eth0
static ip_address=192.168.1.101/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1

interface wlan0
static ip_address=192.168.1.101/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```

## Установка Django Apach2 wsgi:
Обновляем систему:

| $ | sudo apt-get update |
|---|-------------:|

Устанавливает pip, Apach2, mod wsgi:

| $ | sudo apt-get install python3-pip apache2 libapache2-mod-wsgi-py3 |
|---|-------------:|

Устанавливаем вертуальную среду:

| $ | sudo pip3 install virtualenv |
|---|-------------:|

Создаем папку с названием проекта:

| $ | mkdir ~/___DomoPhone___ |
|---|-------------:|

Заходим в нашу папку:

| $ | cd ~/___DomoPhone___ |
|---|-------------:|

В каталоге проекта создайте виртуальную среду Python, набрав:

| $ | virtualenv ___project___ |
|---|-------------:|

Необходимо активировать виртуальную среду:

| $ | source ~/___DomoPhone___/___project___/bin/activate |
|---|-------------:|

Устанавлеваем Django в вертуальную среду:

| (project)$ | pip install django |
|---|-------------:|

Создаем проект Django:

| (project)$ | django-admin.py startproject ___DomoPhone___ ~/___DomoPhone___ |
|---|-------------:|

Вы должны получить структуру каталогов, которая выглядит следующим образом:

```javascript
.
└── ./DomoPhone/          # parent project directory
    ├── manage.py         # Django management script
    ├── DomoPhone/            # project code directory
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    └── project/     # project virtual environment directory
        └── . . .
```

Откройте файл настроек в вашем текстовом редакторе:

| (project)$ | nano ~/___DomoPhone___/___DomoPhone___/settings.py |
|---|-------------:|

В квадратных скобках введите ip адреса:

```python
ALLOWED_HOSTS = ["server_domain_or_IP", "127.0.0.1", "127.0.1.1"]
```

И укажите статический каталог:

```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
```

Сохраните и закройте файл.

Теперь мы можем перенести исходную схему базы данных в нашу базу данных SQLite, используя скрипт управления:

| (project)$ | cd ~/___DomoPhone___ |
|---|:-------------|
| (project)$ | ./manage.py makemigrations |
| (project)$ | ./manage.py migrate  |

Создайте административного пользователя для проекта, набрав:

| (project)$ | ./manage.py createsuperuser |
|---|-------------:|

Что бы собрать все статические данные:

| (project)$ | ./manage.py collectstatic |
|---|-------------:|

Что бы разрешить трафик на порт 8000, наберите:

| (project)$ | sudo iptables -I INPUT -p tcp --dport 8000 -j ACCEPT |
|---|-------------:|

Протестируйте свой проект, запустив сервер разработки Django с помощью этой команды:

| (project)$ | ./manage.py runserver 0.0.0.0:8000 |
|---|-------------:|

В веб-браузере перейдите на доменное имя или IP-адрес вашего сервера, а затем :8000:

|  http://server_domain_or_IP:8000 |
|----------------|

Когда вы закончите исследование, нажмите CTRL-C в окне терминала, чтобы выключить сервер разработки.

Теперь мы закончили с Django, поэтому мы можем выйти из нашей виртуальной среды, набрав:

| (project)$ | deactivate |
|---|-------------:|

Приставка к вашей строке должна исчезнуть.(myprojectenv)

Чтобы настроить проход WSGI, нам нужно отредактировать файл виртуального хоста по умолчанию:

| $ | sudo nano /etc/apache2/sites-available/000-default.conf |
|---|-------------:|

И добавить в файл:

```
<VirtualHost *:80>
    Alias /static /home/pi/DomoPhone/static
    <Directory /home/pi/DomoPhone/static>
        Require all granted
    </Directory>
    <Directory /home/pi/DomoPhone/DomoPhone>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>
    WSGIDaemonProcess DomoPhone python-home=/home/pi/DomoPhone/roject python-path=/home/pi/DomoPhone
    WSGIProcessGroup DomoPhone
    WSGIScriptAlias / /home/pi/DomoPhone/DomoPhone/wsgi.py
</VirtualHost>
```
Выдача разрешений:

| $ | chmod 664 ~/___DomoPhone___/db.sqlite3 |
|---|:-------------|
| $ | chmod 775 ~/___DomoPhone___ |
| $ | sudo chown :www-data ~/___DomoPhone___/db.sqlite3 |
| $ | sudo chown :www-data ~/___DomoPhone___ |

Hастроить наш брандмауэр:

| $ | sudo iptables -D INPUT -p tcp --dport 8000 -j ACCEPT |
|---|:-------------|
| $ | sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT |

Проверьте файлы Apache, чтобы убедиться, что вы не допустили синтаксических ошибок:

| $ | sudo apache2ctl configtest |
|---|:-------------|

Последняя строчка должна выглядеть так:
```
Output
. . .
Syntax OK
```

Перезапустите Apache, набрав:

| $ | sudo systemctl restart apache2 |
|---|:-------------|

## Установка Samba:

| $ | sudo apt-get install samba samba-common-bin |
|---|:-------------|

Теперь Samba нужно настроить. Для этого открываем файл конфигурации:

| $ | sudo nano /etc/samba/smb.conf |
|---|:-------------|

И добавляем в конец файла следующие настройки:

```
[DomoPhone]
Comment = DomoPhone folder
Path = /
Browseable = yes
Writeable = yes
only guest = no
create mask = 0777
directory mask = 0777
Public = yes
Guest ok = yes
```

Что бы дать права на папки нужно:

| $ | sudo chmod -R 777 /home |
|---|:-------------|

