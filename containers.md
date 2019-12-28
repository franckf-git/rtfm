# containers

## podman

```bash
podman ps                      #check podman
podman container list --all    #list container
podman image ls -a             #check your images
podman rm UUID                 #remove containers
podman search httpd            #searching
podman image rm UUID           #remove images
podman info
podman run -P                  #allocation dynamique sur tous les ports exposés dans le Dockerfile

podman run -d -v /root/mysql-data:/var/lib/mysql/data:Z -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -e MYSQL_DATABASE=db -p 3306:3306 registry.access.redhat.com/rhscl/mariadb-102-rhel7
```

```bash
args=(
    # Disable SELinux label to enable mounting runtime socket
    --security-opt label=disable
    # Enable legacy X11
    -v /tmp/.X11-unix/:/tmp/.X11-unix/
    -e DISPLAY=:0
    # Enable xdg runtime for wayland and pulseaudio socket
    -v /run/user/1000/:/run/user/1000/
    -e XDG_RUNTIME_DIR=/run/user/1000
    -e PULSE_SERVER=/run/user/1000/pulse/native
    # fix XError bad access
    --ipc host
)
podman run ${args[@]} ...
```

### Volumes

Créer un container avec un volume "testvolume" persistant (et créer au lancement de la commande)

```bash
podman run --name=test --detach --interactive --tty --mount type=volume,source=testvolume,destination=/home/node/app --publish=8080:80/tcp --publish=8000:8000/tcp devnodesqlhttpd
```

Les données de ce volume seront disponibles dans :

```
.local/share/containers/storage/volumes/testvolume/_data
```

### Create a container with a Dockerfile, run it and enter into

```bash
podman build --tag fedora-nodejs:podman .
podman run --name livre-d-or --detach --interactive --tty --volume ~/livre-d-or:/home:Z --publish 8080:8080/tcp localhost/fedora-nodejs:podman
podman exec --user=root --interactive --tty livre-d-or /bin/bash
```

### Examples Dockerfile

```config
FROM centos:latest
RUN yum -y install httpd
CMD [“/usr/sbin/httpd”, “-D”, “FOREGROUND”]
EXPOSE 80
```

```config
FROM registry.centos.org/che-stacks/centos-stack-base

WORKDIR /usr/src/open-adventure

COPY ./open-adventure /usr/src/open-adventure

RUN sudo yum -y update && \
    sudo yum -y install kernel-headers && \
    sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org && \
    sudo rpm -Uvh http://elrepo.org/linux/kernel/el7/x86_64/RPMS/elrepo-release-7.0-3.el7.elrepo.noarch.rpm && \
    sudo yum --enablerepo=elrepo-kernel -y install kernel-ml && \
    sudo yum --enablerepo=elrepo-kernel -y swap kernel-headers -- kernel-ml-headers && \
    sudo yum -y install centos-release-scl && \
    sudo yum -y install gcc && \
    sudo yum -y install make && \
    sudo yum -y install libedit-devel && \
    sudo yum -y install python-yaml && \
    sudo make

CMD tail -f /dev/null
```

```config
FROM registry.fedoraproject.org/fedora

RUN dnf -y update && \
    dnf -y install nodejs mysql-server
```

#### Projet Node

Dockerfile

```config
FROM node:alpine

WORKDIR /nodebook
ADD ./package.json ./package-lock.json ./
RUN npm ci
ADD ./examples ./examples

CMD ["node", "examples/hello.js"]
```

.dockerignore

```config
node_modules
```

### Create a systemd service with podman

```bash
vi /etc/systemd/system/mariadb-podman.service
```

```config
[Unit]
Description=Custom MariaDB Podman Container
After=network.target

[Service]
Type=simple
TimeoutStartSec=5m
ExecStartPre=-/usr/bin/podman rm "mariadbpodman"

ExecStart=/usr/bin/podman run --name mariadbpodman -v /root/mysql-data:/var/lib/mysql/data:Z -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -e MYSQL_DATABASE=db -p 3306:3306 registry.access.redhat.com/rhscl/mariadb-102-rhel7

ExecReload=-/usr/bin/podman stop "mariadbpodman"
ExecReload=-/usr/bin/podman rm "mariadbpodman"
ExecStop=-/usr/bin/podman stop "mariadbpodman"
Restart=always
RestartSec=30

[Install]
```

```bash
systemctl daemon-reload
```

### systemd in podman

Dockerfile

```config
FROM fedora

RUN dnf -y install httpd; dnf clean all; systemctl enable httpd

EXPOSE 80

CMD [ "/sbin/init" ]
```

```bash
podman build -t systemd .
```

sur la machine hôte :

```bash
sudo setsebool -P container_manage_cgroup true
```

```bash
podman run -ti -p 80:80 systemd
```

### --help

```
  attach      #Attach to a running container
  build       #Build an image using instructions from Dockerfiles
  commit      #Create new image based on the changed container
  container   #Manage Containers
  cp          #Copy files/folders between a container and the local filesystem
  create      #Create but do not start a container
  diff        #Inspect changes on container's file systems
  events      #Show podman events
  exec        #Run a process in a running container
  export      #Export container's filesystem contents as a tar archive
  generate    #Generated structured data
  healthcheck #Manage Healthcheck
  help        #Help about any command
  history     #Show history of a specified image
  image       #Manage images
  images      #List images in local storage
  import      #Import a tarball to create a filesystem image
  info        #Display podman system information
  init        #Initialize one or more containers
  inspect     #Display the configuration of a container or image
  kill        #Kill one or more running containers with a specific signal
  load        #Load an image from container archive
  login       #Login to a container registry
  logout      #Logout of a container registry
  logs        #Fetch the logs of a container
  mount       #Mount a working container's root filesystem
  pause       #Pause all the processes in one or more containers
  play        #Play a pod
  pod         #Manage pods
  port        #List port mappings or a specific mapping for the container
  ps          #List containers
  pull        #Pull an image from a registry
  push        #Push an image to a specified destination
  restart     #Restart one or more containers
  rm          #Remove one or more containers
  rmi         #Removes one or more images from local storage
  run         #Run a command in a new container
  save        #Save image to an archive
  search      #Search registry for image
  start       #Start one or more containers
  stats       #Display a live stream of container resource usage statistics
  stop        #Stop one or more containers
  system      #Manage podman
  tag         #Add an additional name to a local image
  top         #Display the running processes of a container
  umount      #Unmounts working container's root filesystem
  unpause     #Unpause the processes in one or more containers
  unshare     #Run a command in a modified user namespace
  varlink     #Run varlink interface
  version     #Display the Podman Version Information
  volume      #Manage volumes
  wait        #Block on one or more containers
```
