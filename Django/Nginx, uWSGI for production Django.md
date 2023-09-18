## 1. `uwsgi_params`

```
uwsgi_param QUERY_STRING $query_string;
uwsgi_param REQUEST_METHOD $request_method;
uwsgi_param CONTENT_TYPE $content_type;
uwsgi_param CONTENT_LENGTH $content_length;
uwsgi_param REQUEST_URI $request_uri;
uwsgi_param PATH_INFO $document_uri;
uwsgi_param DOCUMENT_ROOT $document_root;
uwsgi_param SERVER_PROTOCOL $server_protocol;
uwsgi_param REQUEST_SCHEME $scheme;
uwsgi_param HTTPS $https if_not_empty;
uwsgi_param REMOTE_ADDR $remote_addr;
uwsgi_param REMOTE_PORT $remote_port;
uwsgi_param SERVER_PORT $server_port;
uwsgi_param SERVER_NAME $server_name;
```

## 2. `uwsgi.ini` configs
```
[uwsgi]
# full path to Django project's root directory
chdir = /home/<projetc directory, the folder which has manage.py file>/
# Django's wsgi file
module = config.wsgi
# full path to python virtual env
home = /home/<path to venv>/
# enable uwsgi master process
master = true
# number of worker processes
processes = 2
# the socket (use the full path to be safe
socket = /home/path/to/socket.sock
# socket permissions
chmod-socket = 666
# clear environment on exit
vacuum = true
# daemonize uwsgi and write messages into given log
daemonize = /home/path/to/uwsgi.log
```