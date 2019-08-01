---
layout: article
title:  "Integrating Django Celery with Travis CI"
date:  2019-08-01 22:13:00 +0300
categories: django celery
description: "Celery is a great asynchronous task queue with stable Django integration. However, there is a lack of information about setting up Travis on such projects."
---

 <a target="_blank" href="https://travis-ci.org/">Travis CI</a> is a great tool for automatic running tests for various Open Source projects, published on GitHub. 
It's relatively easy to add Travis CI to Django Python project <a target="_blank" href="https://docs.travis-ci.com/user/languages/python/">Travis CI</a>, 
however, if you you use  <a target="_blank" href="http://www.celeryproject.org/">Celery</a> , the task of running tests becomes non-trivial.
Current example will use  <a target="_blank" href="https://www.rabbitmq.com/">RabbitMQ</a> as broker and results storage.
 <a target="_blank" href="https://docs.celeryproject.org/en/latest/django/first-steps-with-django.html">This guide</a> will help you to add Celery into your Django project.

At first, let's start our project and create app. The idea is to create first async function in the file `celery.py` file and econd one in "polls" app. 
The first function can be used to check Celery worker status while second can be used as a template for your own apps functions.
Let's start project by running `django-admin startproject celery_travis_integration` 
and start an app via 
`python manage.py startapp polls`.

After that we need to add the line `'polls.apps.PollsConfig',` into `INSTALLED_APPS` array of `settings.py`. 
Then define variable `CELERY_RESULT_BACKEND = "rpc"` for `celery_travis_integration/celery_travis_integration/settings.py`.

Let's define files necessary for Celery to run.

Firstly we need to define Celery instance and add first test method in `celery_travis_integration/celery_travis_integration/celery.py`:

```
from __future__ import absolute_import, unicode_literals
import os
from celery import Celery
from celery.decorators import task
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'celery_travis_integration.settings')
app = Celery('celery_travis_integration')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
@task(bind=True)
def debug_task(self):
    return "Debug_task completed"
```

Secondly we need to ensure that our app is imported into Celery `celery_travis_integration/celery_travis_integration/__init.py__`:

```
from __future__ import absolute_import, unicode_literals
from .celery import app as celery_app
__all__ = ('celery_app',)
```

Thirdly let's create a second test method in the `polls/tasks.py`:

```
from celery.decorators import task
@task(bind=True)
def debug_task2(self):
    return "Debug_task2 completed"
```

Fourth task is to add tests into `celery_travis_integration/tests.py`. Test will fail if connection to Celery is missing.

```
from .celery import debug_task
from django.test import TestCase
class CeleryTravisIntegration(TestCase):
    def test_broker(self):
        print ("Starting test_broker")
        task_id = debug_task.delay().task_id
        print ("Waiting result")
        debug_task.AsyncResult(task_id).get(timeout=5)
        print ("Completed")
```

Fifth task is to add tests into `polls/tests.py`. 

```
from django.test import TestCase
from .tasks import debug_task_polls
class PollsPageTests(TestCase):
    def test_async_polls(self):
        print ("Starting test_async_polls")
        task_id = debug_task_polls.delay().task_id
        print ("Waiting result")
        debug_task_polls.AsyncResult(task_id).get(timeout=5)
        print ("Completed")
```

Later we need to run a standart Django set of commands:
`python manage.py makemigrations`, 
`python manage.py migrate`, 
`python manage.py runserver`, 
`python manage.py test`.

Output should be like that:

```
Ran 2 tests in 0.159s
OK
Destroying test database for alias 'default'...
```

If you face following expression: `celery.exceptions.NotRegistered: 'polls.tasks.debug_task_polls'`, simply restart celery worker. Worker doesn't detech changes in the code automatically, therefore it needs to be restarted manually each time you change async functions in the project.
In some cases, when worker can't connect to RabbitMQ, it might happen that RabbitMQ server needs a restart. Since we connect celery to it via `localhost`, firewall shouldn't cause much trouble during our integration.


Before running tests you can check if your methods have been loaded into Celery. 
For that we need to start worker and to run command `celery -A celery_travis_integration worker --loglevel=info --pool=solo` in another window of terminal.
Such output means that both functions have been recognised by worker:

```
[tasks]
  . celery_travis_integration.celery.debug_task
  . polls.tasks.debug_task_polls
```

Now our project is ready for travis integration. To do it, we need to configure `requirements.txt` and `.travis.yml` files.
File `.travis.yml`:

```
addons:
  apt:
    packages:
      - rabbitmq-server
language: python
python:
  - "3.6"
  - "3.7"
install:
  - pip install -r requirements.txt
script:
  - celery multi start worker1 -A celery_travis_integration --pool=solo
  - python manage.py test
  - celery multi stopwait worker1
dist: xenial
```

File `requirements.txt`:

```
Django>=2.1.9
celery>=4.3.0
```

`.gitignore` file can be downloaded from <a target="_blank" href="https://www.gitignore.io/api/django">gitignore.io</a>

 ![Travis CI status]({{ site.url }}/assets/img/2019-08-01-celery-travis-integration/successful-build.png)

As we can see, the build was completed successfully.

If you face any issues, the code of test project can be found on  <a target="_blank" href="https://github.com/sashabrava/celery_travis_integration">GitHub</a>.