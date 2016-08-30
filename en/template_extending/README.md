# Template extending

Another nice thing Django has for you is __template extending__. What does this mean? It means that you can use the same parts of your HTML for different pages of your website.

Templates help when you want to use the same information/layout in more than one place.  You don't have to repeat yourself in every file. And if you want to change something, you don't have to do it in every template, just once!

Remember how we've had to copy all our changes on both the `index.html` and the `about.html` templates? We're now going to learn how Django's template extending can help us avoid that duplication.

## Create base template

A base template is the most basic template that you extend on every page of your website. It contains all the content that should appear in every page no matter what (for example: your site logo, footer information, main navigation menu).

Let's create a `base.html` file in `blog/templates/blog/`:

    blog
    └───templates
        └───blog
                about.html
                base.html
                index.html

Then open it up and copy everything from `index.html` to `base.html` file, like this:

{% filename %}blog/templates/blog/base.html{% endfilename %}
```html
{% load staticfiles %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href='//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext' rel='stylesheet' type='text/css'>
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                    <div class="post">
                        <p>published: 14.06.2014, 12:14</p>
                        <h2><a href="">My first post</a></h2>
                        <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
                    </div>

                    <div class="post">
                        <p>published: 14.06.2014, 12:14</p>
                        <h2><a href="">My second post</a></h2>
                        <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
                    </div>
                </div>
            </div>
        </div>
    </body>
</html>
```

Then in `base.html`, replace your whole `<body>` (everything between `<body>` and `</body>`) with this:

{% filename %}blog/templates/blog/base.html{% endfilename %}
```html
<body>
    <div class="page-header">
        <h1><a href="/">Django Girls Blog</a></h1>
    </div>
    <div class="content container">
        <div class="row">
            <div class="col-md-8">
            {% block content %}
            {% endblock %}
            </div>
        </div>
    </div>
</body>
```

You might notice this replaced all blog post content (both `<div class="post">` and everything inside them) with:

```html
{% block content %}
{% endblock %}
```
But why?  You just created a `block`!  You used the template tag `{% block %}` to make an area that will have HTML inserted in it. That HTML will come from another templates that extends this template (`base.html`). We will show you how to do this in a moment.

Now save `base.html`, and open your `blog/templates/blog/index.html` again.
You're going to remove everything except the two `<div class="post">` and everything inside them. When you're done the file will look like this:

{% filename %}blog/templates/blog/index.html{% endfilename %}
```html
<div class="post">
    <p>published: 14.06.2014, 12:14</p>
    <h2><a href="">My first post</a></h2>
    <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
</div>

<div class="post">
    <p>published: 14.06.2014, 12:14</p>
    <h2><a href="">My second post</a></h2>
    <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
</div>
```

We want to use this as part of our template for all the content blocks.
Time to add block tags to this file!

{% raw %}You want your block tag to match the tag in your `base.html` file. You also want it to include all the code that belongs in your content blocks. To do that, put everything between `{% block content %}` and `{% endblock content %}`. Like this: {% endraw %}

{% filename %}blog/templates/blog/index.html{% endfilename %}
```html
{% block content %}
    <div class="post">
        <p>published: 14.06.2014, 12:14</p>
        <h2><a href="">My first post</a></h2>
        <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
    </div>

    <div class="post">
        <p>published: 14.06.2014, 12:14</p>
        <h2><a href="">My second post</a></h2>
        <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
    </div>
{% endblock %}
```

Only one thing left. We need to connect these two templates together.  This is what extending templates is all about!  We'll do this by adding an extends tag to the beginning of the file. Like this:

{% filename %}blog/templates/blog/index.html{% endfilename %}
```html
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        <p>published: 14.06.2014, 12:14</p>
        <h2><a href="">My first post</a></h2>
        <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
    </div>

    <div class="post">
        <p>published: 14.06.2014, 12:14</p>
        <h2><a href="">My second post</a></h2>
        <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
    </div>
{% endblock %}
```

That's it! Check if your website is still working properly :)

> If you have an error `TemplateDoesNotExist` that says that there is no `blog/base.html` file and you have `runserver` running in the console, try to stop it (by pressing Ctrl+C - Control and C buttons together) and restart it by running a `python manage.py runserver` command.

{% raw %}And finally, we want to repeat the same steps in the `about.html` template. We will extend `base.html` and include your personal details inside the `{% block content %}` tag. The template should now look like this:{% endraw %}

{% filename %}blog/templates/blog/about.html{% endfilename %}
```html
{% extends 'blog/base.html' %}

{% block content %}
    <h2>Hi! My name is [your name] and I made this blog</h2>
    <p>Here is a little bit more about me [...]</p>
{% endblock %}
```

Save your changes and visit http://127.0.0.1:8000/about/ in your browser. Everything should look like it did before, but now all our templates contain no repeated code! All common elements (like the "Django Girls Blog" title and all the contents inside `<head>`) are part of `base.html`, but the parts that are different for each page are stored in `index.html` and `about.html`.


## One more thing

It'd be good to see if your website will still be working on the public Internet, right? Let's try deploying to PythonAnywhere again. Here's a recap of the steps...

* First, push your code to Github

```
$ git status
[...]
$ git add -all .
$ git status
[...]
$ git commit -m "Modified templates to display posts from database."
[...]
$ git push
```

* Then, log back in to [PythonAnywhere](https://www.pythonanywhere.com/consoles/) and go to your **Bash console** (or start a new one), and run:

```
$ cd my-first-blog
$ git pull
[...]
```

* Finally, hop on over to the [Web tab](https://www.pythonanywhere.com/web_app_setup/) and hit **Reload** on your web app. Your update should be live!

Works like a charm? We're proud! Step away from your computer for a bit, you have earned a break. :)

![Figure 13.4](images/donut.png)
