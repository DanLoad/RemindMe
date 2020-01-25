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

| $ | source ~/___DomoPhone___/___project___/bin/activate  |
|---|-------------:|

Устанавлеваем Django в вертуальную среду:

| (project)$ | pip install django  |
|---|-------------:|

Создаем проект Django:

| (project)$ | django-admin.py startproject ___DomoPhone___ ~/___DomoPhone___  |
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

| (project)$ | nano ~/___DomoPhone___/___DomoPhone___/settings.py  |
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
