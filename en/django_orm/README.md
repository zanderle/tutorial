# Django ORM and QuerySets

> TODO: this chapter is missing context - explain why we want to access and filter

In this chapter you'll learn how Django connects to the database and stores data in it. Let's dive in!


## What is a QuerySet?

A QuerySet is, in essence, a list of objects of a given Model. QuerySet allows you to read the data from the database, filter it and order it.

It's easiest to learn by example. Let's try this, shall we?


## Django shell

Open up your local console (not on PythonAnywhere) and type this command:

    (myvenv) ~/djangogirls$ python manage.py shell

The effect should be like this:

    (InteractiveConsole)
    >>>

You're now in Django's interactive console. It's just like Python prompt but with some additional Django magic :). You can use all the Python commands here too, of course.


### All objects

Let's try to display all of our posts first. You can do that with the following command:

    >>> Post.objects.all()
    Traceback (most recent call last):
          File "<console>", line 1, in <module>
    NameError: name 'Post' is not defined

Oops! An error showed up. It tells us that there is no Post. It's correct -- we forgot to import it first!

    >>> from blog.models import Post

This is simple: we import model `Post` from `blog.models`. Let's try displaying all posts again:

    >>> Post.objects.all()
    [<Post: my post title>, <Post: another post title>]

It's a list of the posts we created earlier! We created these posts using the Django admin interface. But, now we want to create new posts using Python, so how do we do that?


### Filter objects

A big part of QuerySets is an ability to filter them. Let's say, we want to find all posts User ola authored. We will use `filter` instead of `all` in `Post.objects.all()`. In parentheses we will state what condition(s) a blog post needs to meet to end up in our queryset. In our situation it is `author` that is equal to `me`. The way to write it in Django is: `author=me`. Now our piece of code looks like this:

    >>> Post.objects.filter(author=me)
    [<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]

Or maybe we want to see all the posts that contain a word 'title' in the `title` field?

    >>> Post.objects.filter(title__contains='title')
    [<Post: Sample title>, <Post: 4th title of post>]

> **Note** There are two underscore characters (`_`) between `title` and `contains`. Django's ORM uses this rule to separate field names ("title") and operations or filters ("contains"). If you only use one underscore, you'll get an error like "FieldError: Cannot resolve keyword title_contains".

You can also get a list of all published posts. We do it by filtering all the posts that have `published_date` set in the past:
    >>> from django.utils import timezone
    >>> Post.objects.filter(published_date__lte=timezone.now())
    []

Unfortunately, the post we added from the Python console is not published yet. We can change that! First get an instance of a post we want to publish:

    >>> post = Post.objects.get(title="Sample title")

> TODO: move the definition of `publish` method from Django Models chapters to here.

And then publish it with our `publish` method!

    >>> post.publish()

Now try to get list of published posts again (press the up arrow button 3 times and hit `enter`):

    >>> Post.objects.filter(published_date__lte=timezone.now())
    [<Post: Sample title>]


### Ordering objects

QuerySets also allow you to order the list of objects. Let's try to order them by `created_date` field:

    >>> Post.objects.order_by('created_date')
    [<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]

We can also reverse the ordering by adding `-` at the beginning:

    >>> Post.objects.order_by('-created_date')
    [<Post: 4th title of post>,  <Post: My 3rd post!>, <Post: Post number 2>, <Post: Sample title>]


### Chaining QuerySets 

You can also combine QuerySets by **chaining** them together:

    >>> Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')

This is really powerful and lets you write quite complex queries.

Cool! You're now ready for the next part! To close the shell, type this:

    >>> exit()
    $
