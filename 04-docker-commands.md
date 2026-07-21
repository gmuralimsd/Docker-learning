Docker commands are used to interact with Docker, a popular platform for developing, shipping, and running applications using containerization. 
Here's a list of some useful Docker commands along with their usage and examples:

**Displays the installed Docker version.**
```
docker --version
```
***Displays system-wide information about Docker.***
```
docker info
```

1. **docker run**: this command is used to creat a containe without port mapping.
   ```
   docker run <image_name>
   ```
   Example: `docker run nginx`

### docker run -d -p 80:80 --name mynginx nginx

Runs a container from a Docker image.

```
- `-d`: Run container in detached mode.
- `-p 80:80`: Map port 80 on the host to port 80 in the container.
- `--name mynginx`: Assign a name to the container.
```

2. **docker pull**: Pull an images or a repository from a registry.
   ```
   docker pull <image_name>
   ```
   Example: `docker pull nginx`

3. **docker build**: Build an image from a Dockerfile.
   ```
   docker build -t <image_name> <path_to_Dockerfile>
   
   ```
   Example: `docker build -t my_image .`

4. **docker ps**: List running containers.
   ```
   docker ps
   ```
   Example: `docker ps`


   **docker ps -a**: List running containers and stoped containers 
     ```
     docker ps
      ```
   
5. **docker images**: List images.
   ```
   docker images
   ```
   Example: `docker images`

6. **docker stop**: Stop one or more running containers.
   ```
   docker stop <container_id>
   ```
   Example: `docker stop abc123def456`

7. **docker rm**: Remove one or more containers.
   ```
   docker rm <container_id>
   ```
   Example: `docker rm abc123def456`

8. **docker rmi**: Remove one or more images.
   ```
   docker rmi <image_name>
   ```
   Example: `docker rmi my_image`
   
   **use this command for delete all stoped containers and images**
   ```
   docker system prune --all
   
   ```

10. **docker exec**: Run a command in a running container.
   ```
   docker exec -it <container_id> <command>

   ```
   Example: `docker exec -it abc123def456 bash`

11. **docker-compose**: Define and run multi-container Docker applications with a YAML file.
   ```
   docker-compose up
   ```
   Example: `docker-compose up`

11. **docker logs**: Fetch the logs of a container.
   ```
   docker logs <container_id>

   ```
   Example: `docker logs abc123def456`

12. **docker inspect**: Return low-level information on Docker objects.
   ```
   docker inspect <object_id>


====================================================

Common Docker commands along with examples to help you understand their usage:

### 1. `docker --version`
Displays the installed Docker version.

docker --version


### 2. `docker info`
Displays system-wide information about Docker.

docker info


### 3. `docker pull`
Pulls an image from a Docker registry (e.g., Docker Hub).

docker pull nginx


### 4. `docker run`
Runs a container from a Docker image.

docker run -d -p 80:80 --name mynginx nginx

- `-d`: Run container in detached mode.
- `-p 80:80`: Map port 80 on the host to port 80 in the container.
- `--name mynginx`: Assign a name to the container.

### 5. `docker ps`
Lists running containers.

docker ps

- `docker ps -a`: Lists all containers, including stopped ones.

### 6. `docker stop`
Stops a running container.

docker stop mynginx


### 7. `docker start`
Starts a stopped container.

docker start mynginx


### 8. `docker restart`
Restarts a running container.

docker restart mynginx


### 9. `docker rm`
Removes a stopped container.

docker rm mynginx


### 10. `docker rmi`
Removes a Docker image.

docker rmi nginx


### 11. `docker images`
Lists all Docker images on the local system.

docker images


### 12. `docker logs`
Fetches logs of a container.

docker logs mynginx


### 13. `docker exec`
Runs a command in a running container.

docker exec -it mynginx /bin/bash

- `-it`: Runs in interactive mode with a terminal.

### 14. `docker build`
Builds an image from a Dockerfile.

docker build -t myapp:latest .

- `-t myapp:latest`: Tags the image with the name `myapp` and tag `latest`.
- `.`: Context path (current directory).

### 15. `docker-compose up`
Starts services defined in a `docker-compose.yml` file.

docker-compose up

- `docker-compose up -d`: Runs in detached mode.

### 16. `docker-compose down`
Stops and removes containers, networks, volumes, and images created by `docker-compose up`.

docker-compose down


### 17. `docker network ls`
Lists all Docker networks.

docker network ls


### 18. `docker network create`
Creates a new Docker network.

docker network create mynetwork


### 19. `docker volume ls`
Lists all Docker volumes.

docker volume ls


### 20. `docker volume create`
Creates a new Docker volume.

docker volume create myvolume


### Example Workflow

1. Pull an image:
   
   docker pull nginx
   

2. Run a container:
   
   docker run -d -p 8080:80 --name webserver nginx
   

3. List running containers:
   
   docker ps
   

4. Check logs of the container:
   
   docker logs webserver
   

5. Execute a command inside the container:
   
   docker exec -it webserver /bin/bash
   

6. Stop the container:
   
   docker stop webserver
   

7. Start the container again:
   
   docker start webserver
   

8. Remove the container:
   
   docker rm webserver
   

9. Remove the image:
   
   docker rmi nginx
   

These commands cover the basics of using Docker for managing images, containers, networks, and volumes, and provide a solid foundation for more advanced Docker usage.
   ```
   Example: `docker inspect abc123def456`

These are just a few commonly used Docker commands. There are many more commands and options available, which you can explore further in the Docker documentation or by using the `docker --help` command.

---
---


# Docker Commands

## Check Docker

*Check Docker Version*
```bash
docker --version
```
**Purpose:** Check whether Docker is installed.

### Docker Information
```bash
docker info
```
**Purpose:** Display Docker system information.

### Docker Help
```bash
docker help
```
**Purpose:** Show all Docker commands.

---

# Docker Images

## Pull an Image
```bash
docker pull nginx
```
**Purpose:** Download the Nginx image from Docker Hub.

## List Images
```bash
docker images
```
**Purpose:** Display all downloaded Docker images.

## Remove an Image
```bash
docker rmi nginx
```
**Purpose:** Delete the specified image.

## Search an Image
```bash
docker search ubuntu
```
**Purpose:** Search images on Docker Hub.

---

# Docker Containers

## Run a Container
```bash
docker run nginx
```
**Purpose:** Create and start a container.

## Run in Background
```bash
docker run -d nginx
```
**Purpose:** Run the container in detached mode.

## Run with a Name
```bash
docker run -d --name webserver nginx
```
**Purpose:** Start a container with a custom name.

## Port Mapping
```bash
docker run -d -p 8080:80 nginx
```
**Purpose:** Map host port 8080 to container port 80.

## Interactive Mode
```bash
docker run -it ubuntu bash
```
**Purpose:** Open a Bash terminal inside the Ubuntu container.

---

# View Containers

## Running Containers
```bash
docker ps
```
**Purpose:** Show running containers.

## All Containers
```bash
docker ps -a
```
**Purpose:** Show all containers (running and stopped).

---

# Start, Stop & Restart

## Start
```bash
docker start webserver
```

## Stop
```bash
docker stop webserver
```

## Restart
```bash
docker restart webserver
```

## Pause
```bash
docker pause webserver
```

## Resume
```bash
docker unpause webserver
```

## Kill
```bash
docker kill webserver
```

---

# Access a Running Container

## Open Bash
```bash
docker exec -it webserver bash
```

## Open Shell
```bash
docker exec -it webserver sh
```

## Exit
```bash
exit
```

---

# Logs

## Show Logs
```bash
docker logs webserver
```

## Live Logs
```bash
docker logs -f webserver
```

---

# Inspect

## Inspect Container
```bash
docker inspect webserver
```

---

# Copy Files

## Host to Container
```bash
docker cp file.txt webserver:/tmp
```

## Container to Host
```bash
docker cp webserver:/tmp/file.txt .
```

---

# Remove Containers

## Remove Container
```bash
docker rm webserver
```

## Force Remove
```bash
docker rm -f webserver
```

---

# Build Images

## Build Image
```bash
docker build -t myapp .
```

## List Images
```bash
docker images
```

---

# Docker Hub

## Login
```bash
docker login
```

## Push Image
```bash
docker push username/myapp
```

## Logout
```bash
docker logout
```

---

# Docker Volumes

## Create Volume
```bash
docker volume create myvolume
```

## List Volumes
```bash
docker volume ls
```

## Inspect Volume
```bash
docker volume inspect myvolume
```

## Remove Volume
```bash
docker volume rm myvolume
```

---

# Docker Networks

## List Networks
```bash
docker network ls
```

## Create Network
```bash
docker network create mynetwork
```

## Connect Container
```bash
docker network connect mynetwork webserver
```

## Inspect Network
```bash
docker network inspect mynetwork
```

## Remove Network
```bash
docker network rm mynetwork
```

---

# Cleanup

## Remove Unused Containers
```bash
docker container prune
```

## Remove Unused Images
```bash
docker image prune
```

## Remove All Unused Resources
```bash
docker system prune -a
```

---

# Important Docker Commands
```
1. docker pull
2. docker run
3. docker ps
4. docker ps -a
5. docker images
6. docker exec
7. docker logs
8. docker stop
9. docker start
10. docker restart
11. docker rm
12. docker rmi
13. docker build
14. docker push
15. docker login
16. docker volume
17. docker network
18. docker inspect
19. docker cp
20. docker system prune
```
