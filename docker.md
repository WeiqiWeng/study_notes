# Docker Study Notes

## Docker Command

### Basic

`docker images` lists all docker images pulled or built so far. 

`docker ps` shows the containers that are currently running. `docker ps -a` lists all the containers, active or not. 

`docker pull xxx:yyy` pulls version `yyy` of application `xxx`. `docker pull xxx` is equivalent to `docker pull xxx:latest`, which pulls the latest version.

Once docker image is pulled, one can run `docker start container_id` to have `container_id` running in container. `docker stop container_id` terminates `container_id`. 

`docker run -d xxx:yyy` runs `xxx:yyy` in detach mode, and returns container ID. If `xxx:yyy` is not on your machine yet, this command will pull the image and start the image in container. 

One host can open one host only once. But several containers can open the same port number. Once host port is binded with container port, messages through host port will be sent to container port. 

`docker run -pHOST_PORT:CONTAINER_PORT xxx:yyy` binds host port 6000 to container port 6379, and runs `xxx` in container. 

You can also give your container a name by `docker run xxx:yyy --name container_name`. 

`docker rm container_id` removes container identified by `container_id`. 

`docker rmi image_id` removes `image_id`. You need to remove any container running the `image_id`. 

### Diagnosis

`docker logs container_id` or `docker logs container_name` checks log of your containers. 

`docker exec -it container_id /bin/bash` uses bash shell to enter the container in interactive mode. Then you can run your command and diagnose. `docker exec -it container_name /bin/bash` also works. If `bash` is not installed, you can try `docker exec -it container_id /bin/sh`. `exit` to exit the container. 

### Docker Network

Containers can live in a isolated network created by Docker. 

`docker network ls` lists all networks. `docker network create network_name` creates a network called `network_name`. To run the container in `network_name`, run `docker run -d --net network_name`. For example, if we want to 
1. run mongoDB in `mongo-network`, 
2. over write environment variables `MONGO_INITDB_ROOT_USERNAME` and `MONGO_INITDB_ROOT_PASSWORD` with given values, and 
3. bind host port 8000 to container port 8000, 
we can do  

```shell
docker run -d -p8000:8000 -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=root_password --name mongodb --net mongo-network mongo
```
.

### Docker Compose

It's a little tedious to run the docker command each time you want to develop something. For each project that involves working with docker images, one can specify a docker-compose file, a yaml file. For the MongoDB image above, the equivalent docker compose file is included as follow. 
```yaml
# mongodb.yaml
version: '3'
services:
  mongodb: # container name
    image: mongo:latest
    ports: 
      - 27017:27017
    environment: # environment variables
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    networks:
      - mongo-network
```

To start the container specified by the docker compose file, run `docker-compose -f mongodb.yaml up`. No matter how many services are specified in your docker compose file, as long as you configure the same networks name, one common docker network can be created so these services should be able to talk to each other. 

`docker-compose -f mongodb.yaml down` shuts all containers down and removes them. 

### Dockerfile

Dockerfile is the blue print to create a docker image. Let's memorize some important Dockerfile keywords. 

`FROM xxx:yyy` bases the image on some docker image `xxx` already exiting in Docker Hub. This applies like the base of your image. 

`ENV x=y xx=yy xxx=yyy` defines environment variables of your image. If you don't want to rebuild your image each time you update your environment variables, consider putting into docker compose file. 

`RUN any-linux-command` runs any linux command in your container. 

`COPY source-contents target-directory` copies `source-contents` *on the host*, to `target-directory` in the container. `RUN cp source-contents target-directory` copies `source-contents` *in the image*, to `target-directory` in the container.

`CMD ["xxx", "yyy"]` runs linux command inside the container which acts like an *entry command*. Mind that `yyy` is usually the path to some file in the container. 


Dockerfile is always named as `Dockerfile`. You can't change it to any other name!

### Build Image

`docker build -t xxx:yyy path-to-Dockerfile` builds your image `xxx` tagged as `yyy` based on a given Dockerfile. 

### Private Docker Registry

Image naming in Docker registries: `registryDomain/imageName:tag`. Note that the following two commands are the same.

1. `docker pull mongo:4.2`
2. `docker pull docker.io/library/mongo:4.2`

Usually before you push your own image to private repository, you need to rename your image with `docker tag xxx:yyy registryDomain/xxx:yyy`. `docker tag` makes a copy and renames. 

#### Amazon Elastic Container Registry

ECR saves different tags (versions) of the same image in one directory. 

Once authentication is done, run `docker push registryDomain/imageName:tag` to push your docker image to registry. 

### Docker Volumes: Data Persistence

Folder in physical host file system is mounted to the virtual file system in container. 

#### Host Volumes

`docker run -v host-file-system-path:virtual-file-system-path`. 

| volumes           | command                                                        | explanation                                          |
|-------------------|----------------------------------------------------------------|------------------------------------------------------|
| Host Volumes      | `docker run -v host-file-system-path:virtual-file-system-path` | mount host file path to container virtual file path  |
| Anonymous Volumes | `docker run -v virtual-file-system-path`                       | a folder is generated and mounted for each container |
| Named Volumes     | `docker run -v name:virtual-file-system-path`                  | can reference the volume by name                     |

Usually people just let Docker handle the mounting work with named volume. It's equivalent to specify volume in the docker compose file. 

```yaml
# mongodb.yaml
version: '3'
services:
  mongodb: # container name
    image: mongo:latest
    ports: 
      - 27017:27017
    volumes:
      - db-data:/var/lib/mysql/data # refer to db-data
    environment: # environment variables
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    networks:
      - mongo-network
```

Each time the container is started, data from `db-data` will be copied to `/var/lib/mysql/data` in the container. 