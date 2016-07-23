# Django templates

Sometimes you want parts of your website to display dynamic data - that is, data that might be different every time someone opens your website. In that case, plain HTML won't be enough. Luckily, Django templates allow us to do a lot more than just write HTML. We can display some Python variables that we defined in our view. Django also gives us some helpful built-in __template tags__ for displaying data.

## What are template tags?

You see, in HTML, you can't really write Python code, because browsers don't understand it. They only know HTML. We know that HTML is rather static, while Python is much more dynamic.

__Django template tags__ allow us to transfer Python-like things into HTML, so you can build dynamic websites faster and easier. Yikes!

## Display the current date and time

Let's say we want to show the current date when a visitor loads our site. Django provides a __template tag__ just for that: `{% now %}`. You can insert the `{% now %}` tag anywhere in your template and the current date will be displayed when you visit the page in the browser. This is how it looks like when you use it in your template alongside regular HTML:

```html
<p>Hello! It is {% now "SHORT_DATE_FORMAT" %}</p>
```

For now let's create a new line and add it right before the closing `</body>` tag in our `index.html` template. The bottom of the file should look like this:

{% filename %}blog/templates/blog/index.html{% endfilename %}
```html
        <p>Hello! It is {% now "SHORT_DATE_FORMAT" %}</p>
    </body>
</html>
```

Save your changes and open http://127.0.0.1:8000/ in your browser to see if it worked. You should see something like `Hello! It is 22/07/2016` at the bottom of the page. Take a moment to think about what we just did: we are showing the current date without actually having to type it in! By using the `{% now %}` tag we're telling Django to figure out what's the current date and then insert that in the HTML that the browser reads. Django will show the correct date today, tomorrow, and forever, because it will use Python to check the calendar whenever a user asks to see the page.

You may have noticed that we typed `"SHORT_DATE_FORMAT"` inside the `{% now %}` tag. Just like Python functions, template tags accept arguments. The arguments you pass to a template tag modify its behavior. In this case, we're telling the `{% now %}` tag to display the current date in a short format (that's why you see "22/07/2016" instead of something longer like "Friday, July 22, 2016").

Let's play with the arguments of the `{% now %}` tag. Replace `"SHORT_DATE_FORMAT"` with `"SHORT_DATETIME_FORMAT"` (don't forget the quotes). Reload the page and you should see something like `Hello! It is 07/22/2016 3:39 p.m.`. Did you notice the difference? Django has also included the current time after the date. If you wait for one minute and reload the page, you should see that the time displayed in your page changes as the minutes pass.

The `{% now %}` tag is just one of many that are built into Django, and you can even create your own! They can do all sort of things to spice up your static HTML with dynamic Python behavior. You will learn more about Django template tags in later chapters.

## Display the date and time in the About page

Open http://127.0.0.1:8000/about/ in your browser and you will see that the date and time are not displayed there. This is because you modified the `blog/templates/blog/index.html` template, and the `/about/` URL uses the `blog/templates/blog/about.html` template. To fix this, copy the piece of code you just added in `index.html` and paste it in `about.html`. Now save your changes and reload the page in your browser and the time and date should appear in the About page as they did in the previous page.

Copying and pasting code between templates is fine when you only have a few of them. However, it can be very annoying and error-prone if you have to do it a lot of times as the number of pages grows. Can you imagine having to do that when your site has 5, 10, or 50 pages? That would be no fun! We will see how Django can help us reduce the repetition in the next chapter.
