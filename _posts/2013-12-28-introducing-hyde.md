---
layout: post
title: Django Notes
comments: true
category: Django
tags: [python, django,web]
---
## Create Project
```
django-admin startproject projectname
```
## Start Server Locally
in project
```
python manage.py runserver
```
## Create Application
inproject
```
python manege.py startapp appname
```
## Adding App into Settings
/projectname/settings.py
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'Appname'
]
```
## Views and URLs Mapping
/appname/views.py
```python
from django.shortcuts import render
from django.http import HttpResponse   //add this

def index(request):
    return HttpResponse("Hello World!")
```
/projectname/urls.py
```python
from django.conf.urls import url
from django.contrib import admin
from appname import views //add this

urlpatterns = [
    url(r'^$',views.index,name='index'),   //add this
    url(r'^admin/', admin.site.urls),
]
```
## Using App's own urls.py
/projectname/urls.py
```python
from django.conf.urls import url
from django.contrib import admin
from django.conf.urls import include   //add this
from appname import views

urlpatterns = [
    url(r'^$',views.index,name='index'),
    url(r'^appname/',include('appname.urls')),   //add this
    url(r'^admin/', admin.site.urls),
]
```
/appname/urls.py
```python
from django.conf.urls import url
from appname import views

urlpatterns = [
    url(r'^$',views.index,name='index'),
]
```
## Adding Templates
/projectname/settings.py
```python
import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
TEMPLATE_DIR = os.path.join(BASE_DIR,"templates")  //add this

.........

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [TEMPLATE_DIR,],   //add this
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```
in project mkdir templates
add html file here
/templates/appname/index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>First App</title>
</head>
<body>
    <h1>This is index.html</h1>
    \{\{ insert_me \}\} //兩個括號
</body>
</html>
```
/appname/views.py
```python
def index(request):
    my_dict = {'insert_me': 'Hello I am from views.py!'}
    return render(request, 'appname/index.html', context=my_dict)
```
