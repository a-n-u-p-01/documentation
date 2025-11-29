## **1. Docker Images**

**Definition:**  
A **Docker image** is a **read-only template** used to create containers. It contains everything needed to run an application—code, libraries, environment variables, and dependencies.

**Key Points:**

- Images are **immutable** (cannot be changed once created).
    
- They are built using a **Dockerfile** (set of instructions).
    
- Images are stored in **layers** to save space and speed up builds.
    
- Can be downloaded from **Docker Hub** or private registries.
    
- Multiple containers can run from the same image.
    

**Common Commands:**

```bash
docker pull <image>        # download image
docker images              # list available images
docker rmi <image>         # remove image
docker build -t name .     # build image from Dockerfile
```

**Example:**  
`nginx`, `ubuntu`, `python:3.11` are common Docker images.

---

## **2. Docker Containers**

**Definition:**  
A **Docker container** is a **running instance** of an image.  
It is lightweight, isolated, and includes everything needed to run an application.

**Key Points:**

- Containers are **mutable**—they can run, stop, restart, and be removed.
    
- They are isolated from the host and other containers using namespaces and cgroups.
    
- Containers can expose ports, persist data, and connect to networks.
    
- When a container is created, Docker uses the image and adds a **thin writable layer** on top.
    

**Container Lifecycle:**

1. Create
    
2. Start
    
3. Run
    
4. Stop
    
5. Remove
    

**Common Commands:**

```bash
docker run <image>        # create + start container
docker ps                 # list running containers
docker stop <container>   # stop container
docker rm <container>     # remove container
```

---

## **Images vs. Containers – Quick Comparison**

|Feature|Docker Image|Docker Container|
|---|---|---|
|Type|Blueprint/Template|Running instance|
|State|Read-only|Read/Write|
|Purpose|Define environment & app|Execute the application|
|Location|Local machine or registry|Runs on Docker Engine|
|Lifecycle|Build → Store → Push|Run → Stop → Restart → Remove|

---

## **Simple Example**

```bash
docker pull ubuntu             # get image
docker run -it ubuntu bash     # create and run container
docker ps                      # see container running
docker stop <id>               # stop it
docker rm <id>                 # remove it
```

---
