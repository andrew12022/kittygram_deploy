# Kittygram

Kittygram — социальная сеть для обмена фотографиями любимых питомцев. Это полностью рабочий проект, который состоит из бэкенд-приложения на Django и фронтенд-приложения на React.

## Функции
- Kittygram даёт возможность пользователям поделиться и похвастаться фотографиями своих любимымих котиков.
- Зарегистрированные пользователи могут создавать, просматривать, редактировать и удалять свои записи.

## Подключение сервера к GitHub
На сервере должен быть установлен Git. Ввести в терминал команду
```
sudo apt update
```
Затем Ввести команду для проверки
```
git --version
```

Если Git не установлен - нужно установить 
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

## Установка
### Бэкенд(Django)

Клонировать репозиторий и перейти в него в командной строке:

```
git clone git@github.com:ваш_логин/infra_sprint1.git
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
DEBUG=ваш_режим_работы
ALLOWED_HOSTS=ваши_адреса
```

Запустить веб-сервер:

```
python manage.py runserver 0.0.0.0:8000
```

Бэкенд будет доступен по адресу: http://IP_вашего_сервера:8000/

### Фронтенд (React)

Установить Node.js на сервер
```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt install -y nodejs
```

Перейти в директорию фронтенда:

```
cd kittygram/frontend
```

Теперь установить зависимости:

```
npm i
```

Запустить приложение:

```
npm run start
```

Фронтенд будет доступен по адресу: http://IP_вашего_сервера:3000/

## Технологии и необходимые инструмены
- Python 3.x
- Node.js 9.x.x
- Git
- Nginx
- Gunicorn 20.x.x
- Django 3.x.x (backend)
- React (frontend)

## Ссылка на приложение
- https://kittygramdeploy.myftp.org

## Автор
- [andrew12022](https://github.com/andrew12022)
