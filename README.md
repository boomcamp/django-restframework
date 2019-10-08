# Django Tutorial 1 - Basic setup and installation using `django`, `django restframework` using `pipenv` environment.

First we need to make sure that we already installed `pipenv` you can find setup procedures for pipenv here https://github.com/boomcamp/setup-pip-pipenv-django-admin-python3

1. Create our Project folder called `tutorial`.

```
mkdir tutorial && cd tutorial
```

2. Install `django` and `djangorestframework` packages using **pipenv**.

```
pipenv install django
pipenv install djangorestframework
```

3. Using `django-admin` create a project folder called `api`.

```
django-admin startproject api .
```

4. Next is creating `languages` app. 

```
django-admin startapp languages
```

5. Create this two files(`serializers.py, urls.py`) under `tutorial/languages/`.

```
touch serializers.py urls.py 
```

6. Activate virtual environment.

```
pipenv shell
```

7. Add these `rest_framework,languages` into `INSTALLED_APPS` under tutorial/api/`settings.py`.

```
INSTALLED_APPS = [
     ...
    'rest_framework',
    'languages'
    ...
]
```

8. tutorial/languages/`models.py`.

```
from django.db import models

class Language(models.Model):
    name = models.CharField(max_length=50)
    paradigm = models.CharField(max_length=50)

    def __str__(self):
        return self.name
```

9. tutorial/languages/`serializers.py`.

```
from rest_framework import serializers
from .models import Language

class LanguageSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Language
        fields = ('id', 'url', 'name', 'paradigm')
```

10. tutorial/languages/`urls.py`.

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

11. tutorial/languages/`views.py`.

```
from django.shortcuts import render
from rest_framework import viewsets
from .models import Language
from .serializers import LanguageSerializer

class LanguageView(viewsets.ModelViewSet):
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer
```

12. tutorial/api/`urls.py`.

I have import ***include*** and added `path('', include('languages.urls'))` under ***urlpatterns***
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('languages.urls'))
]
```

13. Create initial migration. 

```
python3 manage.py makemigrations
```

14. Create the tables.

```
python3 manage.py migrate
```

Served at `http://127.0.0.1:8000/`

```
python3 manage.py runserver
```


### References and Tutorials for more advance tutorials about JWT, Table joining etc.
---
[Django Restframework serializes](https://www.django-rest-framework.org/tutorial/quickstart/#serializers)

[pipenv installation](https://hackernoon.com/reaching-python-development-nirvana-bb5692adf30c)

[Python and Django Restframeworks](https://www.youtube.com/channel/UC-QDfvrRIDB6F0bIO4I4HkQ/videos)
