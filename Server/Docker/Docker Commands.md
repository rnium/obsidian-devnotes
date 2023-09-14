1. `docker run hello-world`
2. `docker build . -t test/firstimage:mytag`
3. `docker run -p 8080:3000 test/firstimage` : Maps 8080 port of host machine to 3000 of Container port*
4. `docker ps` : *"ps" stands for "Process Status." It is a command used to display information about currently running processes on the system. In the case of Docker, `docker ps` is a Docker-specific command that provides information about running Docker containers, which are essentially isolated processes managed by the Docker daemon. It displays details about the active containers*

5. `docker ps -a` : all docker containers
6. `docker stop <CONTAINER ID>`
7. `docker rm <CONTAINER ID>`
8. `docker image rm <IMAGE NAME>`  *Remove image*
9. `docker image rm -f <IMAGE NAME>` : *Force remove*
10. `docker build --platform linux/amd64 -t rnium/gettingstarted` : build for specific platform 