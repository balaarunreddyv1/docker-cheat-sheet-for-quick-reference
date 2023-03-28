My Docker Cheatsheet For me memory.

Containers run inside docker host
--- whats going on in the container

- `docker history ${IMAGE_NAME}` -> shows the changes in an the image.
- `docker container ls` -> list all the containers
- `docker container top` -> process list in one container
- `docker container inspect ${CONTAINER_ID}` -> details of one container config - json data on how the container was started 
- `docker iamge inspect ${IMAGE_ID}` -> details of one IMAGE config - json data on how the IMAGE was started 
- `docker container stats` -> performance stats for all the containers. CONTAINER ID,NAME,CPU%,MEM,USAGE/LIMIT,MEM%,NET I/O,BLOCK I/O,PIDS
- `docker ps` -> shows the list of running container process. -a for all.
- `docker rm` -> remove a container
- `docker rmi` -> remove image

START A CONTAINER:
- `docker container start ${CONTAINER_ID}` --> starts the ended container if exsists.

RUN A CONTAINER:
- `docker container run -it --name ${CONTAINER_NAME} nginx bash` -> overrided the default command with bash & opens a bash terminal or executes the last argument inside the container. use docker inspect command to inspect the json cmd key value.

RUN COMMAND IN A RUNNING DETACHED CONTAINER:
- `docker container exec -it ${CONTAINER_ID} 'cmd'` -> runs an additional process, will run a command with in the running detached container, use bash/sh cmd to start and interactive shell :-D

ATTACH A CONTAINER:
- `docker attach ${CONTAINER_ID}`

PORTS:
- `docker network ls` -> list all the network
NOTE: `bridge` is a docker virtual network that connects the container ports to to the host ports.
- `docker container run -d -p ${LOCAL_PC_PORT}:${DOCKER_CONTAINER_PORT} --name ${IMAGE_NAME} nginx`
- `docker container port ${CONTAINER_ID}` --> Checks ports status

VOLUME MAPPING:
- `docker run -v ${LOCAL_PC_DIRECTORy}:${DOCKER_CONTAINER_DIRECTORY} imageName`

BUILD A DOCKER FILE:
- `docker build . -t {name}:${tag}` -> (build a docker image inside the Dockerfile folder, use -f to refernece dockerfile location)
- `docker build https://github.com/docker/rootfs.git` -> (build a docker image using a github url that a Dockerfile)

USEFUL ARGUMENTS
-   --name x -> the container name will be x
-   -d -> detached mode
-   -e TEST=arun -> passes an environmental variable into the container.
-   -i -> interactive shell Keeps STDIN open.
-   -t or -tty -> Allocate a pseudo-TTY
-   -rm -> auto deletes the container at the end which will save space.
-   --help => displays all the sub commands



DOCKERFILE:
- # -> comments inside a dockerfile
- ARG -> are env's that are only visible until building an Image, after that they are destroyed.
- FROM -> base docker image that will be expanded late
- WORKDIR -> sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions
- COPY -> copy files to WORKDIR
- ENV -> sets the enviromental variables.
- RUN -> runs a command in the container.
- EXPOSE -> exposed ${DOCKER_CONTAINER_PORT} but we still need to use -p to expose to ${LOCAL_PC_PORT} see line 24.
- CMD -> final command that will run everytime start a image.
  
Example of a dockerfile:
  

```  
  FROM node:10.15.0-slim

  RUN echo "deb http://deb.debian.org/debian stretch main" > /etc/apt/sources.list
  RUN apt-get update -y && apt-get install -yq libgconf-2-4 git bzip2 libfontconfig
  RUN mkdir -p /usr/share/man/man1
  RUN apt-get install -y openjdk-8-jdk-headless
  RUN apt-get update && apt-get install -y wget --no-install-recommends \
      && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
      && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
      && apt-get update \
      && apt-get -y install gconf-service libasound2 libatk1.0-0 libatk-bridge2.0-0 libc6 libcairo2 libcups2 libdbus-1-3 \
                  libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 \
                  libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 \
                  libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget \
                  libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget \
      && apt-get install -y google-chrome-stable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst ttf-freefont \
        --no-install-recommends \
      && apt-get install -y vim \
      && rm -rf /var/lib/apt/lists/* \
      && apt-get purge --auto-remove -y curl \
      && rm -rf /src/*.deb

  # It's a good idea to use dumb-init to help prevent zombie chrome processes.
  ADD https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 /usr/local/bin/dumb-init
  RUN chmod +x /usr/local/bin/dumb-init

  RUN mkdir -p /home/ptruser/
  # Add user so we don't need --no-sandbox.
  RUN groupadd -g 1001 -r ptruser && useradd -u 1001 -r -g ptruser -G audio,video ptruser \
      && chown -R ptruser:ptruser /home/ptruser
  ENV WORKSPACE=/home/ptruser/workspace
  RUN mkdir -p $WORKSPACE && chown -R ptruser:ptruser $WORKSPACE
  WORKDIR $WORKSPACE
  USER ptruser

  ENTRYPOINT ["dumb-init", "--"]
```
  
