# Docker Commands Reference Guide

## Container Lifecycle Commands

### Running Containers
```bash
# Run a container
docker run [OPTIONS] IMAGE [COMMAND]
  -d, --detach          # Run in background
  -p, --publish         # Map port (host:container)
  -v, --volume         # Bind mount a volume
  --name               # Assign name to container
  -e, --env           # Set environment variables
  --rm                # Remove container when it exits
  -it                 # Interactive terminal

# Examples
docker run -d nginx                     # Run nginx in background
docker run -p 8080:80 nginx            # Map port 8080 to container's 80
docker run -it ubuntu bash             # Run ubuntu with interactive shell
```

### Managing Running Containers
```bash
# List containers
docker ps                  # List running containers
docker ps -a              # List all containers

# Container operations
docker start CONTAINER    # Start stopped container
docker stop CONTAINER     # Stop running container
docker restart CONTAINER  # Restart container
docker pause CONTAINER    # Pause container processes
docker unpause CONTAINER  # Unpause container processes
docker rm CONTAINER      # Remove container
docker rm -f CONTAINER   # Force remove running container
```

## Image Management

### Basic Image Operations
```bash
# List and pull images
docker images            # List all images
docker pull IMAGE        # Pull image from registry
docker rmi IMAGE        # Remove image
docker image prune      # Remove unused images

# Build images
docker build [OPTIONS] PATH
  -t, --tag            # Name and tag the image
  -f, --file          # Specify Dockerfile location
  --no-cache          # Build without cache

# Example
docker build -t myapp:1.0 .
```

### Image Information
```bash
docker inspect IMAGE    # Display detailed image info
docker history IMAGE   # Show image layer history
docker tag SOURCE TARGET  # Create tag TARGET from SOURCE
```

## Network Commands

### Network Management
```bash
# List and create networks
docker network ls                # List networks
docker network create NETWORK   # Create network
docker network rm NETWORK      # Remove network

# Connect/disconnect containers
docker network connect NETWORK CONTAINER    # Connect container to network
docker network disconnect NETWORK CONTAINER # Disconnect container
```

## Volume Commands

### Volume Operations
```bash
# Volume management
docker volume ls                # List volumes
docker volume create VOLUME    # Create volume
docker volume rm VOLUME       # Remove volume
docker volume prune          # Remove unused volumes
```

## Container Interaction

### Executing Commands
```bash
# Execute command in running container
docker exec [OPTIONS] CONTAINER COMMAND
  -i, --interactive    # Keep STDIN open
  -t, --tty           # Allocate pseudo-TTY

# Example
docker exec -it CONTAINER bash  # Start shell in container
```

### Logs and Monitoring
```bash
# View container logs
docker logs [OPTIONS] CONTAINER
  -f, --follow         # Follow log output
  --tail N            # Show last N lines
  --since TIMESTAMP   # Show logs since timestamp

# Monitor container metrics
docker stats [CONTAINER]  # Display container resource usage
```

## Docker Compose Commands

### Basic Compose Operations
```bash
# Compose management
docker-compose up     # Create and start containers
docker-compose down   # Stop and remove containers
docker-compose ps     # List containers
docker-compose logs   # View output from containers

# Service operations
docker-compose start  # Start services
docker-compose stop   # Stop services
docker-compose restart # Restart services
```

## System Commands

### Docker System Management
```bash
# System information
docker info           # Display system-wide information
docker version        # Show Docker version
docker system df     # Show Docker disk usage

# Clean up
docker system prune  # Remove unused data
  -a, --all         # Remove all unused images
  --volumes         # Remove unused volumes
```

## Best Practices

1. Always tag your images with specific versions instead of using `latest`
2. Use named volumes instead of bind mounts when possible
3. Clean up unused containers, images, and volumes regularly
4. Use multi-stage builds to create smaller production images
5. Never store sensitive data in images
6. Use `.dockerignore` to exclude unnecessary files
7. Set resource limits for containers in production

## Security Notes

1. Run containers with `--read-only` when possible
2. Use user namespaces to limit container privileges
3. Regularly update base images
4. Scan images for vulnerabilities
5. Avoid running containers with `--privileged`

## Troubleshooting

1. Check logs using `docker logs CONTAINER`
2. Inspect container details with `docker inspect CONTAINER`
3. Monitor resource usage with `docker stats`
4. Use `docker events` to track Docker daemon events
5. Enable debug mode with `docker --debug`
