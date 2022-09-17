# Django Authentication

# How to do django login logout password change and passwprd reset?

In every web application, you need authentication for your project. Here we will walk through how to do it simply and quickly in your project. So let us do it now...

I assume you guys know the basic knowledge of Python and Django. If you are not familiar with python programming and the Django web framework, please watch my Python Tutorial for Beginner and Django Basic to Advanced Tutorial 

#Setting up the environment

To check whether you have Python installed or not on your device, just open the command prompt on your device and write

    python -- version

It will show you what version to install on your device. It will message that Python is not installed on your device if not installed. Do not worry if it is not installed. Just go to the Python official documentation from there you can download the latest version or specific version on your device.

I assume you have Python installed. Next, we will create a directory, and inside the directory, we will do our project. To create a directory on a windows device, write on the command prompt

      mkdir nameOfTheDirectory

Then go to that directory. To go to that directory write 

      cd nameOfTheDirectory

Once the virtual machine installs, we will create a virtual machine. To create or activate a virtual machine, just write 

    python -m venv nameOfTheVirtualMachine

Once the virtual machine installs, we will create a virtual machine. To create or activate a virtual machine, just write 
  
    nameOfTheVirtualMachine\Scripts\activate

if the need arises to deactivate the virtual machine, just write 
  
    nameOfTheVirtualMachine\Scripts\deactivate.bat

Once we activate the virtual machine next, we will install Django, and all of the installations will be installed inside the virtual machine. The benefit of using the virtual machine is that it will not clash with the global packages which are already preinstalled on your device. To install Django, just write the command -
  
    pip install django

Once the Django install is finished, then we are almost ready to start our project, and we can use the Django management commands. To start any Django project, write the command in the command prompt.
  
    django-admin startproject nameOfTheProject

Now we can start to create a super user, then do the migration and migrate to create the database and run the server, but also we can create the app. it is the choice of the developer; whichever options you prefer, we can choose that direction. This tutorial will use a third-party package to design our login logout HTML templates. To install the crispy_forms package, we have to install it. To install the crispy_form, just write the command
  
    pip install django-crispy-forms

As we have to install Django-crispy-forms and create the app, we now have to inform Django through the settings files. Now go to the setting file and open it and write where it says INSTALLED_APPS 
inside there, add those Django-crispy-forms and the name of the app.

    INSTALLED_APPS = [
        'crispy_forms', 
    ]

As we are using the crispy_forms, we have to use bootstrap, so now we have to add the bootstrap.

Write inside the setting files.

    CRISPY_TEMPLATE_PACK = 'bootstrap4' # we can use different version of the bootstrap.

Now, we will create our app application. In Django, one thing to remember is that the Django project is one thing, and the app is another. In one Django project, you can have multiple apps. Let us create our app inside the project name is accounts. You can give a different name as you wish for your application. Now go to the project folder through the command prompt
  
    cd nameOfTheProject

To create an app, write the command. 

    python manage.py startapp nameOfTheApp  # which is accounts

Then we have to add it to the installed app inside the setting files.

  INSTALLED_APPS = [
      'accounts',  #nameOfTheApp
  ]

Here we can create our super user. To create a superuser, use the command:
  
   python manage.py createsuperuser

The next screen will ask you to provide the username, email, and password.

Next, we will do our database part and run our Django development server using the below commands:
  
    python manage.py migration
    python manage.py migrate
    python manage.py runserver

If everything goes well, you can browse to navigate to the localhost. But, first, go to the web browser and write in the address bar- 127.0.0.1:8000 to see the web application up and running.


Now, we will start our built-in auth application. Django offers us most of the pre-written classes out of the box so that we do not have to reinvent the wheel. The function we will use, and Django offers us out of the box:

    Login through the LoginView class view
    Logout through the LogoutView class view
    Password reset through the PasswordResetView class view
    Password change through the PasswordChangeView class view 


To implement the above functionalities, we have to provide templates in our application. Now, we will create a urls.py file inside the accounts app folder and write the following code:


    from django.contrib.auth import views
    from django.urls import path
    urlpatterns = [
    ]

To log in the user in your Django application, we will use the LoginView, which is the class-based view, and we will add the following lines of the code:


    from django.contrib.auth import views
    from django.urls import path 
    urlpatterns = [
        path('login/', views.LoginView.as_view(), name='login'), 
    ]

Here we used the as_view() method from the LoginView class to return a callback object and that will be assigned as a view function to the path() function.

Now we will provide the template for our login view. To do the template, we will create a templates folder inside the root folder of our account application. Inside the templates folder, create a base.html file and add the following code: base.html. Inside the base.html file, we will observe the CSS and bootstrap tag and Django unique tag. These tags are straightforward, not rocket science.

Next, we will create another folder inside the templates/registration called registration, and we will create a number of HTML templates. First, we will create a login.html template. Click the highlighted text it will take you to the code page. Inside the login.html page, first, we have extended the base.html. Next, we have loaded crispy_forms_tags; also, we have used the POST method to get all the data from the input fields. csrf_token used for the protection of our login form.

    {% extends  'base.html' %}
    {% load crispy_forms_tags %}
    {% block main %}
    <div class="card">
    <div class="card-body">
    <h4 class="card-title">Log in to your account</h4>
    <form method="post">
    {% csrf_token %}
    <input type="hidden" name="next" value="{{ next }}">
    {{ form|crispy }}
    <button type="submit" class="btn btn-primary btn-block">Log in</button>
    </form>
    </div>
    </div> 
    {% endblock %} 

Once you are login where do you want o land? So, for that, we have to redirect our URL to any page. You can land on the home page or any other page, but we will redirect our URL to the admin page here. To redirect the URL, you need to tell Django about that, so write it to the setting file.
# url to redirect after successful login 
    LOGIN_REDIRECT_URL = '/admin' 

Next, we will start to do the logout. To log out the user, we will use the LogoutView class-based view. Now we will add the logout path to the accounts urls.py file.

    urlpatterns = [
       path('login/', views.LoginView.as_view(), name='login'), 
       path('logout/', views.LogoutView.as_view(), name='logout'),
     ]

The same as login, we used the as_view() method to return a callable object from the LogoutView class.
We have done our URL path. Next, we will add the Logged_out.html. Write the following code:


    {% extends 'base.html' %}
    {% block main %}
    <p>You are logged out!</p>
    <a  href="{% url 'login' %}">Log in again</a>
    {% endblock %}


Now, we will do the password reset using the following class

    PasswordResetView
    PasswordRestDoneView
    PasswordRestConfirmView
    PasswordResetCompleteView

Now, we will set our URLs inside the urls.py files in the accounts apps URLs.


    urlpatterns = [ 
       path('password-reset/', views.PasswordResetView.as_view(),name='password_reset'),
       path('password-reset/done/',views.PasswordResetDoneView.as_view(),name='password_reset_done'),
       path('reset/<uidb64>/<token>/',views.PasswordResetConfirmView.as_view(), name='password_reset_confirm'), 
       path('reset/done/', views.PasswordResetCompleteView.as_view(), name='password_reset_complete'), 

    ]  

Once you have written the above URL path next, we will add a registration/password_reset_form.html template. Write the following code. You can get the code also from password_reset_form.html:

    {% extends 'base.html' %}
    {% load crispy_forms_tags %}
    {% block main %}
    <div  class="card">
    <div  class="card-body">
    <h4  class="card-title">Reset your password</h4>
    <form  method="post">
    {% csrf_token %}
    <input  type="hidden"  name="next"  value="{{ next }}">
    {{ form|crispy }}
    <button  type="submit"  class="btn btn-primary btn-block">Reset</button>
    </form>
    </div>
    </div>
    </div>
    {% endblock %}

Now, the challenge for yourself is to try to create the rest of the templates for 

    password_reset_confirm.html
    password_reset_done.html
    password_reset_email.html
    password_reset_complete.html

Now, we will do the password change using the following class-based functionalities.
PasswordChangeView
PasswordChangeDoneView 

Next, we will add our URL path for the above functionalities inside the urls.py in the accounts app.

    urlpatterns = [    
       path('password-change/', views.PasswordChangeView.as_view(),name='password_change'),
       path('password-change/done/',views.PasswordChangeDoneView.as_view(), name='password_change_done'),
    ]

To complete the password change, we have to provide the password_change_form.html in the 
Write the following code for the password_change_form.html

    {% extends 'base.html' %}
    {% load crispy_forms_tags %}
    {% block main %}
    <div  class="card">
    <div  class="card-body">
    <h4  class="card-title"> Change your password</h4>
    <form  method="post">
    {% csrf_token %}
    {{ form|crispy }}
    <button  type="submit"  class="btn btn-success">Change password </button>
    </form>
    </div>
    </div>
    {% endblock %}

We also need to add the password_change_done.html code. Now, write the following code :

    {% extends 'base.html' %}
    {% load crispy_forms_tags %}
    {% block title %}Changed Password successful{% endblock %}
    {% block breadcrumb %}
        <li class="breadcrumb-item active"><a href="{% url 'password_change' %}"></a>Change password</li>
        <li class="breadcrumb-item active">Success</li>
    {% endblock %}
    {% block content %}
    <div class="alert alert-success" role="alert">
        <strong>Success!!</strong>Your password has been changed.
    </div>
    <a href="{% url 'home' %}" class="btn btn-secondary">Return to home page</a>
    {% endblock %}


We are almost finished. Before we wrap up everything, we need to do a couple of things. Our accounts urls.py all the paths we have to connect with the project URL files. Now, go to that loginlogout/urls.py file and write 

    from django.contrib import admin
    from django.urls import path, include
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('accounts/', include('accounts.urls'))
    ]

Here we added include() function. That, include() function will import all the URLs from the accounts urls.py files. Another thing we need to inform the project about our all the templates where they are located. So, go to the setting file and add the following command; just add the pink color code rest of the code will there pre-written:

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [BASE_DIR/'templates'],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',                     
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]

Finally, we will add the email configuration to the setting files.

    EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
    EMAIL_HOST = 'smtp.gmail.com'
    EMAIL_PORT=587
    EMAIL_USE_TLS=True
    EMAIL_HOST_USER='youemail@gmail.com'
    EMAIL_HOST_PASSWORD='yourpassword'

Now, start the development server and write the following URLs to the web browser:

    http://127.0.0.1:8000/accounts/login/

    http://127.0.0.1:8000/accounts/logout/

    http://127.0.0.1:8000/accounts/password-change/

    http://127.0.0.1:8000/accounts/password-reset/

We have seen in this Django authentication tutorial how simply and quickly you can add to your Django project all the above functionalities.

In this tutorial showed how to do quickly and simple way to set up the django project and do login logout and passwprd change and reset.

    You will find full tutorial - https://www.youtube.com/c/programmingwithmohammed
