# Django, DRF, React Development with Docker

Boilerplate for developing and deploying Django/React on Docker

## Development Instructions

- Create a new App
```sh
docker exec -it djangodrfreactdocker_web_1 python manage.py startapp dummy2
```

- Create a new Database
```sh
docker exec djangodrfreactdocker_db_1 createdb -Upostgres webapp
```

## Deployment Instructions

- Create deploy/local_settings.py (Don't source control it, keep it a secret!). Example:
```sh
DEBUG = False
SECRET_KEY = 'h5f-7@dl)r)qpd3^gnu=p)7p$i*zeahqk0@gh90xq9c40vu0(#'
ALLOWED_HOSTS = ['*'] # set this to your real host in production!
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'webapp',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': '',
    }
}
STATIC_ROOT = '/webapp/static'
MEDIA_ROOT = '/webapp/media'
```

## Features
- Django
- Django Rest Framework
- Docker

## Todo
- Support for React & Webpack static Assets
- Support for painless code reloaded (aka Data Volumes)
