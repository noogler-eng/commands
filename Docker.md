# Docker Useful Commands

## Docker Basics

### 1. **Check Docker version**
To check the Docker version installed on your machine, run:
docker --version
2. Run a Docker container
Run a container using an image:

docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
Example:

docker run -d -p 80:80 nginx
3. List running containers
To view all running containers:

docker ps
4. List all containers (including stopped ones)
To view all containers, including the ones that are stopped:

docker ps -a
5. Stop a running container
To stop a running container:

docker stop <container_id>
6. Start a stopped container
To start a container that is stopped:

docker start <container_id>
7. Remove a container
To remove a container:

docker rm <container_id>
8. List all images
To view all the available images:

docker images
9. Remove a Docker image
To remove a Docker image:

docker rmi <image_id>
10. Build an image from a Dockerfile
To build an image from a Dockerfile in the current directory:

docker build -t <image_name>:<tag> .
11. Run a container in interactive mode
To run a container interactively:

docker run -it <image_name> /bin/bash
12. View container logs
To view the logs of a container:

docker logs <container_id>
13. View resource usage of containers
To view the resource usage of running containers:

docker stats
14. Show detailed information about a container
To inspect a container and get detailed information:

docker inspect <container_id>
15. Pull an image from Docker Hub
To pull an image from Docker Hub:

docker pull <image_name>
Docker Compose (for multi-container applications)

16. Start services defined in docker-compose.yml
To start all services defined in the docker-compose.yml file:

docker-compose up
17. Start services in detached mode
To start services in detached mode (run in the background):

docker-compose up -d
18. Stop services defined in docker-compose.yml
To stop all running services:

docker-compose down
19. Build images defined in docker-compose.yml
To build all images defined in the docker-compose.yml file:

docker-compose build
20. View logs from all containers in a Compose project
To view the logs from all containers in a Docker Compose project:

docker-compose logs
