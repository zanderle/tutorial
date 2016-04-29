# Django views - time to create!

A *view* is a place where we put the "logic" of our application. It is where we decide what information we want to show and perform any actions before that. A view will be connected to an *URL*, which we will se in the next chapter. First, we'll need to define our view. And because our views live in our application, we first need to create an application.

### Creating an application

To keep everything tidy, we will create a separate application inside our project. It is very nice to have everything organized from the very beginning. To create an application we need to run the following command in the console (from `djangogirls` directory where `manage.py` file is):

    (myvenv) ~/djangogirls$ python manage.py startapp blog

You will notice that a new `blog` directory is created and it contains a number of files now. Our directories and files in our project should look like this:
    
    djangogirls
    ├── blog
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── migrations
    │   │   └── __init__.py
    │   ├── models.py
    │   ├── tests.py
    │   └── views.py
    ├── db.sqlite3
    ├── manage.py
    └── mysite
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py

After creating an application we also need to tell Django that it should use it. We do that in the file `mysite/settings.py`. We need to find `INSTALLED_APPS` and add a line containing `'blog',` just above `)`. So the final product should look like this:

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
)
```


Views are placed in the `views.py` file. We will add our *views* to the `blog/views.py` file.

## blog/views.py

OK, let's open up this file and see what's in there:

```python
from django.shortcuts import render

# Create your views here.
```


Not too much stuff here yet.

Lines that start with `#` are comments - it means that those lines won't be run by Python. Pretty handy, right?

The simplest *view* can look like this.

```python
def post_list(request):
    return render(request, 'blog/index.html', {})
```

Here, we defined (`def`) a view called `post_list` that will take `request` and then `render` our template `blog/index.html`.

> NOTE: a view is just a custom Python function.

## Django templates

As we learned in the previous chapters, we use HTML to display our web pages. In Django, we call these files `templates`. In the next chapters we'll see different things that templates allow us to do. For now all we need to know is where Django can find them.

Templates are saved in `blog/templates/blog` directory. So first create a directory called `templates` inside your blog directory. Then create another directory called `blog` inside your templates directory:

    blog
    └───templates
        └───blog

(You might wonder why we need two directories both called `blog` - as you will discover later, this is simply a useful naming convention that makes life easier when things start to get more complicated.)

Now move your `index.html` file inside the `blog/templates/blog` directory.

> TODO: css won't work. Maybe leave it inside HTML first, and move it to a separate file in later chapters?

See how your website looks now: http://127.0.0.1:8000/

![It worked!](images/it_worked2.png)

Nothing changed! That's because we haven't told Django what *URL* our `post_list` view should be connected to. We'll find out how to do that in the next chapter.


> Learn more about Django views by reading the official documentation: https://docs.djangoproject.com/en/1.8/topics/http/views/
