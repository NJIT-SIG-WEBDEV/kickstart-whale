# kickstart-whale
Docker containers to help sig-web members kickstart development. 

- [Introduction](#introduction)
- [FAQ](#faq)
  - [Known Issues?](#known-issues)

# Introduction

This project will create three docker containers. The first being a base that includes packages such as npm and flask. SSH is also configured in the base. The second container is running Apache + PHP while the third is running MySQL. 

All can be used together or separately. In fact, if you would rather Apache be in the base container you can clone this repo, add the package to the Dockerfile, configure supervisor to start Apache and then rebuild the image. Use this in whatever way that makes sense for you. If for some reason you would want to install Firefox, you can do that too. Considering the base image includes openssh-server you can X forward. 

Before you can begin you must [install Docker ](https://docs.docker.com/engine/installation/). If you already have Docker ensure it is up to date. 

# FAQ

- Do I need Docker Compose in order to use this?

You do not need Docker Compose but it is highly recommended to get this running. If you choose not to use it you can pull the images manually:

```bash
docker pull jasonodoom/sigweb-docker
docker pull jasonodoom/sigweb-lap
docker pull jasonodoom/sigweb-mysql
```

Then run with your choice of docker options:

```bash
docker run -d --name sigweb-base --restart always -v $PWD/sigweb:/home/webdev jasonodoom/sigweb-docker 
```

If you would like to use Docker Compose you should follow the [installation instructions](https://docs.docker.com/compose/install/) You then have the option of building via Compose or pulling from Docker Hub.

**NOTE:** By default, *docker-compose.yml* is configured to pull from Docker Hub. If you would like to build instead uncomment the "#build:" lines and comment the "image:" lines.

- How do I run this with Compose?

To start the project with Compose run:

```bash
docker-compose up
```

If you don't want stdout you can append **-d** to daemonize the process, silencing output:

```bash
docker-compose up -d
```

- Where is my data?

All `sigweb-*` images are configured to use volumes which allow data between the host and docker container to be shared. Therefore, any files in */home/webdev* in *sigweb-docker* and */var/www/html* in *sigweb-lap* will be stored on your local filesystem.


- How do I build images locally?
  
  If you would rather not use the available images on Docker Hub: edit *docker-compose.yml*, comment or remove the "image:" lines and uncomment "#build." 
  
  It should look something like this when done:

  ```bash
sigweb:
   build: ./sigweb-base
   volumes:
    - $PWD/sigweb/webdev/home:/home/webdev/
   expose:
    - "22"
  #image: jasonodoom/sigweb-docker
  container_name: "sigweb-base"
```
You would do this for each.

- How do I SSH into the base container?

You can SSH into jasonodoom/sigweb-docker by running:

```bash
ssh webdev@IP_ADDRESS_OF_CONTAINER
```
 where *webdev* is the default user. 

- Where can I get more information regarding the MySQL Database?

The jasonodoom/sigweb-db image uses the official Docker mysql image. Visit the [repository page](https://hub.docker.com/_/mysql/) for more information.

# Known Issues

- What is the mysql db password?

The db password is set via **MYSQL_ROOT_PASSWORD** as 'M/$q1' However, it is ignoring the last three characters and only uses the first two. Therefore, as of now the password is simply 'M/'

You can run:

```bash
docker inspect sigweb-db | grep IPAddress | cut -d '"' -f 4
```
to get the IP of the jasonodoom/sigweb-db container.

Then connect via the mysql client:

```bash
mysql -u root -h IP_ADDRESS -pM/
```
for example.


A PHPMyAdmin container is on the To Do List among other things. Pull requests are also encouraged. 

