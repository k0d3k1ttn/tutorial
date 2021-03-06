# Django Views #

## What is a View? ##

- What you actually **see** when you visit a webpage
- Can be written in any of a combination of several things including html, javascript, CSS, etc.
- Can be static (always the same) or dynamic (changes based on other stuff, like what's in the database, or what you clicked on, or what time it is, etc.)

## From URL to Your View ##
- User clicks on a link to your site
- Their computer makes a request for the web page
- The site (`critters_site`) forwards the request to the application (`cat_shelter`)
- The application matches the request against (rule, function) pairs
- If a rule matches - call the first matching rule's function
- Otherwise, throw an error

## Regex Notes ##

- What's a Regex?  
    A Regular Expression - matches strings (like URLs!) against patterns.
- Common regex symbols:
  - `^` : the beginning
  - `$` : the end
  - `\d` : a digit
  - `+` : repeat one more times
  - `*` : repeat zero or more times
  - `?` : repeat zero or one times

## Next Steps ##

### Connect your application to your site ###
Why? So that when certain requests hit your site (`critters_site`), they are sent to your application (`cat_shelter`) for handling.

How?  Add a reference to your **application** URLs in your **site** URLs

- Open `critters_site/urls.py`
- Add a URL entry pointing to your cat_shelter.urls.  Use `r` to specify when the site should use that app.
  - To forward **all** requests to your app, use:
    - `url(r'', include('cat_shelter.urls')),`
    - "If I get any request, look for instructions in the `cat_shelter` app."
- To forward all requests starting with with `kitties` to the `cat_shelter` app, use:
    - `url(r'^kitties/', include('cat_shelter.urls')),`
    - "If I get a request like `http://127.0.0.1:8000/kitties/...`, look for instructions in the `cat_shelter` app."

*Example*

    from django.conf.urls import include, url
    from django.contrib import admin
    
    urlpatterns = [
      url(r'^admin/', include(admin.site.urls)),
      url(r'', include('cat_shelter.urls')),
    ]
    
### Add application-specific instructions into your **application** URLs ###

Why? Our site `critters_site` forwards requests to our application `cat_shelter`.  We want to point to code that actually handles the requests.

How? 
  - Create a file `cat_shelter/urls.py`
  - At the very top put the following two lines
    - `from django.conf.urls import include, url`
    - `from . import views`
  - Add a `url_patterns` block like the one in `critters_site/urls.py`, with a URL entry for your view.  Use `r` to specify when the application should use that view.
    - To use the `current_cats` view for empty strings--when you specify no path in the request, like `http://127.0.0.1:8000/` - use:
      - `url(r'^$', views.current_cats),`
      - `^` matches the start of a string and `$` matches the end, so `^$` matches a string with nothing between beginning and end
    - To use the `current_cats` view for requests like `http://127.0.0.1:8000/current`, use:
       `url(r'^current$', views.current_cats),`

*Example*

    from django.conf.urls import include, url
    from . import views
    
    urlpatterns = [
        url(r'^$', views.current_cats),
    ]

### Create a function to call your view ###
Why?
    When a matching request arrives at our handler, we need to actually specify what to do!

How?
  - Open `cat_shelter/views.py`
    - Add a function corresponding to your view (such as `current_cats`)
    - This function basically just presents the HTML in the file you specify.

*Example*

    def current_cats(request):
    
      return render(request, 'cat_shelter/current_cats.html', {})
      
### Create the view your handler refers to ###
Why?  We told our handler to render an HTML page... now we need to put some HTML in it!

How?
  - Create `cat_shelter/templates` and `cat_shelter/templates/cat_shelter` directories. 
  - Note: Django convention is to create a directory inside the `templates` directory for your app (even though your templates directory is already inside a directory for your app.  Whatever.)
  - For the example above, you will create a file named `current_cats.html` in the `cat_shelter/templates/cat_shelter` directory.  You can leave it blank or [Add HTML](../html/README.md)
  - You'll need to restart your django server to see this addition.  Hit Control-C, up-arrow, enter!
