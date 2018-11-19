Small web project to manage Assetto Corsa Competizione servers, build on Django.
Allows to manage multiple configs and multiple server instances. Works in Linux and in Windows, dunno about OSX.


## Development
Currently, the web-app consists of two sub-apps
* cfgs: Basically creates an autogenerated view from ACCs custom.json, navigating through the object. Allows to create and edit multiple configurations, they are stored in the folder 'settings.CONFIGS'.
* instances: Start a new ACC server instance or stop/delete running instances. Each instance uses a copy of the ACC 'server' directory, which is placed in the folder 'settings.INSTANCES'.

Quick start:
```bash
git clone https://github.com/gotzl/accservermanager.git
cd accservermanager/
# Configure the things at the bottom of accservermanager/settings.py, ie the path to your ACC server files
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
``` 

## Dependencies
```bash
pip install django django-material random-word
```
Windows users might want to follow the official Django install instructions.


## Deployment
Follow the development instructions to deploy the app, should be good enough for our purposes...

Alternatively, I've created a Dockerfile which uses wine to run the ACC server.

```bash
# Create the image
docker build -t accservermanager .
# fire up a container
docker run -d --name accservermanager -v PATH_TO_ACC/server:/server:r -p 8000:8000 -p 9231:9231/udp -p 9232:9232/tcp accservermanager
# initiate the app and create a manager user
docker exec -i -t accservermanager /bin/bash
python3 manage.py migrate
python3 manage.py createsuperuser
```

When logged in you can add more users with djangos admin pages (...:8000/admin).
