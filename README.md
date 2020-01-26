# DomoPhone

## Установка:

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

<VirtualHost *:80>

    Alias /static /home/sammy/___DomoPhone___/static
    <Directory /home/sammy/___DomoPhone___/static>
        Require all granted
    </Directory>

    <Directory /home/sammy/___DomoPhone___/myproject>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>

    WSGIDaemonProcess myproject python-home=/home/sammy/myproject/myprojectenv python-path=/home/sammy/myproject
    WSGIProcessGroup myproject
    WSGIScriptAlias / /home/sammy/myproject/myproject/wsgi.py

</VirtualHost>
