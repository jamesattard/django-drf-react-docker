# Django, DRF, React Development with Docker

Boilerplate for developing and deploying Django/React on Docker

## Development Instructions

- Run the stack
```sh
docker-compose up
```

- Create a new Database
```sh
docker exec djangodrfreactdocker_db_1 createdb -Upostgres webapp
```

- Create a new App
```sh
docker exec -it djangodrfreactdocker_web_1 python manage.py startapp dummyApp
```

- Access the development project through http://localhost:8000/

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

- Rebuild image and run the stack
```sh
docker build -f Dockerfile.prod -t webapp:latest .
cd deploy/ && docker-compose up
```

- Create database and run migrations
```sh
docker exec deploy_db_1 createdb -Upostgres webapp
docker exec deploy_web_1 python3 manage.py migrate
```

- Access the production project through http://localhost:8700/
You will get an Nginx error 404 because Debug is set to False. You can confirm that this is working either by creating an app or by enabling Debug.

## FAQ
- How is nginx connecting to the uwsgi server?
Through Unix socket. This is located at /webapp/app.sock. Current permissions are very open (666).

- How do I connect to the container for troubleshooting?
You can 'exec' inside a running container as follows:
```sh
docker exec -it deploy_web_1 bash
```

- Do I need to rebuild the Docker image everytime I update the code?
Sucks to say yes. Watch for this space until I stop procrastinating and enable data volumes or fork it as a feature. Thanks!

- Is this production ready?
Not really, but almost. Check out the security, and data volumes for the database and you'll be all set!

## Features
- Django
- Django Rest Framework
- Docker

## Todo
- Support for React & Webpack static Assets
- Support for painless code reloaded (aka Data Volumes)
