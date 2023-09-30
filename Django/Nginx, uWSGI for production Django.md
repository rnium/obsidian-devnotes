- Install `nginx, uwsgi` globally or run the setup.sh listed in `0. setup.sh`
- uwsgi files will be in `/etc/uwsgi/` directory
- add `uwsgi.params` in  `/etc/uwsgi/` directory
- `.ini` files should be in the `/etc/uwsgi/vessals` directory
- modify directive values as per requirement

## 0. `setup.sh`
```
#!/bin/sh
apt update
apt upgrade
apt install nginx
apt install python3-venv
apt install python3-pip
apt install build_essential python3-dev libpcre3 libpcre3-dev
pip install uwsgi
mkdir -p /etc/uwsgi/vessals
```

## 1. `uwsgi.ini` configs
```
[uwsgi]
project = proj_name
base = /home/

chdir = %(base)/%(project)
home = %(base)/%(project)/venv
module = config.wsgi

master = true
processes = 2

socket = /run/uwsgi/%(project).sock
chmod-socket = 666
vaccuum = true
daemonize = /var/log/%(project)_uwsgi.log
pidfile = /run/uwsgi/%(project).pid
```

## 2. `uwsgi.service` 
```[Unit]
Description=uWSGI Emperor service

[Service]
ExecStartPre=/bin/bash -c 'mkdir -p /run/uwsgi'
ExecStart=/usr/local/bin/uwsgi --emperor /etc/uwsgi/sites
Restart=always
KillSignal=SIGQUIT
Type=notify
NotifyAccess=all

[Install]
WantedBy=multi-user.target
```

## 3. `uwsgi_params`
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

## 4. `nginx` config
```
# inside a server context

location /static {
	alias /home/rony/projectName/static;
	access_log off;
	expires 7d;
	add_header Cache-Control "public, max-age=604800";
}

location /media {
	access_log off;
	alias /home/rony/projectName/media;
}

location / {
	uwsgi_pass unix:///run/uwsgi/projectName.sock;
}
```