### Integrating rest swagger for api documentation

What Is Swagger?

Swagger allows you to describe the structure of your APIs so that machines can read them. The ability of APIs to describe their own structure is the root of all awesomeness in Swagger. Why is it so great? Well, by reading your API’s structure, we can automatically build beautiful and interactive API documentation. We can also automatically generate client libraries for your API in many languages and explore other possibilities like automated testing. Swagger does this by asking your API to return a YAML or JSON that contains a detailed description of your entire API. This file is essentially a resource listing of your API which adheres to OpenAPI Specification. The specification asks you to include information like:
What are all the operations that your API supports?
What are your API’s parameters and what does it return?
Does your API need some authorization?
And even fun things like terms, contact information and license to use the API.
You can write a Swagger spec for your API manually, or have it generated automatically from annotations in your source code. Check swagger.io/open-source-integrations for a list of tools that let you generate Swagger from code.

[swagger.io](https://swagger.io/docs/specification/2-0/what-is-swagger/).


### Rest swagger integration.

We are going to integrate [django-rest-swagger](https://django-rest-swagger.readthedocs.io/en/latest/) as default api documentation.

1. Install rest swagger.

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/django-restframework$ pipenv install django-rest-swagger
Installing django-rest-swagger…
Adding django-rest-swagger to Pipfile's [packages]…
✔ Installation Succeeded 
Pipfile.lock not found, creating…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
✔ Success! 
Updated Pipfile.lock (b07e99)!
Installing dependencies from Pipfile.lock (b07e99)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 15/15 — 00:00:06

```

2. Update `REST_FRAMEWORK` under `tutorial/api/settings.py` and add `'DEFAULT_SCHEMA_CLASS': 'rest_framework.schemas.coreapi.AutoSchema' ,`

```
REST_FRAMEWORK = {
    'DEFAULT_SCHEMA_CLASS': 'rest_framework.schemas.coreapi.AutoSchema' ,
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
}
```

3. `tutorial/languages/urls.py`

from django.urls import path, include
from . import views 
from rest_framework import routers 

```
from rest_framework_swagger.views import get_swagger_view
from rest_framework.documentation import include_docs_urls
from django.conf.urls import url
```

`schema_view = get_swagger_view(title="Swagger Docs")`

router = routers.DefaultRouter()
router.register('languages', views.LanguageView)
router.register('paradigms', views.ParadigmView)
router.register('programmers', views.ProgrammerView)

urlpatterns = [
    `url(r'^docs/', schema_view),`
    path('', include(router.urls))
]

4. First we need to secure our endpoints so i add `permission_classes = (permissions.IsAuthenticatedOrReadOnly,)` at bottom of every class object

We can now start documenting our endpoints: `tutorial/languages/views.py`.

```
from django.shortcuts import render
from rest_framework import viewsets, permissions
from .models import Language, Paradigm, Programmer
from .serializers import LanguageSerializer, ParadigmSerializer, ProgrammerSerializer

class LanguageView(viewsets.ModelViewSet):
    """
    retrieve:
        Return the given language.

    list:
        Return a list of all languages.

    create:
        Create a new language.

    destroy:
        Delete a language.

    update:
        Update a language.

    partial_update:
        Update a language.
    """
    queryset = Language.objects.all()
    serializer_class = LanguageSerializer
    permission_classes = (permissions.IsAuthenticatedOrReadOnly,)

class ParadigmView(viewsets.ModelViewSet):
    """
    retrieve:
        Return the given paradigm.

    list:
        Return a list of all paradigm.

    create:
        Create a new pardigm.

    destroy:
        Delete a paradigm.

    update:
        Update a paradigm.

    partial_update:
        Update a paradigm.
    """
    queryset = Paradigm.objects.all()
    serializer_class = ParadigmSerializer
    permission_classes = (permissions.IsAuthenticatedOrReadOnly,)

class ProgrammerView(viewsets.ModelViewSet):
    """
    retrieve:
        Return the given programmer.

    list:
        Return a list of all programmer.

    create:
        Create a new programmer.

    destroy:
        Delete a programmer.

    update:
        Update a programmer.

    partial_update:
        Update a programmer.
    """
    queryset = Programmer.objects.all()
    serializer_class = ProgrammerSerializer
    permission_classes = (permissions.IsAuthenticatedOrReadOnly,)
```

### Documentations

Now navigate to `http://127.0.0.1:3000/docs/`

Screenshot 1

![alt text](root-api.png)


Screenshot 2

![alt text](collapsed-api.png)
