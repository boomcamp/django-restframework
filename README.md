# Step-4 - Json Web Token

What is JSON Web Token?

JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

Source : https://jwt.io/

1. We need to install `djangorestframework-jwt` library into our active virtual environment.

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

Response:

Screenshot - TODO

It will generate `refresh` and `token` json encoded values where you can verify your generated token here: https://jwt.io/ and check `secret base64 encoded`

2. Requesting for new token.

```
url: http://127.0.0.1:8000/api/token/refresh/
method: POST
body:
    form-urlencoded:
        refresh: REFRESH_TOKEN
```
Response:

Screenshot - TODO

Example of new access token:

```
{
"access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNTYxMzU4MTM5LCJqdGkiOiJmMTc4ZjA3Y2UzNjc0ZTllYTUyOTYyYmU1NjJhM2NiOCIsInVzZXJfaWQiOjF9.YQvnZadr4HI2AexyVnlNqlbNQxwwjALNdByYT6pPoo8"
}
```

explanation: 
**refresh** = You can use this to request new token incase your current `token` expires
**token** = Access token that can use for any api endpoints.

After generating token you can access these api endpoints using your provided `token`.

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

