## 1. **`docker run`**

**Function:**  
Creates a new container from an image **and starts it**.

**Key Points:**

- If the image is not present locally, Docker **pulls it from Docker Hub**.
    
- You can run containers in the foreground (default) or background (`-d`).
    
- Common options:
    
    - `-d` → run detached
        
    - `-p host:container` → publish ports
        
    - `--name <name>` → assign container name
        
    - `-it` → interactive terminal
        

**Examples:**

```bash
docker run nginx
docker run -d -p 8080:80 --name web nginx
docker run -it ubuntu bash
```

---

## 2. **`docker ps`**

**Function:**  
Displays information about containers.

**Key Points:**

- By default **only running** containers are shown.
    
- Useful for checking container status, IDs, ports, and names.
    
- Use `-a` to see **all containers**, including stopped ones.
    

**Examples:**

```bash
docker ps            # running containers
docker ps -a         # all containers
```

---

## 3. **`docker stop`**

**Function:**  
Gracefully stops a running container.

**Key Points:**

- Sends a **SIGTERM** signal to allow safe shutdown.
    
- After a timeout (default: 10s), Docker sends **SIGKILL**.
    
- Needs container **ID or name**.
    

**Examples:**

```bash
docker stop web
docker stop 3d2f1a9c4b
```

---

## 4. **`docker rm`**

**Function:**  
Removes a **stopped** container permanently.

**Key Points:**

- Containers must be stopped before removal (unless `-f` is used).
    
- Frees disk space and cleans old containers.
    
- Can remove multiple containers at once.
    

**Examples:**

```bash
docker rm web
docker rm 3d2f1a9c4b d5e4f9
docker rm -f mycontainer   # force remove even if running
```

---

# 🔑 **Summary Table**

|Command|Purpose|Important Options|
|---|---|---|
|`docker run`|Create + start a container|`-d`, `-p`, `--name`, `-it`|
|`docker ps`|List containers|`-a`|
|`docker stop`|Stop a running container|—|
|`docker rm`|Remove a stopped container|`-f`, multiple IDs|
