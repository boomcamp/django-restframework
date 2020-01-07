# Step-4 - Json Web Token

We're going to produce token's for our app

1. We need to install `djangorestframework-jwt` library in our active virtual environment.

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ pipenv install djangorestframework_simplejwt
Installing djangorestframework_simplejwt…
Adding djangorestframework_simplejwt to Pipfile's [packages]…
✔ Installation Succeeded 
Pipfile.lock (271d0b) out of date, updating to (0bbb1b)…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
✔ Success! 
Updated Pipfile.lock (271d0b)!
Installing dependencies from Pipfile.lock (271d0b)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 7/7 — 00:00:02
```

2. Update or add this into `REST_FRAMEWORK` under `tutorial/api/settings.py`.

```
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
}
```

4. Under `tutorial/api/urls.py` we need to declare variables `TokenObtainPairView, TokenRefreshView` from `rest_framework_simplejwt.views` views.

```
...
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView
```

and update **urlpatterns** with these code.

```
urlpatterns = [
    ...
    path('api/token/', TokenObtainPairView.as_view()),
    path('api/token/refresh/', TokenRefreshView.as_view())
]
```

We should now have these complete code:

```
from django.contrib import admin
from django.urls import path, include
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('languages.urls')),
    path('api-auth', include('rest_framework.urls')),
    path('api/token/', TokenObtainPairView.as_view()),
    path('api/token/refresh/', TokenRefreshView.as_view())
]

```

5. We can now download or install [insomnia](https://insomnia.rest/download/) directly .

```
(tutorial) dev-mentor@devmentor-PC-MK34LEZCBEAD:~/Downloads/tutorial$ sudo snap install insomnia
```

you can also use any REST gui client you may want.

### Accessing our endpoints

1. Providing a token.

```
endpoint: http://127.0.0.1:8000/api/token/
method: POST
body:
    form-urlencoded: 
        username: admin, 
        password: black0Z12345
```

Token:

![alt text](request-token.png)

It will generate two tokens which are `refresh` and `token` json encoded values.

2. Requesting for a new token.

```
url: http://127.0.0.1:8000/api/token/refresh/
method: POST
body:
    form-urlencoded:
        refresh: REFRESH_TOKEN
```

Referesh token:

![alt text](request-new-if-expired.png)


Descriptions: 

1. **token** = Access token that can use for any api endpoints.

2. **refresh token** = You can use this to request new token incase your current `token` expires

After generating tokens we can access these endpoints.

```
http://127.0.0.1:8000/paradigms/
http://127.0.0.1:8000/languages/
http://127.0.0.1:8000/programmers/
```

Example response:

```
url: http://127.0.0.1:8000/paradigms/
method: GET 
type: 
    Autorization
    bearer: YOUR_TOKEN
```

after sending the request you should see a response like:
```
{
    "id": 1,
    "url": "http://127.0.0.1:8000/paradigms/1/",
    "name": "functional"
},
{
    "id": 2,
    "url": "http://127.0.0.1:8000/paradigms/2/",
    "name": "procedural"
},
{
    "id": 3,
    "url": "http://127.0.0.1:8000/paradigms/3/",
    "name": "object oriented"
}

```
### Next

[step5: Re-installing ](https://github.com/boomcamp/django-restframework/tree/step5-tutorial).

