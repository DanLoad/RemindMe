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
└── ./myproject/          # parent project directory
    ├── manage.py         # Django management script
    ├── myproject/            # project code directory
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    └── myprojectenv/     # project virtual environment directory
        └── . . .
```
