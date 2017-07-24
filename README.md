# Django, DRF, React Development with Docker

Boilerplate for developing and deploying Django/React on Docker

## Development Instructions

1. Run the stack

   ```sh
   docker-compose up
   ```
   

2. Create a new Database

   ```sh
   docker exec djangodrfreactdocker_db_1 createdb -Upostgres webapp
   ```
   

3. Create a new App

   ```sh
   docker exec -it djangodrfreactdocker_web_1 python manage.py startapp dummyApp
   ```
   

4. Access the development project through http://localhost:8000/


## Deployment Instructions

1. Create deploy/local_settings.py (Don't source control it, keep it a secret!). Example:

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
   

2. Rebuild image and run the stack

   ```sh
   docker build -f Dockerfile.prod -t webapp:latest .
   cd deploy/ && docker-compose up
   ```
   

3. Create database and run migrations

   ```sh
   docker exec deploy_db_1 createdb -Upostgres webapp
   docker exec deploy_web_1 python3 manage.py migrate
   ```
   

4. Access the production project through http://localhost:8700/

   You will get an Nginx error 404 because Debug is set to False. You can confirm that this is working either by creating an app or by enabling Debug.
   

## FAQ
1. How is nginx connecting to the uwsgi server?

   Through Unix socket. This is located at /webapp/app.sock. Current permissions are very open (666).
   

2. How do I connect to the container for troubleshooting?

   You can 'exec' inside a running container as follows:
   ```sh
   docker exec -it deploy_web_1 bash
   ```
   

3. Do I need to rebuild the Docker image everytime I update the code?

   Since the application source code is mounted as a data volume, you do not need to rebuild the whole image. You only need to restart the docker container as follows:
   ```sh
   docker restart deploy_web_1
   ```
   

4. Is this production ready?

   Not really, but almost. Check out the security, and data volumes for the database and you'll be all set!
   

## Features
- Django
- Django Rest Framework
- Docker

## Todo
- Support for React & Webpack static Assets
