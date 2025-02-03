# Docker Learning Guide

This guide covers Docker from beginner to advanced levels with explanations and commands.

---

## 1. Introduction to Docker
### What is Docker?
Docker is an open-source platform that enables developers to automate the deployment of applications inside lightweight, portable containers.

### Benefits of Docker
- Consistency across environments
- Fast application deployment
- Efficient resource utilization
- Simplifies dependency management

### Virtual Machines vs. Docker
| Feature | Virtual Machine | Docker |
|---------|---------------|--------|
| Startup Time | Slow | Fast |
| Performance | Uses more resources | Lightweight |
| Isolation | Full OS | Process-level |

---

## 1. Docker Basics
### Understanding Images and Containers
- **Images**: Blueprint for containers (e.g., Ubuntu, Node.js)
- **Containers**: Running instances of images

### Running Your First Container
```sh
docker run hello-world
```

### Managing Containers
```sh
# List running containers
docker ps
docker container

# List all containers
docker ps -a
docker container

docker start <ID>  # Start a container
docker stop <ID>   # Stop a container
docker rm <ID>     # Remove a container
```

### Port Mapping
```
docker run -it -p 6000:6000 <ImageName>
```

### Environment Variable
```sh
docker run -it -e key = value -p 6000:6000 <ImageName>
```

### Start Container in terminel
```sh
docker exec -it <container_id> bash
```

### Working with Docker Images
```sh
docker pull nginx  # Download an image
docker images      # List all images
docker rmi <ID>    # Remove an image
docker tag nginx mynginx:v1  # Tag an image
```

### Docker Hub and Private Registries
```sh
docker login       # Login to Docker Hub
docker push myimage:latest  # Push image to Docker Hub
```

---

## 2. Docker Networking
### Network Modes
- **Bridge** (Default): Containers can communicate via a private network
- **Host**: Uses the host network
- **None**: No network access

### Creating a Custom Network
```sh
docker network create mynetwork
docker network ls      # List networks
docker network rm mynetwork  # Remove a network
```

### Connecting Containers via Networks
```sh
# Connect your container to your network. So no need for port mapping.
docker run -it --network = host <ImageName>
```

### Connecting Containers via Networks
```sh
docker network connect mynetwork container1
docker network disconnect mynetwork container1
```

---

## 3. Docker Volumes & Storage
### Understanding Volumes
Volumes persist data outside the container lifecycle.

### Creating and Using Volumes
```sh
docker volume create myvolume
docker volume ls         # List volumes
docker run -v myvolume:/data ubuntu
docker volume inspect myvolume
docker volume rm myvolume
```

---

## 4. Docker Compose
### Writing a `docker-compose.yml` File
```yaml
version: '3'
services:
  app:
    image: node:14
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    networks:
      - mynetwork
networks:
  mynetwork:
```
Start services:
```sh
docker-compose up -d
docker-compose down
```

---

## 5. Dockerfile & Image Creation
### Writing a Dockerfile
```Dockerfile
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "app.js"]
```
Build and Run:
```sh
docker build -t myapp .
docker run -p 3000:3000 myapp
```

---

## 6. Advanced Docker Concepts
### Multi-Stage Builds
```Dockerfile
FROM node:14 as builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx
COPY --from=builder /app/dist /usr/share/nginx/html
```

### Docker BuildKit
```sh
DOCKER_BUILDKIT=1 docker build -t myapp .
```

---

## 7. Docker Swarm & Orchestration
```sh
docker swarm init
docker node ls
docker service create --name web -p 80:80 nginx
docker service ls
```

---

## 8. Kubernetes (K8s) with Docker
### Running Docker Containers on Kubernetes
```sh
kubectl apply -f deployment.yaml
kubectl get pods
kubectl delete pod mypod
```

---

## 9. CI/CD with Docker
### Using Docker in GitHub Actions
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Docker Image
        run: docker build -t myapp .
```

---

## 10. Monitoring & Logging
### Checking Logs
```sh
docker logs <container_id>
docker stats
```

---

## 11. Docker Security
### Scanning Images for Vulnerabilities
```sh
docker scan myimage
```

---

## 12. Docker in Production
### Optimizing Performance
```sh
docker system prune -a
docker build --no-cache -t myapp .
```

---

## 13. Troubleshooting & Debugging
### Debugging Containers
```sh
docker exec -it <container_id> sh
docker inspect <container_id>
docker logs <container_id>
```

---


## Resources
- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)

 🐳
