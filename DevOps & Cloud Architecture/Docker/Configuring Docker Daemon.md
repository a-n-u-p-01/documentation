## 1️⃣ **What is Docker Daemon?**

- The **Docker Daemon (`dockerd`)** is a background service that **runs on the host machine**.
    
- It is responsible for:
    
    - Building Docker images
        
    - Running containers
        
    - Managing Docker objects (images, containers, volumes, networks)
        
    - Handling Docker API requests from the Docker CLI or remote clients
        

**Key Points:**

- Daemon **listens for Docker API requests**.
    
- Works in client-server architecture:
    
    - **Docker Daemon (server)** → performs actions
        
    - **Docker Client (CLI)** → sends commands (`docker run`, `docker build`)
        

**Check if Docker Daemon is running:**

```bash
sudo systemctl status docker
```

---

## 2️⃣ **Docker Daemon Components**

|Component|Function|
|---|---|
|dockerd|Main Docker daemon process|
|Docker API|Interface for CLI / remote clients to communicate|
|Container runtime|Runs containers (default: `runc`)|
|Image storage|Manages images and layers|
|Network management|Handles bridge, overlay, and other networks|
|Volume management|Handles persistent storage|

---

## 3️⃣ **Docker Daemon Configuration**

Docker Daemon can be configured in multiple ways:

### a) **Daemon Configuration File**

- File location (depending on OS):
    
    - Linux: `/etc/docker/daemon.json`
        
    - Windows: `C:\ProgramData\Docker\config\daemon.json`
        

**Example:**

```json
{
  "debug": true,
  "data-root": "/mnt/docker-data",
  "log-level": "info",
  "storage-driver": "overlay2",
  "insecure-registries": ["myregistry.local:5000"]
}
```

**Explanation:**

- `"debug": true` → enable debug mode
    
- `"data-root"` → location to store images and containers
    
- `"log-level"` → set log verbosity
    
- `"storage-driver"` → filesystem driver for containers
    
- `"insecure-registries"` → allow non-HTTPS registries
    

---

### b) **Command-Line Options**

Docker Daemon can also be started with CLI options:

```bash
sudo dockerd --debug --storage-driver=overlay2 --insecure-registry myregistry.local:5000
```

**Note:** Options in CLI **override daemon.json**.

---

### c) **Environment Variables**

Docker daemon can be influenced by environment variables:

```bash
export DOCKER_OPTS="--debug --log-level=info"
```

- Often used in systemd service files or scripts.
    

---

## 4️⃣ **Common Docker Daemon Configurations**

|Setting|Purpose|
|---|---|
|debug|Enables detailed logs|
|data-root|Changes default location of Docker images & containers|
|storage-driver|Controls filesystem driver (overlay2, aufs, etc.)|
|log-driver|Defines logging driver (json-file, syslog, journald)|
|insecure-registries|Allows non-HTTPS registries|
|max-concurrent-downloads|Limits parallel image downloads|
|live-restore|Keeps containers running even when daemon restarts|

---

## 5️⃣ **Restarting Docker Daemon After Changes**

After editing `daemon.json` or configuration:

```bash
sudo systemctl restart docker
sudo systemctl status docker
```

Check if configurations are applied:

```bash
docker info
```

---

## 6️⃣ **Security Considerations**

- Only **root or users in `docker` group** can communicate with the daemon.
    
- Docker daemon runs with **high privileges**, so exposing it to the network can be risky.
    
- Always secure **remote API access** with TLS if needed.
    

---

## 7️⃣ **Troubleshooting Docker Daemon**

- Check daemon logs:
    

```bash
journalctl -u docker
```

- Restart daemon if container operations fail:
    

```bash
sudo systemctl restart docker
```

- Verify configuration:
    

```bash
docker info
```

---

## 8️⃣ **Summary**

|Aspect|Description|
|---|---|
|What is it?|Background service (`dockerd`) managing Docker|
|Role|Build images, run containers, manage networks & storage|
|Config file|`/etc/docker/daemon.json`|
|CLI config|`dockerd` options|
|Key settings|debug, storage-driver, data-root, log-level, insecure-registries|
|Security|Runs as root → restrict access|
|Restart|Required after config changes|

---