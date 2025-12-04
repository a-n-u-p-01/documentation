### **Overview**

The Docker Command-Line Interface (CLI) is the primary way to interact with Docker. It allows you to manage images, containers, networks, and volumes from the terminal. Understanding the CLI is essential for day-to-day container management.

---

## **1. Docker CLI Structure**

```
docker [COMMAND] [OPTIONS] [ARGUMENTS]
```

- **COMMAND** → what you want Docker to do (run, build, ps, etc.)
    
- **OPTIONS** → optional flags (`-d`, `--name`, `-p`)
    
- **ARGUMENTS** → specific target (image name, container ID, file path)
    

---

## **2. Common Docker Commands**

### **Container Management**

| Command                                | Description                                |
| -------------------------------------- | ------------------------------------------ |
| `docker run [OPTIONS] IMAGE [COMMAND]` | Create and start a container from an image |
| `docker ps`                            | List running containers                    |
| `docker ps -a`                         | List all containers (including stopped)    |
| `docker stop CONTAINER`                | Stop a running container                   |
| `docker start CONTAINER`               | Start a stopped container                  |
| `docker restart CONTAINER`             | Restart a container                        |
| `docker rm CONTAINER`                  | Remove a container                         |

---

### **Image Management**

|Command|Description|
|---|---|
|`docker images`|List local Docker images|
|`docker pull IMAGE`|Download an image from Docker Hub|
|`docker rmi IMAGE`|Remove a local image|
|`docker build -t NAME .`|Build an image from a Dockerfile in current directory|

---

### **Logs & Monitoring**

|Command|Description|
|---|---|
|`docker logs CONTAINER`|View container logs|
|`docker exec -it CONTAINER bash`|Open an interactive shell inside a running container|
|`docker stats`|Show live resource usage for containers|

---

### **Networking & Ports**

- Map container ports to host ports:
    

```bash
docker run -p 8080:80 IMAGE
```

- `8080` → host port, `80` → container port
    

---

### **Volumes & Data**

- Bind mount a host directory:
    

```bash
docker run -v /host/path:/container/path IMAGE
```

- Named volume:
    

```bash
docker volume create mydata
docker run -v mydata:/container/path IMAGE
```

---

### **3. Tips for Using Docker CLI**

- Use `--rm` to automatically remove a container after it stops:
    

```bash
docker run --rm IMAGE
```

- Use `-d` to run a container in the background (detached mode):
    

```bash
docker run -d IMAGE
```

- Always give containers descriptive names with `--name`
    

---

### **4. Verification & Practice**

Try the following commands to familiarize yourself with CLI basics:

```bash
docker run hello-world
docker ps -a
docker images
docker exec -it <container_id> bash
```

---