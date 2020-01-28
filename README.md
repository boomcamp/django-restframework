# Creating the base project, simple migration and serving our first app.

Before we get started, make sure that [pipenv]( https://github.com/boomcamp/setup-pip-pipenv-django-admin-python3) already installed in our development machines.

1. Create folder `tutorial`.

```
mkdir tutorial && cd tutorial
```

2. Install `django` and `djangorestframework` packages.

```
pipenv install django djangorestframework
```

3. Activate the virtual environment.

```
pipenv shell
```

4. Using `django-admin` create a project folder called `api`. this will be our base directory project.

```
django-admin startproject api .
```

5. Create app called `languages`. 

```
django-admin startapp languages
```

Output :

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ ls
api  languages  manage.py  Pipfile  Pipfile.lock
```

6. Create these files(`serializers.py, urls.py`) under `tutorial/languages/`.

```
touch serializers.py urls.py 
```

Output:

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial/languages$ ls
admin.py  apps.py  __init__.py  migrations  models.py  serializers.py  tests.py  urls.py  views.py

```

7. Add `rest_framework,languages` inside `INSTALLED_APPS` under tutorial/api/`settings.py`.

```
INSTALLED_APPS = [
     ...
    'rest_framework',
    'languages'
    ...
]
```


<h1 id="simple-orm"> We also need to update our source files</h1>.

8. tutorial/languages/`models.py`.

**models.py** = Holds fields of language app.

```
from django.db import models

class Language(models.Model):
    name = models.CharField(max_length=50)
    paradigm = models.CharField(max_length=50)

    def __str__(self):
        return self.name
```

9. tutorial/languages/`serializers.py`.

**serializers.py** = Holds model and queryset

```
from rest_framework import serializers
from .models import Language

class LanguageSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Language
        fields = ('id', 'url', 'name', 'paradigm')
```

10. tutorial/languages/`urls.py`.

**languages/urls.py** =  Holds routing of our language app.

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

**languages/views.py** = Holds views of our language app.

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

**api/urls.py** = This is our main route for `api` project.

We also need to import ***include*** and add the `path('', include('languages.urls'))` inside ***urlpatterns***.

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('languages.urls'))
]
```

13. Create our first migration, by default django uses`SQLite` which we're going to use to migrate into database.

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ python manage.py makemigrations
Migrations for 'languages':
  languages/migrations/0001_initial.py
    - Create model Language
```

14. Create the tables.

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, languages, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying languages.0001_initial... OK
  Applying sessions.0001_initial... OK
```

15. Last is to serve it at `http://127.0.0.1:8000/`

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ python manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
October 08, 2019 - 08:54:52
Django version 2.2.6, using settings 'api.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Result : (Root page)

![Django Root page](root.png)

# Next

[Django Object Relational Mapper(ORM)](https://github.com/boomcamp/django-restframework/tree/step2-simple-orm)

