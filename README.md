# Session-5

## Configuration

### Python
1. Pull some python lightweight image:
```
docker run -it --name django \
-v "$PWD":/home/django \
-w /home/django \
-p8080:8000 \
python:3.10-bullseye
```
2. Enter to the container:
```
docker exec -it django /bin/bash
```
3. Install django and dependencies:
```
pip install django djangorestframework
```

## Django development
1. Create a new django project:
```
django-admin startproject myproject
```
2. Exit your container and open your project with visual code:
```
exit


code myproject
```
3. Go to you myproject directory inside your django project directory, open your settings.py and go to installed apps section in order to add rest framework library:

```
...
INSTALLED_APPS = [
...
'lastlibraryofyourapp',

'rest_framework',

```

4. Inside your django project directory create a new folder called api, your directory tree must be like this:

```
---myproject
|
|
-------myproject
|
|
-------api
```
5. Inside your api directory create an __init__.py file and views.py file:

```
touch __init__.py
code views.py
```

6. Django is based in the MVC architecture, then in this file we have to create the view of the API that return the data of our application:

```
# api/views.py
from rest_framework.response import Response
from rest_framework.decorators import api_view

@api_view(['GET'])
def getData(request):
  person = {'name': 'Dennis', 'age': 28}
  return Response(person)
```
7. In the previous file we created some function with the api view decorator in order to allow different HTTP verbs in the request. Now create a urls.py file in order to indicate the routes of our API:

```
# api/urls.py
from django.urls import path
from . import views

urlpatterns = [
  path('', views.getData)
]
```
8. In the last code we define the views functions in order to expose data with the url pattern '', which means that the data of the response will be available on the route localhost:8000. Then we must update our urls.py of the general app in order to add the configuration of the urls.py file of the API app:

```
# myproject/urls.py
# somelibraries...

urlpatterns = [
  # somedefaulturls,
  path('', include('api.urls'))
]
```
9. Then run your server and go to your browser to check it:
```
python manage.py runserver
```
