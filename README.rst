
توضیحات:
============
یک شاخه از ``django-comments-dab`` که توسط irania9O_ فارسی سازی شده است. 

.. _irania9O : https://github.com/irania9O

به شما اجازه می دهد تا سیستم دیدگاه را برای model های خود فعال کنید مانند blogs, pictures, video و غیره ...

*قابلیت های ارائه شده*

    1. ایجاد یک دیدگاه (v2.0.0 به صورت احراز شده یا ناشناس)

    2. پاسخ دادن به دیدگاه موجود. (v2.0.0 به صورت احراز شده یا ناشناس)

    3. ویرایش دیدگاه (کاربر احراز شده ی نویسنده دیدگاه)

    4. حذف دیدگاه. (کاربر احراز شده ی نویسنده دیدگاه و مدیران)

    5. واکنش دادن به دیدگاه. (کاربران احراز شده)

    6. قابلیت های دیگر...

- همه ی عملیات ها توسط API از ورژن V2.0.0 قابل انجام است.

- Bootstrap 4.1.1 در تم استفاده شده و مورد نیاز است.


نصب
============

پیش نیاز ها:
-------------

    1. **django>=2.1**
    2. **djangorestframework**  # برای استفاده از API
    3. **Bootstrap 4.1.1**


نصب:
-------------
سورس فایل را از پیتهاب بردارید و سپس پوشه comment را به برنامه خود کپی کنید.

::

    $ git clone https://github.com/irania9O/Persian-Django-App-Comment.git

انجام تنظیمات:
--------------------------

    1. نام ``comment`` را به  installed_apps در فایل ``settings.py`` پروژه خود اضافه کنید. حتما باید بعد از ``django.contrib.auth`` باشد.
    2. متغیر ``LOGIN_URL`` باید در  settings مشخص شود.

فایل ``settings.py`` باید به این شکل باشد:

.. code:: python

    INSTALLED_APPS = (
        'django.contrib.admin',
        'django.contrib.auth',
        ...
        'comment',
        ..
    )

    LOGIN_URL = 'login'  # یا لینک مورد نظرتان

در فایل ``urls.py``:

.. code:: python

    urlpatterns = patterns(
        path('admin/', admin.site.urls),
        path('comment/', include('comment.urls')),
        ...
        path('api/', include('comment.api.urls')),  # برای استفاده از API
        ...
    )

انجام عملیات دیتابیس:
-----------

::

    $ python manage.py migrate comment



برپایی
=====

مرحله 1 - اتصال مدل نظر با مدل هدف
-------------------------------------------------------

در فایل models.py بخش را ``comments`` به عنوان یک ``GenericRelation`` برای مدل اضافه کنید.

توجه: باید دقت کنید که نام به صورت ``comments`` **و نه به صورت** ``comment``.

مثال: برای مدل ``Post``, به صورت زیر استفاده می شود:

.. code:: python

    from django.contrib.contenttypes.fields import GenericRelation

    from comment.models import Comment

    class Post(models.Model):
        author = models.ForeignKey(User)
        title = models.CharField(max_length=200)
        body = models.TextField()
        # the field name should be comments
        comments = GenericRelation(Comment)

Step 2 - افزودن template tags:
------------------------------

بخش ``render_comments`` *از دو آرگومان ضروری و یک آرگومان اختیاری استفاده می کند*:

    1. نمونه ای از مدل مورد نظر (**اجباری**)
    2. درخواست شی. (**اجباری**)
    3. oauth. (اختیاری - به صورت پیش فرض false است)

استفاده
=====

1. استفاده اولیه:
----------------

استفاده از تگ ``include_bootstrap`` برای bootstrap-4.1.1, اگر قبلا در پروژه استفاده شده از این بخش صرف نظر کنید

در بخش template (مثال. post_detail.) template tags زیر را اضافه کنید جایی که ``obj`` یک نمونه از مدل post است.

.. code:: jinja

    {% load comment_tags %}  {# Loading the template tag #}
    {% render_comments obj request %}  {# Render all the comments belong to the passed object "obj" #}
    {% include_bootstrap %} {# Include bootstrap 4.1.1 - remove this line if BS is already used in your project #}


2. استفاده بیشتر:
------------------

برای استفاده بیشتر شما نیاز دارید که مستندات را از لینک زیر یا از پوشه ی docs_ بخوانید.


.. _docs: https://github.com/Radi85/Comment/tree/develop/docs


مثال
========

در محیط مجازی پایتونی خود میتوانید ایت مثال را تست کنید.

.. code:: bash

    $ git clone https://github.com/irania9O/Persian-Django-App-Comment.git  # or clone your forked repo
    $ cd Comment
    $ python3 -m venv local_env  # or any name. local_env is in .gitignore
    $ export DEBUG=True
    $ source local_env/bin/activate
    $ pip install -r test/example/requirements.txt
    $ python manage.py migrate
    $ python manage.py create_initial_data
    $ python manage.py runserver





با استفاده از اطلاعات زیر وارد شوید:

    username: ``test``

    password: ``test``

آیکون ها از Feather_ برگرفته شده اند. بابت کار بسیار خوبشان ازشان متشکریم.

.. _Feather: https://feathericons.com