# Django templates

Sometimes you want parts of your website to display dynamic data - that is, data that might be different every time someone opens your website. In that case, plain HTML won't be enough. Luckily, Django templates allow us to do a lot more than just write HTML. We can display some Python variables that we defined in our view. Django also gives us some helpful built-in __template tags__ for displaying data.

## What are template tags?

You see, in HTML, you can't really write Python code, because browsers don't understand it. They only know HTML. We know that HTML is rather static, while Python is much more dynamic.

__Django template tags__ allow us to transfer Python-like things into HTML, so you can build dynamic websites faster and easier. Yikes!

> TODO: Before diving into other features of django templates, let's start with more simple stuff. For now, we want to show that it allows us to include some dynamic data. Current date/time is a great example for that.
> 1. Use date.now inside the template - footer for example (to show how to include some dynamic data)
> 2. Check if it works. (it works, but date.now doesn't print the time very nicely)
> 3. Use date template tag (to make it prettier and to show how template tags work)
> 4. Check if it works on /about/ (it doesn't :) ). This is a segway to template inheritance ("look, this is a piece of code we'll need in both places. Why don't we put it in one template, and make that template a parent template")
