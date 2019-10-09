# Introduction

Django REST framework is a powerful and flexible toolkit for building Web APIs.

Some reasons you might want to use REST framework:

- The Web browsable API is a huge usability win for your developers.
- Authentication policies including packages for OAuth1a and OAuth2.
- Serialization that supports both ORM and non-ORM data sources.
- Customizable all the way down - just use regular function-based views if you don't need the more powerful features.
- Extensive documentation, and great community support.
- Used and trusted by internationally recognised companies including Mozilla, Red Hat, Heroku, and Eventbrite.

### Prerequisite

1. Python and Django knowledge.
2. Have a configured environment.
  - https://github.com/boomcamp/pyenv-installation
  - https://github.com/boomcamp/setup-pip-pipenv-django-admin-python3
3. Jason web token knowledge.
4. ORM(Object Relational Mapper) knowledge.

Tutorial branches:


1. **step1-basics** = Creating tutorial folder and setting up virtual environment with pipenv, django and djangorestframework installation, creating api app.

2. **step2-simple-orm** = Basic ORM template Language, Paradigm, Programmer model serializers, url, views creating basic table

3. **step3-permissions** = Adding permissions.IsAuthenticatedOrReadOnly languages/views.py.

4. **step4-jwt** = Generating and Securing api, adding rest_framework_simplejwt.authentication.JWTAuthentication to REST_FRAMEWORK, importing TokenObtainPairView, TokenRefreshView to tutorial/api/urls.py.

5. **step5-tutorial** = Done.

6 **step6-rest-swagger** = Swagger api documentation.


If havent configured environment : https://github.com/boomcamp/setup-pip-pipenv-django-admin-python3

Official documentation:

[Django](https://www.djangoproject.com/)

[Django Restframework](https://www.django-rest-framework.org/)

[Django Restswagger](https://django-rest-swagger.readthedocs.io/en/latest/)
