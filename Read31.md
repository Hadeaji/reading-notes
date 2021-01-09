# REST Framework & Docker

## Docker

Docker is really just Linux containers which are a type of virtualization.

Virtualization has its roots at the beginning of computer science when large, expensive mainframe computers were the norm. How could multiple programmers use the same single machine? The answer was virtualization and specifically virtual machines which are complete copies of a computer system from the operating system on up.

### Containers vs Virtual Environments

Virtual environments are used to isolate Python software packages locally. We can create an isolated box for individual projects so one can use Python 2.7 and Django 1.5 while another can use Python 3.5 and Django 2.1 on the same computer. Virtual environments are great.

But…virtual environments can only isolate Python packages. They still rely on a global, system-level installation of Python albeit they can refer to the proper version. So when we use Python 2.7 in a project, we’re pointing to an installation of Python 2.7 on the computer itself, not actually within the virtual environment.

Also we can’t run a production database or other services within virtual environments so compared to Docker containers they are far more limited.

### Install Docker

after downlading and installing Once Docker is done installing we can confirm the correct version is running. It should be at least version 19.

```
$ docker --version
Docker version 19.03.5, build 633a0ea
```


Docker Compose is an additional tool that is automatically included with Mac and Windows downloads of Docker. However if you are on Linux, you will need to add it manually. You can do this by running the command 
`sudo pip install docker-compose` after your Docker installation is complete.

To confirm Docker installed correctly we can run our first command `docker run hello-world`

This will download an official image and then run the container.

Want to inspect just the current image?

`docker image ls`

### Images and Containers

When we ran hello-world we used an official Docker image. We did not have to create the image ourself. But typically we will create custom images and we do so using a Dockerfile. We also will use docker-compose.yml files to run the containers.

A baking analogy we can use here is as follows:

A Dockerfile is the recipe for a cake

- An image is a snapshot of the recipe at a given time
- A docker-compose.yml says how to make the cake
- And the container is the actual, baked cake

#### Images

First create a local directory on your computer for our code. I’ve chosen to make a code folder on my Desktop (I’m using an Apple computer) and then a python3.7 folder within that.

```
cd ~/Desktop
mkdir code && cd code
mkdir python3.7 && cd python3.7

touch Dockerfile

```

With your text editor add the following single line of code to the Dockerfile.

`FROM python:3.7-alpine`

#### Image Builds

With our instructions complete it’s time to “build” or create the image for the first time. Don’t forget that period `.` at the end of the command!

`docker image build .`

There will be a lot of output here ending with Successfully built and the hash id, `b2c276538711`, for the image. The actual id hashes may change by the time you read this post as each image continually updated by the Docker community.

#### Image Layers

Every image is made up of one or more image layers. The base layer is often a flavor of Linux, like `alpine`.

#### Containers

Now that we have our Python image how do we run it? Well just as a Dockerfile is a list of image instructions there is also a docker-compose.yml file that is a list of container instructions.

To demonstrate a real-world image and container example we will now “Dockerize” a basic Django web app. You might not fully understand every step but you’ll see how Docker can work to run a web application completely on its own.

On the command line, go up a directory to our code folder and create a new one called djangoapp. Then we’ll use Pipenv to install Django and enter the virtual environment shell.

```
cd ..
mkdir djangoapp && cd djangoapp
pipenv install django==3.0
pipenv shell
```
This creates both a Pipfile and a Pipfile.lock. Now create an example_project that we’ll put in our current directory. Don’t forget the period . at the end of the command here!

```
django-admin startproject example_project .
```
And then we can use runserver to start this new Django project.

```
python manage.py runserver
```

### Dockerized Django

We’ll now create a Dockerfile for our image which will completely replace our local dev environment, so this will have Python 3 and Django. Then we’ll add a docker-compose.yml for the container instructions. Make each file via the command line.

```
touch Dockerfile
touch docker-compose.yml
```

The important takeaways from this tutorial are this:

- Docker is a way to run Linux containers
- Containers are a lightweight alternative to Virtual Machines
- Dockerfile is a list of instructions for creating an image
- Images are made up of one or more layers
- Containers are a running instance of an image
- docker-compose.yml controls how to run the container
- Containers are stateless and ephemeral in nature. We can link the local filesystem via volumes but things become more complex with databases.


## Library Website and API

Django REST Framework works alongside the Django web framework to create web APIs. We cannot build a web API with only Django Rest Framework; it always must be added to a project after Django itself has been installed and configured.

### Traditional Django

First we need a dedicated directory on our computer to store the code. This can live anywhere but for convenience, if you are on a Mac, we can place it in the Desktop folder. The location really does not matter; it just needs to be easily accessible.

```
cd ~/Desktop
mkdir code && cd code
```

Now we are within the code folder which will be the location for all the code in this book. The next step is to create a dedicated directory for our library site, install Django via Pipenv, and then enter the virtual environment using the shell command. You should always use a dedicated virtual environment for every new Python project.

```
mkdir library && cd library
pipenv install django~=3.1.0
pipenv shell

(library) $
```

Pipenv creates a Pipfile and a Pipfile.lock within our current directory. The (library) in parentheses before the command line shows that our virtual environment is active

raditional Django website consists of a single project and one (or more) apps representing discrete functionality. Let’s create a new project with the startproject command. Don’t forget to include the period . at the end which installs the code in our current directory. If you do not include the period, Django will create an additional directory by default.

```
(library) $ django-admin startproject config .
```

Django automatically generates a new project for us which we can see with the tree command. (Note: If tree doesn’t work for you on a Mac, install it with Homebrew: `brew install tree`.)

The files have the following roles:

- `__init__.py` is a Python way to treat a directory as a package; it is empty
- `asgi.py` stands for Asynchronous Server Gateway Interface and is a new option in Django 3.0+
- `settings.py` contains all the configuration for our project
- `urls.py` controls the top-level URL routes
- `wsgi.py` stands for Web Server Gateway Interface and helps Django serve the eventual web pages
- `manage.py` executes various Django commands such as running the local web server or creating a new app.

### Django REST Framework

On the command line type the below.

```
pipenv install djangorestframework~=3.11.0
```

Add it to the `INSTALLED_APPS` config in our `settings.py` file. I like to make a distinction between third-party apps and local apps as follows since the number of apps grows quickly in most projects.

```
# config/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # 3rd party
    'rest_framework', # new

    # Local
    'books',
]
```

There are multiple ways we can organize these files however my preferred approach is to create a dedicated api app. That way even if we add more apps in the future, each app can contain the models, views, templates, and urls needed for dedicated webpages, but all API-specific files for the entire project will live in a dedicated api app.

Let’s first create a new api app.

```
python manage.py startapp api
```

Then add it to `INSTALLED_APPS`

The api app will not have its own database models so there is no need to create a `migration` file and run migrate to update the database.

- URLs:

First at the project-level we need to `include` the `api` app and configure its URL route,which will be `api/`.

Then create a `urls.py` file within the api app

`touch api/urls.py` And update it as follows:

```
from django.urls import path
from .views import BookAPIView

urlpatterns = [
    path('', BookAPIView.as_view()),
]
```

- Views:

Next up is our views.py file which relies on Django REST Framework’s built-in generic class views. These deliberately mimic traditional Django’s generic class-based views in format, but they are not the same thing.

Within our views.py file, update it to look like the following:

```
from rest_framework import generics

from books.models import Book
from .serializers import BookSerializer


class BookAPIView(generics.ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

Then we create a BookAPIView that uses ListAPIView to create a read-only endpoint for all book instances. There are many generic views available and we will explore them further in later chapters.

The only two steps required in our view are to specify the queryset which is all available books, and then the serializer_class which will be `BookSerializer`.

### Serializers

A serializer translates data into a format that is easy to consume over the internet, typically JSON, and is displayed at an API endpoint. We will also cover serializers and JSON in more depth in following chapters. For now I want to demonstrate how easy it is to create a serializer with Django REST Framework to convert Django models to JSON.

Make a `serializers.py` file within our `api` app.

`touch api/serializers.py`

Then update it as follows in a text editor:

```
from rest_framework import serializers

from books.models import Book


class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = ('title', 'subtitle', 'author', 'isbn')
```

On the top lines we import Django REST Framework’s serializers class and the Book model from our books app. We extend Django REST Framework’s ModelSerializer into a BookSerializer class that specifies our database model Book and the database fields we wish to expose: title, subtitle, author, and isbn.

- cURL:

We want to see what our API endpoint looks like. We know it should return JSON at the URL 
`http://127.0.0.1:8000/api/` after running the server

Now open a new, second command line console. We will use it to access the API running in the existing command line console.
