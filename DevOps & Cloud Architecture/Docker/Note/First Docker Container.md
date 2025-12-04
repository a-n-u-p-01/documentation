## **1. What is a Docker container?**

A container is:

- a running instance of a Docker image
    
- isolated from the host system
    
- lightweight because it shares the host OS kernel
    
- portable and reproducible
    

---

## **2. Pulling an Image**

Before running a container, Docker will pull an image from a registry (default: Docker Hub).

Example:

```bash
docker pull nginx
```

If you run a container with an image you don’t have, Docker will pull it automatically.

---

## **3. Running Your First Container**

Basic run command:

```bash
docker run nginx
```

This:

- downloads `nginx` image (if not available locally)
    
- starts a container from it
    

But this container stops immediately because **nginx needs the foreground process**.

So we run it with options:

---

## **4. Running Container in Detached Mode**

```bash
docker run -d nginx
```

`-d` = detached, meaning it runs in the background.

---

## **5. Exposing Container Ports**

Containers have their own internal network.  
To access something (e.g., web server), you map ports.

```bash
docker run -d -p 8080:80 nginx
```

This maps:

- host port **8080**
    
- to container port **80**
    

Check by visiting:  
`http://localhost:8080`

---

## **6. Listing Running Containers**

```bash
docker ps
```

Shows:

- container ID
    
- image name
    
- ports
    
- status
    

To list **all** containers (including stopped):

```bash
docker ps -a
```

---

## **7. Stopping a Container**

```bash
docker stop <container-id>
```

---

## **8. Removing a Container**

```bash
docker rm <container-id>
```

If a container is running, stop it first.

---

## **9. Useful Commands During First Run**

### **Container logs**

```bash
docker logs <container-id>
```

### **Get a shell inside container**

```bash
docker exec -it <container-id> bash
```

(If the image does not contain bash, use `sh`.)

### **Check image list**

```bash
docker images
```

---

## **10. Summary Flow**

1. Pull an image → `docker pull nginx`
    
2. Run a container → `docker run -d -p 8080:80 nginx`
    
3. Check running containers → `docker ps`
    
4. View logs → `docker logs <id>`
    
5. Stop → `docker stop <id>`
    
6. Remove → `docker rm <id>`
    

This completes the **first Docker container workflow**.

---
