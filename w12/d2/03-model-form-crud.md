# The New & Improved Cat Collector Code-Along, Part 3

Wonderful of you to return! In part 3 of Cat Collector we will be seeing how Django provides powerful forms that we can use to enable any CRUD operation on our data quickly and easily. These are sort of Django's alternative to providing 100% proper RESTful routing support but it does end up saving us a lot of time even if the naming conventions aren't quite RESTful.

## Model CRUD in Django

In more minimalist frameworks, the duty of creating proper RESTful routes and forms for all operations on every resource in our app falls on us - the developers. But larger frameworks like Django or Rails often provide a quick way to cut through all the boilerplate and let us just deliver a form that is already made foer showing or updating our data. Django's solution is `ModelForms` and `generic editing views` which are classes that we can use to make a form that is already connected to our models and allows the easiest possible editing of our data.

We have already worked two READ operations into our app: `read all` and `read one`. Now let's add in the ability to `create` cats, `update` cats, and `delete` cats.

## Create-a-cat

Typically this operation is an HTTP POST request that is sent to the collection URL: `/cats`. In order for any creation of a new data object to be possible via the web, we need to follow a process:

1. The browser must be sent a page with a form on it.
2. The form must be filled out and submitted by the user.
3. The form is then returned to the server via a POST with the data attached.
4. The server reads the form data, validates it, and writes it to the table.

This is a fairly complicated process to get right when we do it ourselves but Django handles most of this internally in the form classes it provides us.

Open your `main_app/views.py` file and we will add the following code:

```python
# add this line to the import at the top
from django.views.generic.edit import CreateView, UpdateView, DeleteView

class CatCreate(CreateView):
  model = Cat
  fields = '__all__'

class CatUpdate(UpdateView):
  model = Cat
  fields = ['name', 'breed', 'description', 'age']

class CatDelete(DeleteView):
  model = Cat
  success_url = '/cats'
```

For the `create` and `update` forms, you must tell it which fields need to be mapped to the form. The `delete` operation needs no form but there is another page we must create for it. Let's create some templates! Inside `templates` these forms are expected to be in a folder with the same name as our app so let's create a `main_app` folder. Inside there, make a `cat_form.html` file and give it these contents:

```html
{% extends 'base.html' %}
{% load staticfiles %}

{% block content %}
	<form action="" method="post">
    {% csrf_token %}
    <table>
    {{ form.as_table }}
    </table>
    <input type="submit" value="Submit!">
  </form>
{% endblock %}
```

This form will be used for both `creating` and `updating`. The `delete` operation wants another file present. It will be used to confirm the deletion, basically asking the user if they are sure about what they are doing. In the same directory, create `cat_confirm_delete.html` and fill it thusly:

```html
{% extends 'base.html' %}
{% load staticfiles %}

{% block content %}
	<h1>Delete Author</h1>

  <p>Are you sure you want to delete this cat: {{ cat }}?</p>

  <form action="" method="POST">
    {% csrf_token %}
    <input type="submit" value="Yes, delete.">
  </form>
{% endblock %}
```

Now on to the URLs. Open up `main_app/urls.py` and add these to the `urlpatterns`:

```python
path('cats/create/', views.CatCreate.as_view(), name='cats_create'),
path('cats/<int:pk>/update/', views.CatUpdate.as_view(), name='cats_update'),
path('cats/<int:pk>/delete/', views.CatDelete.as_view(), name='cats_delete'),
```

Now we see some routes here that use Django's syntax for variable in URLs. But in this case, we cannot choose what we name them. They must always be type `int` and be named `pk`. This stands for `primary key` and it is the variable name that Django expects the Cat ID to be in. This is another Django convention that we must accept.