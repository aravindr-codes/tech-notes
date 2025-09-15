# 🐳 Docker Quick Reference

## 📌 Basics
- `docker --version` → Show Docker version  
- `docker info` → Display system-wide information  
- `docker help` → Get help for Docker commands  

---

## 📦 Images
- `docker pull <image>` → Download an image from Docker Hub  
- `docker images` → List all local images  
- `docker rmi <image_id>` → Remove an image  
- `docker build -t myapp .` → Build an image from Dockerfile  

---

## 🚀 Containers
- `docker run hello-world` → Run a test container  
- `docker run -it ubuntu bash` → Run container interactively  
- `docker run -d -p 8080:80 nginx` → Run in detached mode with port mapping  
- `docker ps` → List running containers  
- `docker ps -a` → List all containers (including stopped)  
- `docker stop <container_id>` → Stop a container  
- `docker start <container_id>` → Start a stopped container  
- `docker restart <container_id>` → Restart a container  
- `docker rm <container_id>` → Remove a container  

---

## 📂 Volumes & Files
- `docker volume ls` → List volumes  
- `docker volume rm <volume_name>` → Remove a volume  
- `docker run -v /host/path:/container/path nginx` → Mount volume  
- `docker cp <container_id>:/file.txt ./` → Copy file from container  

---

## 📡 Networks
- `docker network ls` → List networks  
- `docker network create mynetwork` → Create a network  
- `docker network connect mynetwork <container>` → Attach container to a network  

---

## 🛠️ Exec & Logs
- `docker exec -it <container_id> bash` → Open a shell inside container  
- `docker logs <container_id>` → Show container logs  
- `docker logs -f <container_id>` → Follow logs in real-time  

---

## 🧹 Cleanup
- `docker system prune` → Remove unused data (images, containers, volumes)  
- `docker image prune` → Remove unused images  
- `docker container prune` → Remove stopped containers  

---

## 📚 References
- [Docker CLI Docs](https://docs.docker.com/engine/reference/commandline/docker/)  
- [Docker Cheat Sheet (PDF)](https://docs.docker.com/get-started/docker_cheatsheet.pdf)  
