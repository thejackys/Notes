1. Start a project

```python
django-admin startproject projectname
cd projectname
```

What's learned:
* Use Django's tools to create a skeleton website and application.
* Start and stop the development server.
* Create models to represent your application's data.
* Use the Django admin site to populate your site's data.
* Create views to retrieve specific data in response to different requests, and templates to render the data as HTML to be displayed in the browser.
* Create mappers to associate different URL patterns with specific views.
* Add user authorization and sessions to control site behavior and access.
* Work with forms.
* Write test code for your app.
* Use Django's security effectively.
* Deploy your application to production.

# What does django sample directory looks like
``` Bash
locallibrary/  #sample project directoy
    manage.py #  used to create applications, work with databases, and start the development web server.
    locallibrary/
        __init__.py # instructs Python to treat this directory as a Python package.
        settings.py #contains all the website settings, including registering any applications we create, the location of our static files, database configuration details, etc.
        urls.py #defines the site URL-to-view mappings.
        wsgi.py #used to help your Django application communicate with the web server.
        asgi.py #standard for Python asynchronous web apps and servers to communicate with each other. Asynchronous Server Gateway Interface (ASGI) is the asynchronous successor to Web Server Gateway Interface (WSGI). ASGI provides a standard for both asynchronous and synchronous Python apps, whereas WSGI provided a standard for synchronous apps only. ASGI is backward-compatible with WSGI and supports multiple servers and application frameworks.
```

## manage.py

```Bash
python3 manage.py startapp catalog # Create an app
```

### Migrations
Django uses Object-Relational-Mapper (ORM).
It provides database migration scripts when our models change to auto migrate data structure to database.
```python
python3 manage.py makemigrations
python3 manage.py migrate

```

### runserver
```Bash
python3 manage.py runserver #run the server
```

## Models
A typical model(a sql Field)
```python
from django.db import models
from django.urls import reverse

class MyModelName(models.Model):
    """A typical class defining a model, derived from the Model class."""

    # Fields
    my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
    # …

    # Metadata
    class Meta:
        ordering = ['-my_field_name']

    # Methods
    def get_absolute_url(self):
        """Returns the URL to access a particular instance of MyModelName."""
        return reverse('model-detail-view', args=[str(self.id)])

    def __str__(self):
        """String for representing the MyModelName object (in Admin site etc.)."""
        return self.my_field_name
```
[model type:](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types)
## Class Metadata
can:
1. Can control the data ordering when retrieved.

``` python

class Meta:
    ordering = ['-my_field_name']

```

2. create and apply new "access permissions" for the model (default permissions are applied automatically)
   [Model metadata options ](https://docs.djangoproject.com/en/5.0/ref/models/options/)

## Methods
Minimally
```python
def __str__(self):
    return self.my_field_name
def get_absolute_url(self):
    """Returns the URL to access a particular instance of the model."""
    return reverse('model-detail-view', args=[str(self.id)])

```
## CRUD
```python
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
# Access model field values using Python attributes.
print(record.id) # should return 1 for the first record.
print(record.my_field_name) # should print 'Instance #1'

# Change record by modifying the fields, then calling save().
record.my_field_name = "New Instance Name"
record.save()

### Search
all_books = Book.objects.all()
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
# Will match on: Fiction, Science fiction, non-fiction etc.
books_containing_genre = Book.objects.filter(genre__name__icontains='fiction')

```
[Searching Docs](https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups)
[making queries](https://docs.djangoproject.com/en/5.0/topics/db/queries/)


## Django admin site

### Register models
Register in admin.py
```python
from .models import Author, Genre, Book, BookInstance, Language

admin.site.register(Book)
admin.site.register(Author)
...
```
### customize the interface 
* List views:
   - Add additional fields/information displayed for each record.
   - Add filters to select which records are listed, based on date or some other selection value (e.g. Book loan status).
   - Add additional options to the actions menu in list views and choose where this menu is displayed on the form.
* Detail views
  * Choose which fields to display (or exclude), along with their order, grouping, whether they are editable, the widget used, orientation etc.
  * Add related fields to a record to allow inline editing (e.g. add the ability to add and edit book records while you're creating their author record).


# Adding pylint django to vscode
[tutorial]{https://dkolodzey.medium.com/pylint-for-django-in-vscode-f3fadb8462d}

# Security

1. secret key in settings.py: