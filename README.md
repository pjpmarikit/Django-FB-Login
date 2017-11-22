### Django-FB-login

This repository is a quick implementation of django's social auth application with Facebook. This was heavily based on Vitor Freitas's [tutorial](https://simpleisbetterthancomplex.com/tutorial/2016/10/24/how-to-add-social-login-to-django.html).

### Installation
This project requires Django framework and social auth app.

```sh
$ pip install django
$ pip install social-auth-app-django
```
| Package | Version |
| ------ | ------ |
| Django | Django==1.11.7 |
| Social Auth | social-auth-app-django==2.0.0 / social-auth-core\==1.5.0 |
### Configuration of settings.py

Include `social_django` in `INSTALLED_APPS` :
```sh
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'social_django',  # HERE
]
```

Include `SocialAuthExceptionMiddleware` in `MIDDLEWARE_CLASSES`:
```sh
MIDDLEWARE_CLASSES = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',

    'social_django.middleware.SocialAuthExceptionMiddleware',  # HERE
]
```
Update `context_processors` in `TEMPLATE`:
```sh
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            PROJECT_DIR.child('templates'),
        ],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',

                'social_django.context_processors.backends',  # HERE
                'social_django.context_processors.login_redirect', # HERE
            ]
        },
    },
]
```
Add the variable `AUTHENTICATION_BACKENDS`:
```sh
AUTHENTICATION_BACKENDS = (
    'social_core.backends.facebook.FacebookOAuth2',
    'django.contrib.auth.backends.ModelBackend',
)
```
Add the following default variables for the default view being used:
```sh
LOGIN_URL = 'login'
LOGOUT_URL = 'logout'
LOGIN_REDIRECT_URL = 'home'
```

### Facebook Login App
Create a facebook app(https://developers.facebook.com/) to test the login. The app settings must look something like this (https://imgur.com/a/F0I3M).

It is important to get the following into `settings.py` depending on the facebook app's credentials.
```sh
SOCIAL_AUTH_FACEBOOK_KEY = '451821651x33143'  # App ID
SOCIAL_AUTH_FACEBOOK_SECRET = '524fada3c3ca5adgb279da535da1d863'  # App Secret
```

`NOTE`: Facebook does not recognize `127.0.0.1` as callback. This is why we are using `localhost` instead.