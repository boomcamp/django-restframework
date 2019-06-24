# Step4 Json Web Token integration

Continuation of the previous topic..

1. Install [Simple JWT](http://getblimp.github.io/django-rest-framework-jwt/) by
```
sudo pipenv install djangorestframework-jwt
```

2. Run environment
```
pipenv shell
```

3. Update `REST_FRAMEWORK` under `tutorial/api/settings.py`
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

4. Under `tutorial/api/urls.py` import

```
...
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView
```
and update **urlpatterns** with these
```
urlpatterns = [
    ...
    path('api/token/', TokenObtainPairView.as_view()),
    path('api/token/refresh/', TokenRefreshView.as_view())
]
```
5. Testing with `postman` or any rest GUI client

Providing a token
```
endpoint: http://127.0.0.1:8000/api/token/
method: POST
body:
    form-urlencoded: 
        username: admin, 
        password: black0Z12345
```

it will generate `refresh` and `token` json encoded values where you can verify your generated token here: https://jwt.io/ and check `secret base64 encoded`

Requesting for new token
```
url: http://127.0.0.1:8000/api/token/refresh/
method: POST
body:
    form-urlencoded:
        refresh: REFRESH_TOKEN
```

example of new access token:
```
{
"access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNTYxMzU4MTM5LCJqdGkiOiJmMTc4ZjA3Y2UzNjc0ZTllYTUyOTYyYmU1NjJhM2NiOCIsInVzZXJfaWQiOjF9.YQvnZadr4HI2AexyVnlNqlbNQxwwjALNdByYT6pPoo8"
}
```

explanation: 
**refresh** = You can use this to request new token incase your current `token` expires
**token** = Access token that can use for any api endpoints

After generating token you can access these api endpoints using your provided `token`
```
http://127.0.0.1:8000/paradigms/
http://127.0.0.1:8000/languages/
http://127.0.0.1:8000/programmers/
```

Diplaying sample reponse using access `token`:
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


[Documentation for django-rest-framework-jwt](http://getblimp.github.io/django-rest-framework-jwt/)
### Requirements
* Python (2.7, 3.3, 3.4, 3.5)
* Django (1.8, 1.9, 1.10)
* Django REST Framework (3.0, 3.1, 3.2, 3.3, 3.4, 3.5)


[Documentation for django-rest-framework-simplejwt](https://github.com/davesque/django-rest-framework-simplejwt)

### Requirements
* Python (3.5, 3.6, 3.7)
* Django (1.11, 2.0, 2.1, 2.2)
* Django REST Framework (3.5, 3.6, 3.7, 3.8, 3.9)
