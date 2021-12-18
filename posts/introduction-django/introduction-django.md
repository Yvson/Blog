Django is a web framework written in Python that thinks predominantly in web product delivering with all basic tools, security, and scalability at hand. Django is versatile and can be used in different ways. It can be part of a whole ecosystem or be a complete system on its own as it offers the possibility of coupling and decoupling its own parts most part of the time. Out of the box, Django offers integration with database via its own ORM, route management (URL dispatcher),  functional and class-based views, Django Template Language (DTL), form validation, admin page, internationalization and localization, logging, caching, and many others. Django supports every aspect of any modern web application and keeps evolving through its community. Furthermore, it is free and open source.

## Django's Origin
> Django was invented to meet fast-moving newsroom deadlines, while satisfying the tough requirements of experienced web developers. 

_Source: **[Django Overview]**_

Django comes from a place of tight deadlines, newspaper rooms. According to the [Django Website], the framework was born inside World Online, a newspaper web operation, that is responsible for building intensive web applications on journalism. In 2003, the World Online developers - Adrian Holovaty and Simon Wilison - started switching from PHP to Python in order to develop its websites. In 2005, after a lot of improvements, World Online turned the framework open-source and named it Django after Django Reinhardt, a jazz manouche guitarist from the 1930s to early 1950s.

## What is Django?
> Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. Built by 
> experienced developers, it takes care of much of the hassle of web development, so you can focus on writing your app without
> needing to reinvent the wheel. It’s free and open source. 

_Source: **[Django Project]**_

Django is a powerful web framework that can build simple and complex web applications. It is simple to learn and has a high-level abstraction language since it is built on top of Python. Django is fast, reliable and scalable that can create or be used in projects that require major levels of complexity with regards to data handling, security features, integration services, and etc.

## What does Django offer?
Django is an almighty framework. Its range of operation goes from database integration and data management with its own ORM to template displaying and everything in between. Considering the main layers of the framework, django can be divided in Model, View, and Template. Similarly to MVC architecture, django uses the View layer to control the data flux between requests and responses, and the Template layer handles the presentation of data to the client. The Model layer is responsible to represent the data persisted in the database as a python object and vice versa. Moreover, other functionalities are:

- **Settings file:**
    A Django settings file contains all the configuration of your Django installation. Inside this file all the configuration is set up via constants.
    Below are basic examples that are used in settings.py:

    ```py
    ALLOWED_HOSTS = ['www.yvsonmoura.com']
    DEBUG = False
    DEFAULT_FROM_EMAIL = 'contato@yvsonmoura.com'
    ```

- **Applications:**
    Django separates the project into applications. These apps can be accessed by importing apps registry from "django.apps".
    
    From any place in the project you can call any application. For example:

    ```py
    from django.apps import apps
    apps.get_app_config('admin').verbose_name
    ```

- **Project management:**
    `django-admin` is Django’s command-line utility for administrative tasks. The manage.py file can also be used for the same purpose.
    From the main folder of the project you can use it for various tasks. For example:

    ```sh
    # Basic Syntax
    $ django-admin <command> [options]
    $ python manage.py <command> [options]
    $ python -m django <command> [options]
    
    ## Practical Examples
    # Outputs to standard output all data in the database associated with the named application(s).
    $ django-admin dumpdata [app_label[.ModelName] [app_label[.ModelName] ...]] 
    
    # Creates a Django project directory structure for the given project name in the current directory or the given destination.
    $ django-admin startproject myproject /Users/jezdez/Code/myproject_repo 
    
    # Creates a Django app directory structure for the given app name in the current directory or the given destination.
    $ python manage.py startapp name [directory]
    ```
    
- **Testing:**
    Django’s unit tests use a Python standard library module: unittest. This module defines tests using a class-based approach.
    Using the django.test.TestCase, a unittest.TestCase subclass, you can run tests in your project. For example:

    ```py
    from django.test import TestCase
    from myapp.models import Car
    
    class CarTestCase(TestCase):
        def setUp(self):
            Car.objects.create(name="Cruze", year="2012")
            Car.objects.create(name="Creta", year="2020")
    
        def test_cars_older_than_2015(self):
            """Cars that are older than 2015"""
            cruze = Car.objects.get(name="Cruze")
            creta = Car.objects.get(name="Creta")
            msg = 'The car is NOT older than 2015.'
            self.assertGreater(cruze.year(), 2015, msg)
            self.assertGreater(creta.year(), 2015, msg)
    ```
    After writing the tests, you can run the tests by executing the following code:

    ```sh
    $ python manage.py test
    ```
    
- **Admin Interface:**
    The admin page gives a simple and fast way of interacting with the project models. It is originally configured to handle authentication, permission, django's User Model, Groups, and other possibilities. It can be used in different many ways by the project creator, for example, to set up staff members, to add/modify/delete instances of classes and subclasses of models, to make/print a report, and etc.
    Adding a new model to the admin interface is simple. For example:

    ```py
    from django.contrib import admin
    from .models import Post
    
    @admin.register(Post)
    class PostAdmin(admin.ModelAdmin):
        list_display = ['title', 'author', 'created_at', 'updated_at', 'updated_at', 'abstract', 'body'] # This line adds the fields of the model to the admin interface.
    ```

- **Security:**
    By default, django has mechanisms to prevent the most common attacks used on the web. They are:
        
    - **Cross site scripting (XSS) Protection:**
        In order to inhibit the execution of any script injected into the database, django, by default, takes some measures to protect the user from running dangerous code. For example, the output of every variable tag is automatically escaped in a django generated template. The following characters are escaped:

        ```html
        < is converted to &lt;
        > is converted to &gt;
        ' (single quote) is converted to &#x27;
        " (double quote) is converted to &quot;
        & is converted to &amp;
        ```

        To disable this default behavior, you can apply a filter (for variables) or a block tag (for template blocks).
        Example for variables:

        ```django
        This will be escaped: {{ variable }}
        This will not be escaped: {{ variable|safe }}
        ```
        Example for template blocks:

        ```django
        {% autoescape off %}
            Hello {{ username }}
        {% endautoescape %}
        ```
        
    - **Cross site request forgery (CSRF) Protection:**
        To prevent any malicious attacker from making a request with the user's credentials, changing any settings or taking actions to harm the user, django uses, by default, a middleware (`CsrfViewMiddleware`) to add a CSRF token to the cookie and checks this token when the client tries to use an 'unsafe' method (POST, PUT or DELETE). Therefore, if the template language is being used in the project, the csrf_token tag must be added to the form tag. For example:

        ```django
        <form method="post">
            {% csrf_token %}
        </form>
        ```
        
    - **SQL Injection Protection:**
        SQL Injection is an attack done by a user that is able to execute arbitrary SQL code on a database. This code execution can create/read/update/delete any data in the database. It is certainly one of the most dangerous attack to be suffered as it compromises all the system. By default, django makes its querysets injection-proof by query parameterization. According to Django website: 
        
        > Django’s querysets are protected from SQL injection since their queries are constructed using query parameterization. A query’s SQL code is defined separately from the query’s parameters. Since parameters may be user-provided and therefore unsafe, they are escaped by the underlying database driver.
        
        _Source: [Django SQL Injection]_
        
        It is possible to make raw queries through a model manager method called `raw`. Using the below code as an example, we have:
            
        ```py
        class Employee(models.Model):
            first_name = models.CharField(...)
            last_name = models.CharField(...)
            hiring_date = models.DateField(...)
        ```
        Raw query:
        ```py
        Employee.objects.raw('SELECT * FROM myapp_employee')
        ```
        
    - **Clickjacking Protection:**
        Clickjacking is a type of attack in which the user is tricked to take an action without knowing the real consequence of it, for the attacker deceives the user with an element inserted into the site by using a hidden frame or iframe.
        
        > Modern browsers honor the X-Frame-Options HTTP header that indicates whether or not a resource is allowed to load within a frame or iframe. If the response contains the header with a value of SAMEORIGIN then the browser will only load the resource in a frame if the request originated from the same site. If the header is set to DENY then the browser will block the resource from loading in a frame no matter which site made the request.
            
        > Dango provides a few ways to include this header in responses from your site:
            1. A middleware that sets the header in all responses.
            2. A set of view decorators that can be used to override the middleware or to only set the header for certain views.
            
        > The X-Frame-Options HTTP header will only be set by the middleware or view decorators if it is not already present in the response. 
        
        _Source: [Django Clickjacking]_
        
    - **SSL/HTTPS:**
        Django provides some options of settings related to SSL/HTTPS protocol connections. They are found in `SecurityMiddleware` and can assist the web server with more options, bringing more control to how clients must connect to the django application. Some of them are:
        
        - SECURE_CONTENT_TYPE_NOSNIFF
        - SECURE_CROSS_ORIGIN_OPENER_POLICY
        - SECURE_HSTS_INCLUDE_SUBDOMAINS
        - SECURE_HSTS_PRELOAD
        - SECURE_HSTS_SECONDS
        - SECURE_REDIRECT_EXEMPT
        - SECURE_REFERRER_POLICY
        - SECURE_SSL_HOST
        - SECURE_SSL_REDIRECT
        
        More in [Django SSL/HTTPS].
        
    - **Host Header Validation:**
        > Django uses the Host header provided by the client to construct URLs in certain cases. While these values are sanitized to prevent Cross Site Scripting attacks, a fake Host value can be used for Cross-Site Request Forgery, cache poisoning attacks, and poisoning links in emails.
        
        _Source: [Django Host Header Validation]_
        
        Django has its own way of ensuring that even if the web server is deceived about the host header, the django application will validate the host header against the ALLOWED_HOSTS setting via `django.http.HttpRequest.get_host()` method.
        
    - **Referrer Policy:**
        Django framework facilitates the referrer policy change through `SecurityMiddleware` middleware and `SECURE_REFERRER_POLICY` setting. The options are:

        > **no-referrer:** Instructs the browser to send no referrer for links clicked on this site.
        
        > **no-referrer-when-downgrade:** Instructs the browser to send a full URL as the referrer, but only when no protocol downgrade occurs.
        
        > **origin:** Instructs the browser to send only the origin, not the full URL, as the referrer.
        
        > **origin-when-cross-origin:** Instructs the browser to send the full URL as the referrer for same-origin links, and only the origin for cross-origin links.
        
        > **same-origin:** Instructs the browser to send a full URL, but only for same-origin links. No referrer will be sent for cross-origin links.
        
        > **strict-origin:** Instructs the browser to send only the origin, not the full URL, and to send no referrer when a protocol downgrade occurs.
        
        > **strict-origin-when-cross-origin:** Instructs the browser to send the full URL when the link is same-origin and no protocol downgrade occurs; send only the origin when the link is cross-origin and no protocol downgrade occurs; and no referrer when a protocol downgrade occurs.

        > **unsafe-url:** Instructs the browser to always send the full URL as the referrer.
        
        _Source: [Django Referrer Policy]_
        
    - **Session Security:**
    By default, Django stores sessions in the database with the model `django.contrib.sessions.models.Session`. There is also the possibility of storing session data on the filesystem or in the cache. The session configuration can be tweaked using the following settings:

        - SESSION_CACHE_ALIAS
        - SESSION_COOKIE_AGE
        - SESSION_COOKIE_DOMAIN
        - SESSION_COOKIE_HTTPONLY
        - SESSION_COOKIE_NAME
        - SESSION_COOKIE_PATH
        - SESSION_COOKIE_SAMESITE
        - SESSION_COOKIE_SECURE
        - SESSION_ENGINE
        - SESSION_EXPIRE_AT_BROWSER_CLOSE
        - SESSION_FILE_PATH
        - SESSION_SAVE_EVERY_REQUEST
        - SESSION_SERIALIZER

- **Internationalization and Localization:**
    Django has full support for translation of text, formatting of dates, times and numbers, and time zones.

    > The words “internationalization” and “localization” often cause confusion; here’s a simplified definition:
    
    > **Internationalization:** Preparing the software for localization. Usually done by developers.
    
    > **Localization:** Writing the translations and local formats. Usually done by translators.
    
    _Source: [Django Internationalization and Localization]_
    
    Through hooks added to the python code and templates, django can track the sentences that should be considered for translation. After that, django extracts the translation strings into a message file. This file is used by translators to organize and translate all sentences to the respective languages. And then, this file is compiled. All this process relies pm the GNU gettext toolset.
    Django’s internationalization hooks are on by default. If the project does not require internationalization, the USE_I18N setting must be equal to False. Moreover, to activate the translation for a project, the middleware `LocaleMiddleware` have to be included to the MIDDLEWARE setting.

- **Performance and optimization:**
    Measuring the performance of the code project is a key point to adjust and optimize it. Django indicates tools and third-party services to help the developer in this matter:

    > **Django tools**
    > **[django-debug-toolbar]** is a very handy tool that provides insights into what your code is doing and how much time it spends doing it. In particular it can show you all the SQL queries your page is generating, and how long each one has taken.
    Third-party panels are also available for the toolbar, that can (for example) report on cache performance and template rendering times.
    
    > **Third-party services**
    > There are a number of free services that will analyze and report on the performance of your site’s pages from the perspective of a remote HTTP client, in effect simulating the experience of an actual user.

    > These can’t report on the internals of your code, but can provide a useful insight into your site’s overall performance, including aspects that can’t be adequately measured from within Django environment. Examples include:
    
    > **Yahoo’s Yslow**
    
    > **Google PageSpeed**
    
    > There are also several paid-for services that perform a similar analysis, including some that are Django-aware and can integrate with your codebase to profile its performance far more comprehensively.
    
    _Source: **[Django Performance and Optimization]**_

- **Geographic framework:**
    GeoDjango availability is found in the contrib module of django and it helps to adapt the framework to geographic data type. According to the documentation:
    
    > GeoDjango strives to make it as simple as possible to create geographic web applications, like location-based services. Its features include:
    > **1. Django model fields for OGC geometries and raster data.**
    > **2. Extensions to django’s ORM for querying and manipulating spatial data.**
    > **3. Loosely-coupled, high-level Python interfaces for GIS geometry and raster operations and data manipulation in different formats.**
    > **4. Editing geometry fields from the admin.**
    
    _Source: **[Django GeoDjango]**_


- **Common Web Application tools:**
    - [Authentication]
    - [Caching]
    - [Logging]
    - [Sending emails]
    - [Syndication feeds]
    - [Pagination]
    - [Messages framework]
    - [Serialization]
    - [Sessions]
    - [Sitemaps]
    - [Static files management]
    - [Data validation]


- **Other core functionalities:**
    - [Conditional Content Processing]
    - [Content Types and Generic Relations]
    - [Flatpages]
    - [Redirects]
    - [Signals]
    - [System Check Framework]
    - [The sites framework]
    - [Unicode in Django]

More in [Django Documentation].

## What is Django good for?
Django is versitle and can fit projects of different nature. To have an ideia about what kinds of projects are using django nowadays, there is a list below:

- [Disqus]
- [Instagram]
- [Knight Foundation]
- [MacArthur Foundation]
- [Mozilla]
- [National Geographic]
- [Open Knowledge Foundation]
- [Pinterest]
- [Open Stack]

Before starting coding a new project, you might ask yourself about what is going to be needed to get the job done and how this goal will be achieved. Django's built-in features ([Django Documentation]) and all third-party packages ([Awesome Django]) developed by the community cover pretty much everything you will need to build your project from scratch. Django can easily build the most common projects ideas. For example:

- E-commerce Platform
- Content Management System
- Video Streaming Platform
- Photo Sharing Network
- Social Media Network
- Fintech Platform
- Institutional Website

Once the project idea for a MVP (Minimum Viable Product) is established, it is time to define the Django responsibility in the whole system. The scenarios could be: 

1. **Total Responsability:**
    - Django serves the client from Front-end to Back-end through its Web Server Gateway Interface (WSGI) or Asynchronous Server Gateway Interface (ASGI). The original architecture of django (MTV) is enforced to the project since the django web app handles everything. This is the traditional approach for building a django Project.
    
        | *Figure 1 - Full Django System.* |
        | :--: |
        | ![total-responsability](https://raw.githubusercontent.com/Yvson/blog/main/posts/introduction-django/total_responsability_django.png) |
        | *All project is based on Django ecosystem.* |


2. **Partial/Hybrid Responsability:**
    - In this scenario, Django WSGI still serves the web server and controls how pages are displayed on the screen. However, javascript files have an indispensable role in this setup. They build the elements in the DOM tree together with the Django Template Language. This approach brings more flexibility to the project, but it can make the maintenance of the code harder due to lack of structure and organization.

        | *Figure 2 - Django serving Static JS Files.* |
        | :--: |
        | ![partial-responsability](https://raw.githubusercontent.com/Yvson/blog/main/posts/introduction-django/partial_responsability_django.png) |
        | *The project is served by Django, but the Front-end is built with Javascript and Django Template Language.* |

3. **Minimum Responsability:**
    - In this case, django is exclusively responsible for handling the back-end and usually does it by implementing an API with REST architecture or GraphQL. The front-end is supplied to the user through static files (Figure 3) or Node.js server (Figure 4) via a web server.

        | *Figure 3 - Django + Static JS Files.* |
        | :--: |
        | ![minimum-responsability-1](https://raw.githubusercontent.com/Yvson/blog/main/posts/introduction-django/minimum_responsability_django_1.png) |
        | *The Front-end is served by the web server through static files and the Back-end is served by Django.* |

        | *Figure 4 - Django + Node.js.* |
        | :--: |
        | ![minimum-responsability-2](https://raw.githubusercontent.com/Yvson/blog/main/posts/introduction-django/minimum_responsability_django_2.png) |
        | *The Front-end is served by a Node.js server and the Back-end is served by Django.* |


The way of configuring the project can delimit the role of django and this can impact strongly how much of the framework functionality and tools will be available in practice. Furthermore, the more tools are present, the more complicated and demanding is to keep the integration among the system parts working properly.

## Setting up a Python Environment and Installing Django
The first step to initiate a project is creating a python environment and installing the django package. If the python language is installed, open a terminal and type:

```sh
# Creating a virtual environment with python (3.10) in a Linux system
$ python -m venv django_project

# Activating the python environment
$ source django_project/bin/activate

# Installing django package
(django_project) $ pip install django
```

## Starting a Django Project
After installing the django package, the command `django-admin startproject <project_folder_name>` creates a django project. Example below:

```sh
# Starting a project with Django
(django_project) $ django-admin startproject my_project
```

## Starting an App
Django, by default, separates the different parts of the project in structures called "apps". So, from the command line, type:

```sh
# Starting an App with Django
(django_project) $ django-admin startapp django_app
```
**OR**

```sh
# Starting an App with Django
(django_project) $ python manage.py startapp django_app
```

## Running the development server
In order to develop the project, django offers a built-in development server. It can be turned on by using the `runserver` argument. For example:

```sh
(django_project) $ python manage.py runserver
```

**OR WITH A SPECIFIC IP ADDRESS AND PORT**

```sh
(django_project) $ python manage.py runserver 0.0.0.0:8000
```

Now, the django welcome page can be accessed via browser in localhost:8000.

## Conclusion
This post is a brief summary of the django framework and how it fits in a project. The framework is mature and has evolved with the help of the community in the last 16 years since it is open source. It is reliable, safe, scalable, and flexible. It gives all the tools a developer needs to build basically any system on the web. Moreover, the community contributes immensely to the core package and third-party packages. Lately, the [version 4.0] of django has been released and more changes are yet to come. Even though, modern web has changed abruptly in the later years, django keeps its place as [one of the most popular back-end frameworks] currently.



  [Django Overview]: <https://www.djangoproject.com/start/overview>
  [Django Website]: <https://docs.djangoproject.com/en/4.0/faq/general/#why-does-this-project-exist>
  [Django Project]: <https://www.djangoproject.com/>
  [Django Documentation]: <https://docs.djangoproject.com/>
  [Django SQL Injection]: <https://docs.djangoproject.com/en/4.0/topics/security/#sql-injection-protection>
  [Django Clickjacking]: <https://docs.djangoproject.com/en/4.0/ref/clickjacking/#preventing-clickjacking>
  [Django SSL/HTTPS]: <https://docs.djangoproject.com/en/4.0/topics/security/#ssl-https>
  [Django Host Header Validation]: <https://docs.djangoproject.com/en/4.0/topics/security/#host-header-validation>
  [Django Referrer Policy]: <https://docs.djangoproject.com/en/4.0/ref/middleware/#referrer-policy>
  [Django Internationalization and Localization]: <https://docs.djangoproject.com/en/4.0/topics/i18n/>
  [Django Performance and Optimization]: <https://docs.djangoproject.com/en/4.0/topics/performance/>
  [django-debug-toolbar]: <https://django-debug-toolbar.readthedocs.io/en/latest/>
  [Django GeoDjango]: <https://docs.djangoproject.com/en/4.0/ref/contrib/gis/tutorial/#introduction>
  [Authentication]: <https://docs.djangoproject.com/en/4.0/topics/auth/>
  [Caching]: <https://docs.djangoproject.com/en/4.0/topics/cache/>
  [Logging]: <https://docs.djangoproject.com/en/4.0/topics/logging/>
  [Sending emails]: <https://docs.djangoproject.com/en/4.0/topics/email/>
  [Syndication feeds]: <https://docs.djangoproject.com/en/4.0/ref/contrib/syndication/>
  [Pagination]: <https://docs.djangoproject.com/en/4.0/topics/pagination/>
  [Messages framework]: <https://docs.djangoproject.com/en/4.0/ref/contrib/messages/>
  [Serialization]: <https://docs.djangoproject.com/en/4.0/topics/serialization/>
  [Sessions]: <https://docs.djangoproject.com/en/4.0/topics/http/sessions/>
  [Sitemaps]: <https://docs.djangoproject.com/en/4.0/ref/contrib/sitemaps/>
  [Static files management]: <https://docs.djangoproject.com/en/4.0/ref/contrib/staticfiles/>
  [Data validation]: <https://docs.djangoproject.com/en/4.0/ref/validators/>
  [Conditional Content Processing]: <https://docs.djangoproject.com/en/4.0/topics/conditional-view-processing/>
  [Content Types and Generic Relations]: <https://docs.djangoproject.com/en/4.0/ref/contrib/contenttypes/>
  [Flatpages]: <https://docs.djangoproject.com/en/4.0/ref/contrib/flatpages/>
  [Redirects]: <https://docs.djangoproject.com/en/4.0/ref/contrib/redirects/>
  [Signals]: <https://docs.djangoproject.com/en/4.0/topics/signals/>
  [System Check Framework]: <https://docs.djangoproject.com/en/4.0/topics/checks/>
  [The sites framework]: <https://docs.djangoproject.com/en/4.0/ref/contrib/sites/>
  [Unicode in Django]: <https://docs.djangoproject.com/en/4.0/ref/unicode/>
  [Disqus]: <https://disqus.com/>
  [Instagram]: <https://www.instagram.com/>
  [Knight Foundation]: <https://knightfoundation.org/>
  [MacArthur Foundation]: <https://www.macfound.org/>
  [Mozilla]: <https://www.mozilla.org/en-US/>
  [National Geographic]: <https://www.nationalgeographic.com/>
  [Open Knowledge Foundation]: <https://okfn.org/>
  [Pinterest]: <https://www.pinterest.com/>
  [Open Stack]: <https://www.openstack.org/>
  [Awesome Django]: <https://awesomedjango.org/>
  [version 4.0]: <https://docs.djangoproject.com/en/4.0/releases/4.0/>
  [one of the most popular back-end frameworks]: <https://statisticsanddata.org/data/most-popular-backend-frameworks-2012-2021/>