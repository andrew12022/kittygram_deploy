# Kittygram

**Kittygram** — социальная сеть для обмена фотографиями любимых питомцев. Это полностью рабочий проект, который состоит из бэкенд-приложения на Django и фронтенд-приложения на React.

## Функции
- Kittygram даёт возможность пользователям поделиться и похвастаться фотографиями своих любимых котиков.
- Зарегистрированные пользователи могут создавать, просматривать, редактировать и удалять свои записи.

## Установка

### Подключение сервера к GitHub

Ввести в терминал команду
```
sudo apt update
```

Затем ввести команду для проверки
```
git --version
```

Если Git не установлен, нужно его установить 
```
sudo apt install git
```

Находясь на сервере сгенерировать пару SSH-ключей командой 
```
ssh-keygen
```

Сохранить открытый ключ в вашем аккаунте на GitHub. Для этого вывести ключ в терминал командой 
```
cat .ssh/id_rsa.pub
```

Скопировать ключ от символов ssh-rsa, включительно, и до конца. Добавить это ключ к вашему аккаунту на GitHub.

### Бэкенд (Django)

Клонировать репозиторий и перейти в него в командной строке:

```
git@github.com:andrew12022/infra_sprint1.git
```

```
cd infra_sprint1/backend
```

Cоздать и активировать виртуальное окружение:

```
python -m venv venv
```

```
source venv/bin/activate
```

Установить зависимости из файла requirements.txt:

```
python -m pip install --upgrade pip
```

```
pip install -r requirements.txt
```

Выполнить миграции:

```
python manage.py migrate
```

Создать суперпользователя:

```
python manage.py createsuperuser
```

Создать файл `.env` в той же директории, что и `settings.py`, и добавить свои переменные:

```.env
SECRET_KEY=ваш_ключ
DEBUG=ваш_режим_работы(False или True)
ALLOWED_HOSTS=внешний IP сервера,127.0.0.1,localhost,домен
```

Подготовить бэкенд-приложение для сбора статики. В файле settings.py указать директорию, куда эту статику нужно сложить
```
STATIC_URL = 'static_backend'
STATIC_ROOT = BASE_DIR / 'static_backend'
```

Собрать статику бэкенд-приложения
```
python manage.py collectstatic
```

Скопировать директорию static_backend/ в директорию /var/www/название_проекта/
```
sudo cp -r путь_к_директории_с_бэкендом/static_backend /var/www/название_проекта
```

### Фронтенд (React)

Установить Node.js на сервер
```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt install -y nodejs
```

Перейти в директорию фронтенда:

```
cd infra_sprint1/frontend
```

Теперь установить зависимости:

```
npm i
```

Из директории с фронтенд-приложением выполнить команду
```
npm run build
```

Скопировать статику фронтенд-приложения в директорию по умолчанию
```
sudo cp -r путь_к_директории_с_фронтенд-приложением/build/. /var/www/имя_проекта/
```

### Установка и настройка Gunicorn

В файле зависимостей он уже есть, если его нет, то нужно прописать команду
```
pip install gunicorn==20.1.0
```

Перейти в директорию с файлом manage.py, и запустить Gunicorn
```
gunicorn --bind 0.0.0.0:8000 backend.wsgi
```

Создать файл конфигурации юнита systemd для Gunicorn в директории
/etc/systemd/system/. Назвать его по шаблону gunicorn_название_проекта.service
```
sudo nano /etc/systemd/system/gunicorn_название_проекта.service
```

Подставьте в код из листинга свои данные
```
[Unit]
Description=gunicorn daemon
After=network.target
[Service]
User=имя_пользователя_в_системе
WorkingDirectory=/home/имя_пользователя/папка_с_проектом/папка_с_файлом_manage.py/
ExecStart=/.../venv/bin/gunicorn --bind 0.0.0.0:8000 backend.wsgi:application
[Install]
WantedBy=multi-user.target
```

Запустить Gunicorn в командной строке
```
sudo systemctl start gunicorn_название_проекта
```

Чтобы systemd следил за работой демона Gunicorn, запускал его при старте системы
и при необходимости перезапускал, использовать команду
```
sudo systemctl enable gunicorn_название_проекта
```

### Установка и настройка Nginx

Если Nginx ещё не установлен на удалённый сервер, установить его
```
sudo apt install nginx -y
```

Запустить Nginx командой
```
sudo systemctl start nginx
```

Обновить настройки Nginx. Для этого открыть файл конфигурации веб-сервера
```
sudo nano /etc/nginx/sites-enabled/default
```

Очистить содержимое файла и записать новые настройки
```
server {
 listen 80;
 server_name ваш_домен;
 location /api/ {
 proxy_pass http://127.0.0.1:8000;
 }

 location /admin/ {
 proxy_pass http://127.0.0.1:8000;
 }
 location / {
 root /var/www/имя_проекта;
 index index.html index.htm;
 try_files $uri /index.html;
 }
}
```

Сохранить изменения в файле, закрыть его и проверить на корректность
```
sudo nginx -t
```

Перезагрузить конфигурацию Nginx
```
sudo systemctl reload nginx
```

### Настройка файрвола ufw
Активировать разрешение принимать запросы только на порты 80, 443 и 22
```
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
```

Включить файрвол
```
sudo ufw enable
```

## Технологии и необходимые инструменты
- Python
- Django
- Django REST framework
- python-dotenv
- Nginx
- Gunicorn
- Node.js
- React

## Автор
- [Андрей Елистратов](https://github.com/andrew12022)
