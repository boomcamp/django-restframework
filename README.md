# Step3 permission

Continuation of the previous topic..

### Activate virtual environment
```
pipenv shell
```

1. Make sure you properly installed 'rest_framework' in your django app or you might get error edit your `tutorial/api/urls.py` with
```
urlpatterns = [
    ...
    path('api-auth', include('rest_framework.urls'))
]
```

2. Create first superuser account and type your password e.g: ***black0Z12345***
```
python3 manage.py createsuperuser --email admin@example.com --username admin
```

3. Start django server
```
python3 manage.py runserver
```

4. Click `login` and enter your created username: ***admin*** and password: ***black0Z12345***

### Permissions
> There are two common permissions that you can use in django rest framworks these are by `Individual views` or by `Global settings`

Edit `tutorial/languages/views.py` and import **permissions** and below `LanguageView` class add **permission_classes = (permissions.IsAuthenticatedOrReadOnly,)**

```
# tutorial/languages/views.py
from django.shortcuts import render
from rest_framework import viewsets, permissions
from .models import Language, Paradigm, Programmer
from .serializers import LanguageSerializer, ParadigmSerializer, ProgrammerSerializer

class LanguageView(viewsets.ModelViewSet):
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer
    permission_classes = (permissions.IsAuthenticatedOrReadOnly,)

class ParadigmView(viewsets.ModelViewSet):
    queryset = Paradigm.objects.all()
    serializer_class = ParadigmSerializer

class ProgrammerView(viewsets.ModelViewSet):
    queryset = Programmer.objects.all()
    serializer_class = ProgrammerSerializer
```
**Syntax** :
`
permission_classes = (permissions.IsAuthenticatedOrReadOnly,)
`

***IsAuthenticatedOrReadOnly*** = Can view rest api even NOT AUTHENTICATED 
            Can view form if AUTHENTICATED

***IsAuthenticated*** = Can view both rest api and form if AUTHENTICATED

### Global permissions under settings 
At bottom of `tutorial/api/settings.py` add
```
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    )
}
```

Complete permissions documentation: https://www.django-rest-framework.org/api-guide/permissions/
