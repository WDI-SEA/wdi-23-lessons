# Django REST Framework

## Learning Objectives

* Describe the Django REST Framework and its benefits.
* Start a Django project with the REST framework
* Create and access a data API hosted in Django

## What is it?

Django REST Framework is a third-party Python framework for Django. It can be added to projects like we add apps and it provides a layer of functionality through which Django will actually behave like a proper RESTful API server.

## Key Concepts

Django by itself is great at using its ORM in conjunction with model forms and editing views to retrieve, display and edit data. Strangely, when we start using it only to serve data via our views/urls is when it starts to break down. Much of the functionality we were using to build CatCollector and get the data to show up correctly was handled by those generic editing view classes. Well, we wouldn't be using those if Django is only our backend, and especially not if we want to have a React front-end.

The Django REST Framework provides us with a couple of powerful tools:

### Serializers

As we will see shortly, there is a big problem in trying to get Django's ORM data into the appropriate format to be sent over the wire. We must import and use a handful of badly organized functions and classes to convert the data. The REST framework provides a lovely set of model serializers (that work much like the editing view classes) that we can use to easily serialize any data from any of our models.

### View Wrappers

Django has some sophisticated view features but most of them relate to the use of model forms and, again, we want to only use it as a backend. The REST framework gives us classes and view decorators that do a number of things:

1. Provide a more robust `Request` object that can properly handle all the HTTP verbs (GET, POST, PUT, and DELETE).
2. Provide a more robust `Response` object capable of detecting appropriate content types for the clients requesting data.
3. Provide us with an array of preset HTTP statis code constants that are extremely easy to read and use.
4. Give us the ability to send JSON or XML automatically.

## Get this show on the road!

Go into your unit4 directory and create a new Django project:

```bash
django-admin startproject djangobackend
```

Go into the new `djangobackend` folder and start a new app:

```bash
django-admin startapp main_app
```

While we are in the terminal, let's install the REST framework:

```bash
pip3 install djangorestframework
```

It is now installed for the system and we can add it into this new Django project. Open that `djangobackend` folder and in your text editor. You should see the `djangobackend` subfolder and in there is settings.py. Let's open that up and find the `INSTALLED_APPS` section. Add `main_app` and `rest_framework` at the top:

```python
INSTALLED_APPS = [
    'main_app',
    'rest_framework',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

While we are in here, let's update the database section to use Postgres:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'djangobackend',
    }
}
```

Back in the terminal, let's make that database:

```bash
createdb djangobackend
```

That's about all the setup we need to get started. Simply installing the REST framework and adding it into your apps is all you need to do to use it.

## Adding a model

This is going to be an API so we should choose what resource we will store in our database.

YOU CHOOSE!

Now we will add that model to our `main_app/models.py` file. Here's a Cat as an example but we are using what you chose:

```py
from django.db import models

# Create your models here.
class Cat(models.Model):
  name = models.CharField(max_length=100)
  description = models.TextField(max_length=250)
  age = models.IntegerField()

  def __str__(self):
    return self.name
```

With our model saved, we can make and run our migrations:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

And of course we have the easy admin interface to be able to add data to this. Add yourself as a super user:

```bash
python3 manage.py createsuperuser
```

To register our model, we need to add it to the `main_app/admin.py` file:

```python
from django.contrib import admin
# import your models here
from .models import Cat

# Register your models here
admin.site.register(Cat)
```

Phew! Now we can add some data. Start the server, log into the admin panel, and add some data!

## Reasoning out Django

Now, a sane person who had gone through a Django tutorial or two would think they have a handle on how to make a url, make a view for it, and get some data to return to the client. Let's see about doing that:

### URL

Add a URL for getting all of the data in your collection. An `index` action! Make a `urls.py` inside `main_app`:

```py
# main_app/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('cats/', views.cats_index, name='cats_index'),
```

Now in `views.py` we can add the view function:

```py
from .models import Cat

def cats_index(request):
    cats = Cat.objects.all()
```

Hm, how do we return this data? I doubt that HttpResponse will work but let's try it:

```py
from .models import Cat

def cats_index(request):
    cats = Cat.objects.all()
    return HttpResponse(cats)
```

Fire up Postman to test your route: `http://localhost:8000/cats/`. Okay, no that didn't work. Oh, there's a JsonResponse object, maybe that's it. (It's not.) Maybe it just needs some extra parameters. (I promise you, it doesn't work.)

Alright fine, what does work. Let's look at this code:

```py
from django.core import serializers

from django.forms.models import model_to_dict
import json

def make_django_work_properly(data):
  data_type = type(data)
  if ('QuerySet' in str(data_type)):
    print("we are a queryset")
    return serializers.serialize("json", data)
  else:
    print("i am a model")
    dict_data = model_to_dict(data)
    return json.dumps(dict_data)
```

First, we must determine whether we have found one single record in the database or if we have found more than one. We use a completely different method for each. If we have a query set, we are in luck and can use one of the built-in Django serializers. We simply import it from django.core and then call it to serialize our data to JSON format. That's cool but it doesn't work if you have only a single record returned which Django treats as a model and not a query set. To serialize a model, Django's built-in serializers are not sufficient, (strangely.) We must import a `model_to_dict` function from django.forms that is used to change a model into a dictionary. After that, we can use another imported library for dealing with JSON and use the `dumps` function to get the dictionary into JSON format.

This, to me, is really badly organized code and it turned out that I was not the only one who felt this way. The Django REST Framework authors took up the challenge to make this a semi-enjoyable and exponentially more useful experience.

## First, the serializers

Yes, that nastiness I was writing above is something that the REST framework has built-in. Thanks, yall! Let's see how stupidly easy these are to use. Make a file inside `main_app` and call it `serializers.py`. Add this code:

```python
from rest_framework import serializers
from .models import Cat

class CatSerializer(serializers.ModelSerializer):
    class Meta:
        model = Cat
        fields = ('id', 'name', 'description', 'age')
```

Just like those forms that were based on our models, this is a serializer specially made for our model. It know exactly how to translate from Django's data types to JSON for us.

## Then, the view wrappers

We need to rewrite our view somewhat (which is fine because it wasn't really working anyway.) When we think about writing view functions with the REST framework, we need to think about them in terms of what URL they would be accessing, the two choices being:

1. The whole collection (/widgets) - Used by GET All and POST New
2. One specific item (/widgets/5) - Used by GET One, PUT One, and DELETE One

These are conventionally referred to as the "list" and the "detail", respectively. We will make one view function for each but they will contain conditionals to test for which verb is being used. Let's start with GET All:

```python
from .models import Cat
from .serializers import CatSerializer

from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response

@api_view(['GET'])
def cats_list(request):
  if request.method == 'GET':
    cats = Cat.objects.all()
    serializer = CatSerializer(cats, many=True)
    return Response(serializer.data)
```

Looking at the code, we see we bring in our data model and our serializer for it. Then we need some things imported from the REST framework. Finally, we see the view function which detects a GET request, gets all the cats, serializes them, and returns them in the response.

Let's add the view function for view one item's details:

```py
@api_view(['GET'])
def cats_detail(request, pk):
  if request.method == 'GET':
    cat = Cat.objects.get(pk=pk)
    serializer = CatSerializer(cat)
    return Response(serializer.data)
```

We'll need a URL for this. Let's rename the other one to `cats_list` and add `cats_detail`:

```py
urlpatterns = [
    path('cats/', views.cats_list, name='cats_list'),
    path('cats/<int:pk>/', views.cats_detail, name='cats_detail'),
```

Time to do a little testing. With the server running, we should be able to use Postman to hit both `http://localhost:800/cats/` and `http://localhost:800/cats/1/` and see valid JSON data returned.

## Adding more CRUD operations

We only need these two views but we can add multiple verbs to each. Let's update our `cats_list` to support POSTing a new item:

```py
@api_view(['GET', 'POST'])
def cats_list(request):
  if request.method == 'GET':
    cats = Cat.objects.all()
    serializer = CatSerializer(cats, many=True)
    return Response(serializer.data, status=status.HTTP_200_OK)
  elif request.method == 'POST':
    serializer = CatSerializer(data=request.data)
    if serializer.is_valid():
      serializer.save()
      return Response(serializer.data, status=status.HTTP_201_CREATED)
    return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

Let's look at what we did here. We added a string of 'POST' to the array in the `@api_view` decorator. We added an `elif` branch that is checking the request to see if it is a POST. And we added status codes onto our responses, making them a lot more helpful. This also shows us how we can receive data and store it back in the database. The serializer acts a lot like the forms we used in the CatCollector. We can validate them and then save them and send a status code back.

## Your turn

We have our C,R, and R routes, you need to add U and D. Update and delete are both done to individual records so they belong in the `cats_detail` view function. You will need to add conditional branches for the new verbs, update the decorator to include the new verbs, and access the database accordingly.