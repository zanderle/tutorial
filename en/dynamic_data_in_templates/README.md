# Dynamic data in templates

> TODO Add some context - (can probably relate to previous chapters) why are we doing this?
> Update this chapter with content from Django Templates and Template Extending
> A break after this chapter?

We have different pieces in place: the `Post` model is defined in `models.py`, we have `post_list` in `views.py` and the template added. But how will we actually make our posts appear in our HTML template? Because that is what we want to do. Take some content (models saved in the database) and display it nicely in our template, right?

This is exactly what *views* are supposed to do: connect models and templates. In our `post_list` *view* we will need to take the models we want to display and pass them to the template. In a *view* we decide what (model) will be displayed in a template.

OK, so how will we achieve this?

We need to open our `blog/views.py`. So far `post_list` *view* looks like this:

{% filename %}blog/views.py{% endfilename %}
```python
from django.shortcuts import render

def post_list(request):
    return render(request, 'blog/index.html', {})
```

Remember when we talked about including code written in different files? Now is the moment when we have to include the model we have written in `models.py`. We will add the line `from .models import Post` like this:

{% filename %}blog/views.py{% endfilename %}
```python
from django.shortcuts import render
from .models import Post
```

The dot before `models` means *current directory* or *current application*. Both `views.py` and `models.py` are in the same directory. This means we can use `.` and the name of the file (without `.py`). Then we import the name of the model (`Post`).

But what's next? To take actual blog posts from the `Post` model we need something called `QuerySet`.

## QuerySet

You should already be familiar with how QuerySets work. We talked about them in [Django ORM (QuerySets) chapter](../django_orm/README.md).

So now we want published blog posts sorted by `published_date`, right? We already did that in QuerySets chapter!

{% filename %}blog/views.py{% endfilename %}
```python
Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
```

Now we put this piece of code inside the `blog/views.py` file by adding it to the function `def post_list(request)`:

{% filename %}blog/views.py{% endfilename %}
```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/index.html', {})
```

Please note that we create a *variable* for our QuerySet: `posts`. Treat this as the name of our QuerySet. From now on we can refer to it by this name.

Also, the code uses the `timezone.now()` function, so we need to add an import for `timezone`.

The last missing part is passing the `posts` QuerySet to the template. Don't worry – we will cover how to display it in a later chapter.

In the `render` function we have one parameter `request` (everything we receive from the user via the Internet) and another giving the template file (`'blog/index.html'`). The last parameter, `{}`, is a place in which we can add some things for the template to use. We need to give them names (we will stick to `'posts'` right now). :)  It should look like this: `{'posts': posts}`. Please note that the part before `:` is a string; you need to wrap it with quotes: `''`.

So finally our `blog/views.py` file should look like this:

{% filename %}blog/views.py{% endfilename %}
```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/index.html', {'posts': posts})
```

That's it! Time to go back to our template and display this QuerySet!

Want to read a little bit more about QuerySets in Django? You should look here: https://docs.djangoproject.com/en/1.9/ref/models/querysets/

## Display post list template

> TODO: This is moved from Django Templates chapter.

In the previous chapter we gave our template a list of posts in the `posts` variable. Now we will display it in HTML.

To print a variable in Django templates, we use double curly brackets with the variable's name inside, like this:

{% filename %}blog/templates/blog/post_list.html{% endfilename %}
```html
{{ posts }}
```

Try this in your `blog/templates/blog/post_list.html` template. Replace everything from the second `<div>` to the third `</div>` with `{{ posts }}`. Save the file, and refresh the page to see the results:

![Figure 13.1](images/step1.png)

As you can see, all we've got is this:

{% filename %}blog/templates/blog/post_list.html{% endfilename %}
```html
[<Post: My second post>, <Post: My first post>]
```

This means that Django understands it as a list of objects. Remember from __Introduction to Python__ how we can display lists? Yes, with for loops! In a Django template you do them like this:

```html
{% for post in posts %}
    {{ post }}
{% endfor %}
```

Try this in your template.

![Figure 13.2](images/step2.png)

It works! But we want them to be displayed like the static posts we created earlier in the __Introduction to HTML__ chapter. You can mix HTML and template tags. Our `body` will look like this:

```html
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

{% raw %}Everything you put between `{% for %}` and `{% endfor %}` will be repeated for each object in the list. Refresh your page:{% endraw %}

![Figure 13.3](images/step3.png)

Have you noticed that we used a slightly different notation this time (`{{ post.title }}` or `{{ post.text }})`? We are accessing data in each of the fields defined in our `Post` model. Also, the `|linebreaksbr` is piping the posts' text through a filter to convert line-breaks into paragraphs.


