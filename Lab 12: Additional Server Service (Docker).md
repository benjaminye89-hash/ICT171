# Lab 12: Additional Server Service (Docker)

**Date:** 15/07/2026  
**Duration:** 2 hours  
**Status:** Complete  

---

## 🎯 Objective
To install, configure, and test an additional server service of my choice. I selected **Docker** – a containerization platform that allows applications to run in isolated environments called containers, making deployment faster and more consistent across different systems.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- Docker Engine (Community Edition)
- Docker Hub (for pulling images)
- `curl`, `apt`, `systemctl`

---

## 📋 Step-by-Step Process

### 1. Understood Docker Concepts
I reviewed the basics of Docker and containerization:

| Term | Definition |
|------|------------|
| **Container** | A lightweight, standalone, executable package that includes everything needed to run software |
| **Image** | A read-only template used to create containers (like a class blueprint) |
| **Docker Hub** | A public registry of Docker images (like an app store for containers) |
| **Dockerfile** | A text file with instructions for building a Docker image |
| **Volume** | A persistent data storage mechanism for containers |
| **Port Mapping** | Exposing a container's port to the host machine |

### 2. Installed Docker
I installed Docker using the official installation script:

```bash
# Update package list
sudo apt update

# Install prerequisites
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker
3. Verified Docker Installation
I verified Docker was installed and running:

bash
# Check Docker version
docker --version

# Check Docker service status
sudo systemctl status docker

# Run hello-world container
sudo docker run hello-world
Output:

text
Hello from Docker!
This message shows that your installation appears to be working correctly.
4. Added User to Docker Group
I added myself to the docker group to avoid using sudo for every command:

bash
sudo usermod -aG docker $USER
newgrp docker
5. Pulled and Ran an Nginx Container
I pulled the official Nginx image and ran a container:

bash
# Pull the image
docker pull nginx:latest

# Run the container with port mapping
docker run -d --name my-nginx -p 8080:80 nginx:latest

# Verify container is running
docker ps
Output:

text
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                  NAMES
abc123def456   nginx:latest   "/docker-entrypoint.…"   5 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp   my-nginx
6. Created a Custom Webpage in Docker
I created a custom webpage inside the container:

bash
# Create a custom index.html file
cat > index.html << EOF
<!DOCTYPE html>
<html>
<head><title>ICT171 Docker Lab</title></head>
<body>
<h1>🚀 Docker is running!</h1>
<p>This page is served from a Docker container.</p>
<p>Hostname: $(hostname)</p>
<p>Date: $(date)</p>
</body>
</html>
EOF

# Copy the file into the container
docker cp index.html my-nginx:/usr/share/nginx/html/

# Test in browser: http://localhost:8080
7. Pulled and Ran a MySQL Container
I ran a MySQL database container:

bash
# Pull MySQL image
docker pull mysql:8.0

# Run MySQL container with environment variables
docker run -d --name my-mysql \
  -e MYSQL_ROOT_PASSWORD=ict171pass \
  -e MYSQL_DATABASE=testdb \
  -e MYSQL_USER=ictuser \
  -e MYSQL_PASSWORD=ictpass \
  -p 3306:3306 \
  mysql:8.0

# Verify MySQL is running
docker ps | grep mysql
8. Created a Docker Compose File
I created a Docker Compose file to run multiple services together:

yaml
# docker-compose.yml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    depends_on:
      - db
    networks:
      - app-network

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: myapp
      MYSQL_USER: appuser
      MYSQL_PASSWORD: apppass
    ports:
      - "3306:3306"
    networks:
      - app-network
    volumes:
      - db-data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
      PMA_USER: root
      PMA_PASSWORD: rootpass
    depends_on:
      - db
    networks:
      - app-network

networks:
  app-network:

volumes:
  db-data:
9. Ran Multi-Service Setup with Docker Compose
I started the multi-service environment:

bash
# Install Docker Compose
sudo apt install docker-compose -y

# Start the services
docker-compose up -d

# Verify all services are running
docker-compose ps
Result: Nginx, MySQL, and phpMyAdmin were all running and interconnected.

10. Tested the Containerized Environment
I accessed each service:

Nginx: http://localhost:8080 – showed custom webpage

phpMyAdmin: http://localhost:8081 – logged into MySQL

11. Performed Container Management
I practiced managing containers:

bash
# List containers
docker ps -a

# Stop a container
docker stop my-nginx

# Start a container
docker start my-nginx

# Remove a container
docker rm my-nginx

# View logs
docker logs my-nginx

# View running processes in container
docker exec my-nginx ps aux
12. Explored Docker Volumes
I inspected persistent storage:

bash
# List volumes
docker volume ls

# Inspect a volume
docker volume inspect db-data

# Create a volume
docker volume create my_volume

# Run container with volume mount
docker run -d --name my-app -v my_volume:/app/data nginx:latest

⚠️ Errors Faced
Error 1: Permission Denied for Docker Commands
Error Message: Got permission denied while trying to connect to the Docker daemon socket

Why it happened: The user was not in the docker group.

How I fixed it: Added user to the docker group:

bash
sudo usermod -aG docker $USER
newgrp docker
What I learned: Docker requires group membership to run without sudo.

Error 2: Port Already in Use
Error Message: Bind for 0.0.0.0:8080 failed: port is already allocated

Why it happened: The port was already used by another container or service.

How I fixed it: Changed the host port mapping or stopped the existing container:

bash
docker stop my-nginx
docker run -d --name my-nginx -p 8081:80 nginx:latest
What I learned: Ports can only be used by one container at a time.

Error 3: Image Pull Failed
Error Message: Error response from daemon: pull access denied

Why it happened: The image name was misspelled or does not exist.

How I fixed it: Verified the correct image name and tag:

bash
docker search mysql
docker pull mysql:8.0
What I learned: Always verify image names on Docker Hub before pulling.

Error 4: Container Exits Immediately
Error Message: (Container status shows Exited (0))

Why it happened: The container process finished or crashed.

How I fixed it: Checked logs to diagnose:

bash
docker logs my-container
For Nginx, ensured the container was running in detached mode (-d).

What I learned: Use docker logs to debug container issues.

🧠 Reflection
What I Learned
Docker simplifies application deployment by packaging code and dependencies together.

Containers are lightweight and start much faster than virtual machines.

Docker Compose manages multi-container applications with a single YAML file.

Port mapping exposes container ports to the host machine.

Volumes provide persistent storage for containers.

Docker Hub contains thousands of pre-built images for various services.

Key Commands Summary
Command	Purpose
docker --version	Check Docker version
docker pull IMAGE	Download an image from Docker Hub
docker run -d --name NAME -p HOST:CONTAINER IMAGE	Run a container in detached mode
docker ps	List running containers
docker ps -a	List all containers (including stopped)
docker stop CONTAINER	Stop a container
docker start CONTAINER	Start a container
docker rm CONTAINER	Remove a container
docker logs CONTAINER	View container logs
docker exec CONTAINER COMMAND	Execute a command in a running container
docker volume ls	List Docker volumes
docker-compose up -d	Start services defined in docker-compose.yml
docker-compose down	Stop and remove services
Real-World Application
Developers use Docker to ensure their applications run the same in development, testing, and production.

DevOps engineers containerize microservices for scalability and manage them with orchestrators like Kubernetes.

Docker Compose is widely used for local development environments (e.g., LAMP/LEMP stacks).

CI/CD pipelines use Docker for building, testing, and deploying applications.


