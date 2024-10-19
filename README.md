
# LOGIN THROUGH SOCIAL ACCOUNTS
## STEP1:
CREATE PROJECT:
```
django-admin startproject djangoAllauth
```
## STEP2:

CREATE APP:
```
python manage.py startapp deshboard
```
## STEP3:

RUN THE SERVER:
```
python manage.py runserver
```
## STEP4:

RUN MIGRATTIONS:
```
python manage.py migrate
```
## STEP5:

INSTALL ALLAUTH:
```
pip install django-allauth
```
## STEP6:

OPEN SETTINGS.PY IN 'djangoAllauth' FOLDER & ADD:

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.sites',   
    'deshboard',   
 
    'allauth',   
    'allauth.account',  
    'allauth.socialaccount',
    'allauth.socialaccount.providers.google', 
    'allauth.socialaccount.providers.facebook', 
    'allauth.socialaccount.providers.github', 
]
```


```
AUTHENTICATION_BACKENDS = (
 'django.contrib.auth.backends.ModelBackend',
 'allauth.account.auth_backends.AuthenticationBackend',
 )
```

```
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
```

```
SOCIALACCOUNT_PROVIDERS = {
    'google': {
        'SCOPE': [
            'profile',
            'email',
        ],
        'AUTH_PARAMS': {
            'access_type': 'online',
        }
    }
}
```

```
SITE_ID = 1
LOGIN_REDIRECT_URL = '/'
# LOGOUT_REDIRECT_URL = '/'
```

```
# ACCOUNT_AUTHENTICATION_METHOD = 'email'
# ACCOUNT_EMAIL_REQUIRED = True
# ACCOUNT_USERNAME_REQUIRED = False
```

```
SOCIALACCOUNT_LOGIN_ON_GET = True
ACCOUNT_LOGOUT_ON_GET= True
```

## STEP7:

CREATE TEMPLATE AND ADD home.html:
```
{% load socialaccount %}
{% providers_media_js %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home Page</title>
</head>
<body>
    <div style="display:flex; gap:50px;">
        <h1>Home Page</h1>
        <div style="display:flex; align-items:center;">
            {% if request.user.is_authenticated %}
                <a href="{% url 'account_logout' %}"><b>Logout</b></a>
            {% else %}
                <div style="display:flex; gap:50px;">
                    <button><a href="{% provider_login_url 'google' %}">Login with Google</a></button>
                    <button><a href="{% provider_login_url 'facebook'  method='oauth2' %}">Login with Facebook</a></button> 
                    <button><a href="{% provider_login_url 'github' %}">Login with GitHub</a></button> 
                    <button><a href="{% url 'account_login' %}"><b>Login</b></a></button>
                </div>
            {% endif %}
        </div>
    </div> 
    <hr>

    <div style="width: 50%; margin:auto; border: 2px solid black; padding: 20px;">
        {% if request.user.is_authenticated %}
            <h2>Username: {{ request.user.username }} <br> Email: {{ request.user.email }}</h2>
        {% else %}
            <h2>Not Logged In</h2>
        {% endif %}
         
    </div>

</body>
</html>
```

## STEP8:
ADD PATH IN URLS.PY:
```
from django.contrib import admin
from django.urls import path, include
from deshboard.views import home

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', home, name='home'),
    path('accounts/', include('allauth.urls')), #allauth
]
```

## STEP9:
RUN MIGRATIONS:

```
python manage.py makemigrations
python manage.py migrate
```
## STEP10:

#### Google:

1.Go to this link: https://console.developers.google.com/

2.Dashbord create a project and proceed

3.Dashbord go to Credentials  On the dropdown, choose OAuth Client ID option

4.Dashbord OAuth consent screen create external give app name and save

5.Create OAuth client ID: 

Authorized Javascript origins: http://127.0.0.1:8000

Authorized redirect URL: http://127.0.0.1:8000/accounts/google/login/callback/

6.Get the credentails

#### Facebook:
1.Go to this link: https://console.developers.google.com/

2.Create App

3.Then, Go to the “App settings” and select “Basic”.

4.Get the credentails

#### Github:
1.Go to this link: https://github.com/settings/applications/new

2.Homapage link: http://localhost:8000/ 

3.Authorized callback url: http://localhost:8000/accounts/github/login/callback/

## STEP11:
CREATE SUPERUSER TO VIEW ADMIN PANEL:
```
python manage.py createsuperuser
```

## STEP12:
ADD SITEID ITS UNIQUE TO EACH CLIENTID.

USING TERMINAL ENTER THIS CMD:

```
python manage.py shell
from django.contrib.sites.models import Site
sites=Site()
sites.domain='http://127.0.0.1:8000/' 
sites.name='htpp://127.0.0.1:8000/'
sites.save()
print(sites.id)
exit()
```
we get the id we need to add in settings.py file in SITE_ID

## STEP13:
Add social applications:

Provider: Google/Facebook/GitHub

Name: Google/Facebook/GitHub

Client id: 

Secret key: 


## STEP14:

SET UP THE PATH IN ADMIN.

## STEP15:

START USING IT THANK YOU!!
