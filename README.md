# Introduction to Django-restframework

Django REST framework is a powerful and flexible toolkit for building Web APIs.

Some reasons you might want to use REST framework:

- The Web browsable API is a huge usability win for your developers.
- Authentication policies including packages for OAuth1a and OAuth2.
- Serialization that supports both ORM and non-ORM data sources.
- Customizable all the way down - just use regular function-based views if you don't need the more powerful features.
- Extensive documentation, and great community support.
- Used and trusted by internationally recognised companies including Mozilla, Red Hat, Heroku, and Eventbrite.

Tutorial branches:

```
step1-basics = Creating tutorial folder and setting up virtual environment with pipenv, django and djangorestframework installation, creating api app

step2-simple-orm = Basic ORM template Language, Paradigm, Programmer model serializers, url, views creating basic table

step3-permissions = Adding permissions.IsAuthenticatedOrReadOnly languages/views.py

step4-jwt = Generating and Securing api, adding rest_framework_simplejwt.authentication.JWTAuthentication to REST_FRAMEWORK, importing TokenObtainPairView, TokenRefreshView to tutorial/api/urls.py

step5-tutorial = Done
```

If havent configured environment : https://github.com/boomcamp/setup-pip-pipenv-django-admin-python3

Official documentation:

[Django](https://www.djangoproject.com/)

[Django Restframework](https://www.django-rest-framework.org/)
