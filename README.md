Dajgno TU Authentication
========================

Requirements
============
- python (2.7, 3.5)
- django (1.11, 2.0)
- social-app-django (2.1.0)

Installation
============
```
pip install git+https://github.com/LeOntalEs/django-tuauth.git
```

Usage
=====
### Prerequisite
Register application in api.tu.ac.th/applications
> note: Callback URL must be same with decarelation in urls.py
> in this example use http://127.0.0.1/oauth/complete/

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
    'tuauth.backend.TUOAuth2',
    ...
)
```
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