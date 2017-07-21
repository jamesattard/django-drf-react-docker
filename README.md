# Django, DRF, React Development with Docker

Boilerplate for developing and deploying Django/React on Docker

## Instructions

- Create a new App
```sh
docker exec -it djangodrfreactdocker_web_1 python manage.py startapp dummy2
```

- Create a new Database
```sh
docker exec djangodrfreactdocker_db_1 createdb -Upostgres webapp
```

## Features
- Django
- Django Rest Framework
- Docker

## Todo
- React
- Volumes support for auto code reload
