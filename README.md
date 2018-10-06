Django TU Authentication
========================

Requirements
============
- python (2.7, 3.5)
- django (1.11, 2.0)
- social-app-django (2.1.0)

Installation
============
```
pip install django-tuauth
```

Usage
=====
### Prerequisite
Register application in api.tu.ac.th/applications
> note: Callback URL must be same with decarelation in urls.py
> in this example use http://127.0.0.1/oauth/complete/tu/

### in setting.py
```python
INSTALLED_APPS = [
    ...
    'social_django',
    'tuauth',
    ...
]
```
add authentication backend in setting.py
```python
AUTHENTICATION_BACKENDS = (
    ...
    'django.contrib.auth.backends.ModelBackend',
    'tuauth.backend.TUOAuth2',
    ...
)
```
> note: 'django.contrib.auth.backends.ModelBackend' for use built-in authen

set client id and client secret in setting.py
```python
SOCIAL_AUTH_TU_KEY = '<client_id>'
SOCIAL_AUTH_TU_SECRET = '<client_secret>'
```

Sample SOCIAL_AUTH_PIPELINE
```python
SOCIAL_AUTH_PIPELINE = [
    'social_core.pipeline.social_auth.social_details',
    'social_core.pipeline.social_auth.social_uid',
    'social_core.pipeline.social_auth.social_user',
    'social_core.pipeline.user.get_username',
    'social_core.pipeline.user.create_user',
    'social_core.pipeline.social_auth.associate_user',
    'social_core.pipeline.social_auth.load_extra_data',
    'social_core.pipeline.user.user_details',
    'social_core.pipeline.social_auth.associate_by_email',
]
```
> See more detail in about **social-app-django** in (https://github.com/python-social-auth/social-app-django)

### in urls.py
```
urlpatterns = [
    ...
    path('oauth/', include('social_django.urls', namespace='social')), # in django2
    ...
]
```

### in template
```
    ...
        <a href="{% url 'social:begin' 'tu' %}">Login with TU</a><br>
    ...
```

Response
=======

### Sample response data
```
HTTP 200 OK
Allow: GET, HEAD, OPTIONS
Content-Type: application/json
Vary: Accept

{
    "birthdate": "...",
    "email": null,
    "username": "...",
    "isactive": "...",
    "firstname": "...",
    "lastname": "...",
    "fullname": "...",
    "staffmail": "...",
    "type": "...",
    "tumail": "...",
    "department": "...",
    "company": "...",
    "role": "1"
}
```

### Roles Meaning

| Value | Meaning    |
|-------|------------|
|       |            |
| 1     | STUDENT    |
| 2     | STAFF      |
| 3     | INSTRUCTOR |

Note: for some case user doesn't have a role(for test id) you need to handle it by yourself.
