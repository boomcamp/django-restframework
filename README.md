# Django Tutorial 1(Django rest framework sample)
> Simple setup and demonstration using `django restframework` and `pipenv` environment

### Create project folder
```
mkdir tutorial
cd tutorial
```

### Install packages usinng **pipenv**
```
pipenv install django
pipenv install djangorestframework
```

### Start api project
```
django-admin startproject api .
```

### Create app 
```
django-admin startapp languages
```

### Create this two files under tutorial/languages/
```
touch serializers.py urls.py 
```

### Activate virtual environment
```
pipenv shell
```

### tutorial/api/settings.py
```
INSTALLED_APPS = [
     ...
    'rest_framework',
    'languages'
    ...
]
```
### tutorial/languages/models.py
```
from django.db import models

class Language(models.Model):
    name = models.CharField(max_length=50)
    paradigm = models.CharField(max_length=50)

    def __str__(self):
        return self.name
```

### tutorial/languages/serializers.py
```
from rest_framework import serializers
from .models import Language

class LanguageSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Language
        fields = ('id', 'url', 'name', 'paradigm')
```

### tutorial/languages/urls.py
```
from django.urls import path, include
from . import views 
from rest_framework import routers 

router = routers.DefaultRouter()
router.register('languages', views.LanguageView)

urlpatterns = [
    path('', include(router.urls))
]
```

### tutorial/languages/views.py
```
from django.shortcuts import render
from rest_framework import viewsets
from .models import Language
from .serializers import LanguageSerializer

class LanguageView(viewsets.ModelViewSet):
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer
```

### tutorial/api/urls.py
> I import ***include*** and added the `path('', include('languages.urls'))` under ***urlpatterns***
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('languages.urls'))
]
```

### Create migrations first 
```
python3 manage.py makemigrations
```

### Create tables
```
python3 manage.py migrate
```

## Lastly serve at http://127.0.0.1:8000/
```
python3 manage.py runserver
```


# References and Tutorials for more advance tutorials about JWT, Table joining etc.
---
[Django Restframework serializes](https://www.django-rest-framework.org/tutorial/quickstart/#serializers)

[pipenv installation](https://hackernoon.com/reaching-python-development-nirvana-bb5692adf30c)

[Python and Django Restframeworks](https://www.youtube.com/channel/UC-QDfvrRIDB6F0bIO4I4HkQ/videos)