# Django templates

Sometimes you want parts of your website to display dynamic data - that is, data that might be different every time someone opens your website. In that case, plain HTML won't be enough. Luckily, Django templates allow us to do a lot more than just write HTML. We can display some Python variables that we defined in our view. Django also gives us some helpful built-in __template tags__ for displaying data.

## What are template tags?

You see, in HTML, you can't really write Python code, because browsers don't understand it. They only know HTML. We know that HTML is rather static, while Python is much more dynamic.

__Django template tags__ allow us to transfer Python-like things into HTML, so you can build dynamic websites faster and easier. Yikes!

> TODO: 1. Use date.now inside the template (footer for example)
> 2. Check if it works.
> 3. Use date template tag
> 4. Check if it works on /about/. Segway to template inheritance
