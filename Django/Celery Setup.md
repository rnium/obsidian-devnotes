1.  Install `celery` package in the virtualenvironment
2.  Install Redis as the message broker
3. Install `redis-py` package in the virtualenvironment
4. Configuration in the project:
- create `celery.py` in the same directory as `settings.py` (preferably *config*) and add the following code in it:
```
import os
from celery import Celery

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
app = Celery("django_celery")
app.config_from_object("django.conf.settings", namespace="CELERY")
app.autodiscover_tasks()
```
- Now add this in `__init__.py` file add this:
```
from .celery import app as celery_app
__all__ = ('celery_app',)
```
- In `settings.py` add the following variables for redis
```
CELERY_BROKER_URL = "redis://localhost:6379"
CELERY_RESULT_BACKEND = "redis://localhost:6379"
```
- Now  with everything set up, add `tasks.py` in every django project's app which will contain the `shared_task` functions
- call `shared-task` function's `delay()` method with requried parameters for that  `shared-task`

## Celery Systemd Service file

```
[Unit]
Description=Celery Service
After=network.target

[Service]
Type=forking
User=root
Group=root
WorkingDirectory=/home/rony/sec_rdb
ExecStart=/bin/sh -c './venv/bin/celery -A config multi start worker1 -l info --pidfile=/var/run/celery/worker1.pid'
ExecStop=/bin/sh -c './venv/bin/celery -A config multi stopwait worker1 -l info --pidfile=/var/run/celery/worker1.pid'
ExecReload=/bin/sh -c './venv/bin/celery -A config multi restart worker1 -l info --pidfile=/var/run/celery/worker1.pid'
Restart=always

[Install]
WantedBy=multi-user.target
```
