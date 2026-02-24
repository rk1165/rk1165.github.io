+++
title = 'Docker'
date = 2024-02-11

+++

### Containerization

- When you are operating at scale, container orchestration - automating the deployment, management, scaling, networking, and availability of your containers - becomes essential.
- Software teams use container orchestration to control and automate many tasks:
  - Provisioning and deployment of conatiners.
  - Redundancy and availability of conatiners.
  - Scaling up or removing containers to spread application load evenly across host infrastructure.
  - Movement of containers from one host to another if there's a shortage of resources in a host, or if a host dies.
  - Allocation of resources between containers
  - External exposure of services running in a container with the outside world
  - Load balancing of service discovery between containers
  - Health monitoring of containers and hosts
  - Configuration of an application in relation to the containers running it

![DevOps toolchain](/images/toolchain.png)

- Docker combines the application and its dependencies into an **image** that can then be run on any machine, provided it can run docker.
- A container consists only of a given application and its dependencies
- **Docker image** is a file. It is built according to an instruction file called `Dockerfile`. An image never changes; you cannot edit an existing file, but you can create a new **layer** to it.
- **Docker container** Containers only contain that which is required to execute an application; and you can start, stop and interact with them. They are **isolated** environments in the host machine with the ability to interact with each other and the host machine itself via defined methods (TCP/UDP).
- Containers are instances of /images.
- To get an image, you have to build it with the `Dockerfile`. You then run the image creating a container.
- `docker rmi <image-id or name>` removes an image
- `docker container ls` to list all containers that are running.
- `docker container ls -a` to list all containers.
- `docker container prune` to delete hundreds of stopped containers.
- `docker system prune` to clear everything.
- `docker pull <image>` pulls /images from a docker registry called docker hub.
- `docker run <image-id or name>` runs an image creating a container.
- `docker run -d nginx` starts a container _detached_, meaning that it runs in the background.
- `docker stop <container id or name>` stops the container
- `docker rm <container-id or name>` removes a container.
- `docker rm --force <container-id or name>` to remove the container forcefully.
- `docker /images` lists all /images.
- `docker exec <container-id>` executes a command inside the container
- `docker search <image name>` to search for /images in the Docker Hub.
- /images can be tagged to save different versions of the same image. You define an image's tag by adding `:<tag>` after the image's name.
- /images are composed of different layers that are downloaded in parallel to speed up the download.
- We can also tag /images locally for convenience. For example:
  - `docker tag ubuntu:16.04 ubuntu:xenial` creates the tag `ubuntu:xenial` which refers to `ubuntu:16.04`
- Tagging is also a way to **rename** /images.
  - `docker tag ubuntu:16.04 fav_distro:xenial` will change the name of the image
- `docker run -d --name looper ubuntu:16.04 sh -c 'while true; do date; sleep 1; done'` run a container in the background
- `-it` is short for `-i` - "interactive, connect STDIN" and `-t` - "allocate a pseudo-TTY".
- `docker logs -f looper` output the logs.
- `docker pause looper` : pausing the container, in this case logs of looper froze
- `docker unpause looper` : unpausing the container
- `docker attach looper` attaching to a container from 2nd terminal say.
- When we stop the container in the attached terminal the original one also gets stopped.
- To attach to a container while making sure we don't close it from the other terminal we can specify to not attach STDIN with `--no-stdin` option.
- To enter a container, we can start a new process in it. `docker exec -it looper bash`
- Docker will automatically search [Docker Hub](https://hub.docker.com/) for the image when running a command such as `docker run hello-world`
- [Official /images](https://docs.docker.com/docker-hub/official_/images/) are curated and reviewed by Docker, Inc. They are build from repositories in the [docker-library](https://github.com/docker-library)
- The image marked as "automated" is [automatically built](https://docs.docker.com/docker-hub/builds/) from the source repository.
- We can start a process with `--rm` flag in order to remove it automatically after it has exited.
- After attaching to a container and hitting ctrl+p, ctrl+q detaches us from the STDOUT.
- SOLN 1.5 : `docker exec -it kind_merkle sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'`

```Dockerfile
FROM ubuntu:16.04
WORKDIR /mydir
RUN touch hello.txt
COPY local.txt
RUN wget http://example.com/index.html
CMD ["/bin/bash"]
```

- `WORKDIR` will create **and** set the cwd to `/mydir` after this directive
- `RUN` will execute a command with `/bin/sh -c` prefix
- `COPY` copies an exisiting local file to the second argument. It's preferred to use `COPY` instead of `ADD` when you are just adding files.
- `CMD` is the command that will be executed when using `docker run`. Any Dockerfile should have exactly one CMD instruction
- `docker build .`
- Docker daemon caches all operations. Changing any build directive will invalidate all the caches **after** that line
- We have a problem in the above docker file that wget doesn't exist in Ubuntu base image. So we need to install it, however since apt sources are also not part of the image we need to the following line too : `RUN apt-get update`. But this creates another problem that later if we add another package, the sources might have changed. So it's a good practice to run commands that depend on another commands together like `RUN apt-get update && apt-get install -y wget`
- An instruction like `CMD ["/bin/bash"]` executes `/bin/bash` command when we start the container created from the image which uses such Dockerfile.
- We can optionally override `Ubuntu 16.04` image's `CMD` instruction. by passing the command at the time of creating a container from our resulting image using for e.g. `docker run -it <myImageName> echo 'Docker is easy.'`
- The updated dockerfile is

```Dockerfile
FROM ubuntu:16.04
WORKDIR /mydir
RUN apt-get update && apt-get install -y wget
RUN touch hello.txt
COPY local.txt .
RUN wget http://example.com/index.html
```

- `docker build -t myfirst .` builds and tags the image
- If we make any changes to our container such as by adding files: we can check the difference between our image and container using `docker diff $container_id`.
- The output has characters in front of them which indicates the type of change in the container's fs. A = added, D = deleted, C = changed
- To create a new image from these changes we can use `docker commit $container_id $new_image_name`
- We can use `ENTRYPOINT` to define the main executable and then docker will combine our run arguments for it.
- `docker cp "dazzling_merkle://mydir/Imgur-JY5tHqr.mp4" .` to copy file to local
- By **bind mounting** a host (our machine) folder to the container we can get the file directly to our machine.
- `docker run -v "$(pwd):/mydir" youtube-dl https://imgur.com/JY5tHqr` : we mount our current folder as `/mydir` in our container
- A volume is simply a folder (or a file) that is shared between the host machine and the container.
- Opening a connection from outside world to a docker container happens in two steps
  - Exposing port : means that you tell Docker that the container listens to a certain port
  - Publishing port : means that Docker will map containers ports to host (your machine) ports
- To expose a port, add line `EXPOSE <port>` in your Dockerfile
- To publish a port, run the container with `-p <host-port>:<container-port>`
- `docker port $container_id` : to list the port mappings for a container
- We could also limit connections to certain protocol only, in this case udp by adding the protocol at the end : `EXPOSE 4567/udp` and `-p 1234:4567/udp`
- 1.10 : `docker run -p 5000:5000 frontend-docker npm start`
- 1.11 : `docker run -v "$(pwd)/logs.txt:/backend-example-docker/logs.txt" -p 8000:8000 backend-docker npm start`
- Before pushing we should rename the image : `docker tag youtube-dl <username>/<repository_name_created_on_dockerhub>`
- To authenticate our push we should first login : `docker login`
- For pushing we can use `docker push <username>/<repositoryname>`
- 1.13 : `docker run -p 8080:8080 spring-project java -jar ./target/docker-example-1.1.3.jar`
- 1.14 : `docker run -p 3000:3000 ruby-project rails s`
- 1.03 : secret
- 1.04 : Secret message is "Docker is easy" cmd : `docker run -d devopsdockeruh/exec_bash_exercise:latest; docker exec -it vibrant_goldstine bash`
- docker run = docker create + docker start
- docker start -a $ID : `-a` is what's going to make docker watch for o/p from the container and print it to your terminal. Kind of attach to the container
- we cannot change the default command when the container was started
- docker logs $ID
- docker exec -i : allows us to have stuff we type into our terminal directed into the running process. -t : prettifying and more
- docker commit -c 'CMD ["redis-server"]' $ID/$NAME : creates an image from a container with command
- The docker history command shows the size of each layer associated with an image. `docker history layer-demo/latest`
- `docker /images --filter "dangling=true"` : To list out all the dangling /images
- `docker /images --quiet --filter=dangling=true | xargs --no-run-if-empty docker rmi` : shows the dangling /images and then removes them subsequently

#### part 2

```yml
version: "3"
services:
  youtube-dl-ubuntu:
    image: <username>/<repositoryname>
    build: .
```

- The key `build:` value can be set to a path (ubuntu), have an object with `context` and `dockerfile` keys or reference a `url of a git repo`
- Next we can build and push with these commands
  - `docker-compose build`
  - `docker-compose push`
- volumes in docker-compose are defined with the following syntax : `location-in-host:location-in-container`
- 2_1: docker-compose run first_volume
- Compose is really meant for running web services.
- Environment variables can also be given to the containers in docker-compose

```yml
version: "3.5"
services:
  backend:
    image:
    environment:
      - VARIABLE=VALUE
      - VARIABLE
```

- Compose can scale the service to run multiple instances : `docker-compose up --scale whoami=3`
- We can use `docker-compose port` to find out which ports the instances are bound to.
- 2.4 : docker-compose up --scale compute=3
- Connecting two services such as a server and its database in docker can be achieved with docker-compose networks. In addition to starting services listed in docker-compose.yml the tool automatically creates and joins both containers into a network where the service name is the hostname. This way containers can reference each other simply with their names
- Inspect a container with `docker inspect $container_name`
- Compose uses the CWD as a prefix for container and volume names. The prefix can be overridden with `COMPOSE_PROJECT_NAME` environment variable.
- `docker volume ls`
- `docker volume prune`
- To put the whole "application" down we can use `docker-compose down`
- Change the ownership using root user and then switch to USER
- `docker build -t youtube-dl:alpine-3.7 -f Dockerfile.alpine .` : using a file
- docker run -v "$(pwd):/app" youtube-dl:alpine-3.7 https://imgur.com/JY5tHqr
- Multi-stage builds are useful when you need some tools just for the build but not for the execution of the image CMD
- Kubernetes is the de facto way of orchestrating your containers in large multi-host environments.

### Book

- `docker image rm -f $(docker image ls -f reference='diamol/*' -q)`
- `docker container top $ID` : lists the process running in the container
- `docker container logs $Id` : displays any log entries the container has collected
- Docker collects log entries using the o/p from the application in the container.
- `docker container inspect $ID` : shows all details of a container
- `docker container run --interactive --tty diamol/base`
- `docker container run --detach --publish 8088:80 diamol/ch02-hello-diamol-web`
- `docker container status $ID` : shows a live view of how much CPU, memory, network, and disk the container is using
- `docker container rm --force $(docker container ls --all --quiet)`
  ![Docker Components](/images/docker-components.png)
- The Docker Engine is the management component of Docker. It looks after the local image cache, downloading /images when you need them, and reusing them if they’re already downloaded. It also works with the operating system to create containers, virtual networks, and all the other Docker resources. The Engine is a background process that is always running
- The Docker Engine makes all the features available through the Docker API, which is just a standard HTTP-based REST API.
- When you run Docker commands, the CLI actually sends them to the Docker API, and the Docker Engine does the work.
- `docker container cp index.html $ID:/usr/local/apache2/htdocs/index.html` : this copies the local file index.html to container with ID's file
- `docker container run --env TARGET=google.com diamol/ch03-web-ping`
- `docker image build --tag web-ping .`
- `docker container run -e TARGET=docker.com -e INTERVAL=5000 web-ping:latest`
- `docker system df` : shows exactly how much disk space Docker is using
- Image layers are shared around
- Any Dockerfile you write should be optimized so that the instructions are ordered by how frequently they change -- with instructions that unlikely to change at the start of file
- `docker container commit $container_name $image_name` : to create a new image from a container's changes
- `docker network create nat`
- when you run containers you can explicitly connect them to that Docker network using the --network flag, and any containers on that network can reach each other using the container names
- `docker image ls -f reference=diamol/golang -f reference=image-gallery` : filtering /images
- There are actually four parts to a full image name (which is proeprly called **image reference**)
- docker.io/diamol/golang:latest
- registry/account/repo_name:image_tag
- `docker container run -d -p 5000:5000 --restart always diamol/registry` : run the docker registry in a container
- `docker info` : to see all the information
- Golden /images use an official image as the base and then add in whatever custom setup they need, such as installing security certificates or configuring default environment settings
- Each container has its own filesystem. One can run multiple containers from the same Docker image, and they will all start with the same disk contents.
- `docker container inspect --format '{{.Mounts}}' $NAME/$ID` : to inspect volume information. Volumes are shown as "mounts" when inspecting a container.
- you can run a container with the `volumes-from` flag, which attaches another container’s volumes
- `docker container run -d --name t3 --volumes-from todo1 diamol/ch06-todo-list` : this container will share the volume from todo1
- `docker container run --mount type=bind,source=$source,target=$target -d -p 8012:80 diamol/ch06-todo-list`
- `docker container run --name todo-configured -d -p 8013:80 --mount type=bind,source=$source,target=$target,readonly diamol/ch06-todo-list`
- When you mount a target that already has data, the source directory replaces the target directory - so the original files from the image are not available.
- However, if you mount a single file from host to the target directory that exists in the container filesystem, the directory contents are merged
- There can be multiple image layers, multiple volume mounts, and multiple bind mounts in a container, but they will always have a single writable layer
- General guidelines for using the storage options:
  - **Writeable layer** perfect for short-term storage, like caching data to disk to save on network calls or computations
  - **Local bind mounts** used to share data b/w the host and the container.
  - **Distributed bind mounts** used to share data b/w network storage and conatiners. Need to aware about the performance and network storage may not offer full filesystem features.
  - **Volume mounts** used to share data b/w container and a storage object that is managed by Docker. Useful for persistent storage, where the application writes data to the volume.
  - **Image layers** these present the initial filesystem for the container.

### Docker compose investigation commands

- `sudo nsenter -n -t 1837916 netstat -tulpn` : find a UDP server running in the same network namespace as PID `1837916`
- `sudo nsenter -n -t $PID netstat -tulpn` : using nsenter to run netstat in the same network namespace as the process with $PID
- `sudo nsenter -n -t 1837916 dig +short @127.0.0.11 59426 rails_server` : (check that it's a DNS server) to make a dns query to port 59426 for $PID : 1837916
- `sudo nsenter -n -t 1837916 iptables-save`: running iptables-save in the container's namespace
