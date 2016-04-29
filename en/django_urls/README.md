# Django urls

We have our template, we have our view, but Django doesn't know where to use it. Let's define what URL we want to show this view and template at.

## What is a URL?

A URL is simply a web address. You can see a URL every time you visit a website - it is visible in your browser's address bar (yes! `127.0.0.1:8000` is a URL! And `https://djangogirls.com` is also a URL):

![Url](images/url.png)

Every page on the Internet needs its own URL. This way your application knows what it should show to a user who opens a URL. In Django we use something called `URLconf` (URL configuration). URLconf is a set of patterns that Django will try to match with the received URL to find the correct view.

## How do URLs work in Django?

Let's open up the `mysite/urls.py` file in your code editor of choice and see what it looks like:

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    # Examples:
    # url(r'^$', 'mysite.views.home', name='home'),
    # url(r'^blog/', include('blog.urls')),

    url(r'^admin/', include(admin.site.urls)),
]
```

As you can see, Django already put something here for us.

Lines that start with `#` are comments - it means that those lines won't be run by Python. Pretty handy, right?

The admin URL, which we will visit in the following chapters is already here:

```python
    url(r'^admin/', include(admin.site.urls)),
```

It means that for every URL that starts with `admin/` Django will find a corresponding *view*. In this case we're including a lot of admin URLs so it isn't all packed into this small file -- it's more readable and cleaner.

## Regex

Do you wonder how Django matches URLs to views? Well, this part is tricky. Django uses `regex`, short for "regular expressions". Regex has a lot (a lot!) of rules that form a search pattern. Since regexes are an advanced topic, we will not go in detail over how they work.

If you still wish to understand how we created the patterns, here is an example of the process - we will only need a limited subset of the rules to express the pattern we are looking for, namely:

	^ for beginning of the text
	$ for end of text
	\d for a digit
	+ to indicate that the previous item should be repeated at least once
	() to capture part of the pattern

Anything else in the url definition will be taken literally.

Now imagine you have a website with the address like that: `http://www.mysite.com/post/12345/`, where `12345` is the number of your post.

Writing separate views for all the post numbers would be really annoying. With regular expression we can create a pattern that will match the url and extract the number for us: `^post/(\d+)/$`. Let's break it down piece by piece to see what we are doing here:

* **^post/** is telling Django to take anything that has `post/` at the beginning of the url (right after `^`)
* **(\d+)** means that there will be a number (one or more digits) and that we want the number captured and extracted
* **/** tells django that another `/` character should follow
* **$** then indicates the end of the URL meaning that only strings ending with the `/` will match this pattern


## Your first Django url!

Time to create our first URL! We want 'http://127.0.0.1:8000/' to be a homepage of our blog and display a list of posts.

We also want to keep the `mysite/urls.py` file clean, so we will import urls from our `blog` application to the main `mysite/urls.py` file.

Go ahead, delete the commented lines (lines starting with `#`) and add a line that will import `blog.urls` into the main url (`''`).

Your `mysite/urls.py` file should now look like this:

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'', include('blog.urls')),
]
```

Django will now redirect everything that comes into 'http://127.0.0.1:8000/' to `blog.urls` and look for further instructions there.

When writing regular expressions in Python it is always done with `r` in front of the string. This is a helpful hint for Python that the string may contain special characters that are not meant for Python itself, but for the regular expression instead.


## blog.urls

Create a new `blog/urls.py` empty file. All right! Add these two first lines:

```python
from django.conf.urls import url
from . import views
```

Here we're importing Django's function `url` and all of our `views` from `blog` application (we don't have any yet, but we will get to that in a minute!)

After that, we can add our first URL pattern:

```python
urlpatterns = [
    url(r'^$', views.post_list, name='post_list'),
]
```

> TODO: Add about page as well (create view, template and url)

As you can see, we're now assigning our `post_list` view to `^$` URL. This regular expression will match `^` (a beginning) followed by `$` (an end) - so only an empty string will match. That's correct, because in Django URL resolvers, 'http://127.0.0.1:8000/' is not a part of the URL. This pattern will tell Django that `views.post_list` is the right place to go if someone enters your website at the 'http://127.0.0.1:8000/' address.

The last part `name='post_list'` is the name of the URL that will be used to identify the view. This can be the same as the name of the view but it can also be something completely different. We will be using the named URLs later in the project so it is important to name each URL in the app. We should also try to keep the names of URLs unique and easy to remember.

Everything all right? Open http://127.0.0.1:8000/ in your browser to see the result.

> TODO: add missing image

By this same procedure you can also add other pages and link them to their own URL. Let's try.

It would be great if your blog had an 'about' section, where people could read more about you and your blog. Just as before, we first need to create a template for our page. Create a new file in `blog/templates` and name it `about.html`. Inside, copy-paste the part of `index.html` that we want to keep (we want the same styling, menu, and title).

```html
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="blog.css">
    </head>
    <body>
        <div class="page-header">
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>
    </body>
</html>
```

Then add some information about yourself and about this blog.

> TODO: Add an example

After that, we need to create a new view.

```python
def about(request):
    return render(request, 'blog/about.html', {})
```

Just like before, this will render our template. What else are we missing? Right, we need to connect this view to an URL.
In your `urlpatterns` you can add a new URL pattern for `about` view by adding `url(r'^about/$', views.about, name='about')`.

So now your `blog/urls.py` should look something like this:

```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.post_list, name='post_list'),
    url(r'^about/$', views.about, name='about'),
]
```

Open http://127.0.0.1:8000/about in your browser to see if it worked.


Great job! You now have your very own website on your computer. Lets share it with the world.

> If you want to know more about Django URLconfs, look at the official documentation: https://docs.djangoproject.com/en/1.8/topics/http/urls/
