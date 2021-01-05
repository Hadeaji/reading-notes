# Django Custom User

For a real-world project, the official Django documentation highly recommends using a custom user model instead. This provides far more flexibility down the line so, as a general rule, always use a custom user model for all new Django projects.

## Setup

To start, create a new Django project from the command line. We need to do several things:

- create and navigate into a dedicated directory called accounts for our code
- install Django
- make a new Django project called config
- make a new app accounts
- start the local web server to make sure everything working fine

## AbstractUser vs AbstractBaseUser

There are two modern ways to create a custom user model in Django: `AbstractUser` and `AbstractBaseUser`.

> `AbstractBaseUser` requires much, much more work. Seriously, don't mess with it unless you really know what you're doing

So we'll use `AbstractUser` which actually subclasses `AbstractBaseUser` but provides more default configuration.

## Custom User Model

Creating our initial custom user model requires four steps:

- update `config/settings.py`
- create a new `CustomUser` model
- create new `UserCreation` and `UserChangeForm`
- update the admin

In `settings.py` we'll add the `accounts` app and use the AUTH_USER_MODEL config to tell Django to use our new custom user model in place of the built-in User model. We'll call our custom user model `CustomUser`

Within `INSTALLED_APPS` add `accounts` at the bottom. Then at the bottom of the entire file, add the `AUTH_USER_MODEL` config.

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'accounts', # new
]
...
AUTH_USER_MODEL = 'accounts.CustomUser' # new
```

Now update accounts/models.py with a new User model which we'll call CustomUser.

```
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    pass
    # add additional fields in here

    def __str__(self):
        return self.username
```

We need new versions of two form methods that receive heavy use working with users. Stop the local server with Control+c and create a new file in the accounts app called forms.py.

`touch accounts/forms.py`

We'll update it with the following code to largely subclass the existing forms.

```
# accounts/forms.py
from django import forms
from django.contrib.auth.forms import UserCreationForm, UserChangeForm
from .models import CustomUser

class CustomUserCreationForm(UserCreationForm):

    class Meta:
        model = CustomUser
        fields = ('username', 'email')

class CustomUserChangeForm(UserChangeForm):

    class Meta:
        model = CustomUser
        fields = ('username', 'email')

# accounts/admin.py
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin

from .forms import CustomUserCreationForm, CustomUserChangeForm
from .models import CustomUser

class CustomUserAdmin(UserAdmin):
    add_form = CustomUserCreationForm
    form = CustomUserChangeForm
    model = CustomUser
    list_display = ['email', 'username',]

admin.site.register(CustomUser, CustomUserAdmin)
```

## Superuser

`python manage.py createsuperuser`

## Templates/Views/URLs

Our goal is a homepage with links to log in, log out, and sign up. Start by updating settings.py to use a project-level templates directory.

```
# config/settings.py
TEMPLATES = [
    {
        ...
        'DIRS': [str(BASE_DIR.joinpath('templates'))], # new
        ...
    },
]
```

Then set the redirect links for log in and log out, which will both go to our home template. Add these two lines at the bottom of the file.

```
# config/settings.py
LOGIN_REDIRECT_URL = 'home'
LOGOUT_REDIRECT_URL = 'home'
```

Create a new project-level templates folder and within it a registration folder as that's where Django will look for the log in template. We will also put our signup.html template in there.

create four templates and Update the files
