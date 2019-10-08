# Step2 basic ORM

Continuation of the previous topic

What is **ORM** = Object-relational mapping (ORM) is a programming technique in which a metadata descriptor is used to `connect object code` to a `relational database`.

Below are example of an ORM.

1. tutorial/languages/`models.py`.
```
from django.db import models

class Paradigm(models.Model):
    name = models.CharField(max_length=50)

    def __str__(self):
        return self.name

class Language(models.Model):
    name = models.CharField(max_length=50)
    paradigm = models.ForeignKey(Paradigm, on_delete=models.CASCADE)

    def __str__(self):
        return self.name


class Programmer(models.Model):
    name = models.CharField(max_length=50)
    languages = models.ManyToManyField(Language)

    def __str__(self):
        return self.name
```

2. tutorial/languages/`serializers.py`.
```
from rest_framework import serializers
from .models import Language, Paradigm, Programmer

class LanguageSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Language
        fields = ('id', 'url', 'name', 'paradigm')

class ParadigmSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Paradigm
        fields = ('id', 'url', 'name')

class ProgrammerSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Programmer
        fields = ('id', 'url', 'name', 'languages')
```


3. tutorial/languages/`urls.py`
```
from django.urls import path, include
from . import views 
from rest_framework import routers 

router = routers.DefaultRouter()
router.register('languages', views.LanguageView)
router.register('paradigms', views.ParadigmView)
router.register('programmers', views.ProgrammerView)

urlpatterns = [
    path('', include(router.urls))
]
```

4. tutorial/languages/`views.py`.
```
from django.shortcuts import render
from rest_framework import viewsets
from .models import Language, Paradigm, Programmer
from .serializers import LanguageSerializer, ParadigmSerializer, ProgrammerSerializer

class LanguageView(viewsets.ModelViewSet):
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer

class ParadigmView(viewsets.ModelViewSet):
    queryset = Paradigm.objects.all()
    serializer_class = ParadigmSerializer

class ProgrammerView(viewsets.ModelViewSet):
    queryset = Programmer.objects.all()
    serializer_class = ProgrammerSerializer
```

6. Re-update migrate tables and serve.
```
python3 manage.py makemigrations
python3 manage.py migrate 
python3 manage.py runserver
```

7. You can insert new records in this sequence.
```
paradigms > languages > programmers
```

### Resources ORM
[Django ORM Relationships Cheat Sheet](https://hackernoon.com/django-orm-relationships-cheat-sheet-14433d6cf68c)
