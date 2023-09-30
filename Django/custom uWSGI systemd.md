## Creating a custom UWSGI service [Ubuntu]

Whenever you make the previous configuration working correctly, and the standard UWSGI service still does not work, you might need to create a custom service yourself.

In some configurations, I personally experienced the default UWSGI service not being able to recognize the Python virtual environment somehow, and therefore not bwing able to start it by using the correct interpreter.

In a similar case, what I usually do to fix the issue is:

1. Create a Service
    
    ```shell
    sudo vim /etc/systemd/system/geonode-uwsgi.service
    ```
    

- ```ini
- ```
[Unit]
    Description=GeoNode UWSGI Service
    
    [Service]
    User=geosolutions
    # The configuration file application.properties should be here:
    
    #change this to your workspace
    WorkingDirectory=/home/geosolutions/geonode
    
    #path to executable.
    #executable is a bash script which calls jar file
    ExecStart=/home/geosolutions/geonode-uwsgi.sh
    
    SuccessExitStatus=143
    TimeoutStopSec=10
    Restart=on-failure
    RestartSec=5
    
    [Install]
    WantedBy=multi-user.target
```
- Create a Bash Script to Call Your Service
    
    ```shell
    vim /home/geosolutions/geonode-uwsgi.sh
    ```
    

```shell
#!/bin/bash
source ~/Envs/geonode3/bin/activate
uwsgi -c /etc/uwsgi/apps-enabled/geonode.ini
```

```shell
chmod +x /home/geosolutions/geonode-uwsgi.sh
```

Start the Service

```shell
sudo systemctl daemon-reload
sudo systemctl start geonode-uwsgi
sudo systemctl status geonode-uwsgi
```

If everything is working, don't forget to enable the new service and disable the default one

```shell
sudo systemctl disable uwsgi
sudo systemctl enable geonode-uwsgi
```