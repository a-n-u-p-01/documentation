### **Overview**

Docker is a platform to develop, ship, and run applications inside containers. Installing Docker correctly is the first step to start containerized development. The installation steps differ slightly depending on your OS.

---

## **1. System Requirements**

|OS|Requirements|
|---|---|
|Windows 10/11 Pro, Enterprise, or Education|Hyper-V or WSL2, 64-bit OS, virtualization enabled|
|MacOS|macOS 10.15 (Catalina) or later, 64-bit Intel or Apple Silicon|
|Linux|Modern 64-bit distro (Ubuntu, Debian, Fedora, CentOS), kernel ≥ 3.10|

---

## **2. Installation Steps**

### **Windows**

1. Enable **WSL 2** (Windows Subsystem for Linux):
    
    - Open PowerShell as Admin:
        
        ```powershell
        wsl --install
        ```
        
2. Install **Docker Desktop** from [Docker Hub](https://www.docker.com/products/docker-desktop/).
    
3. Follow installer prompts and ensure **Use WSL 2 instead of Hyper-V** is selected.
    
4. Verify installation:
    
    ```powershell
    docker --version
    docker run hello-world
    ```
    

---

### **MacOS**

1. Download **Docker Desktop for Mac** from [Docker Hub](https://www.docker.com/products/docker-desktop/).
    
2. Open `.dmg` file and drag Docker to **Applications**.
    
3. Launch Docker Desktop and follow setup instructions.
    
4. Verify installation:
    
    ```bash
    docker --version
    docker run hello-world
    ```
    

---

### **Linux (Ubuntu/Debian Example)**

1. Update package index:
    
    ```bash
    sudo apt-get update
    ```
    
2. Install dependencies:
    
    ```bash
    sudo apt-get install \
      ca-certificates \
      curl \
      gnupg \
      lsb-release
    ```
    
3. Add Docker’s official GPG key:
    
    ```bash
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```
    
4. Set up the repository:
    
    ```bash
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
    
5. Install Docker Engine:
    
    ```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
    
6. Verify installation:
    
    ```bash
    docker --version
    sudo docker run hello-world
    ```
    

---

## **3. Post-Installation Tips**

- **Windows:** Make sure WSL 2 integration is enabled for your Linux distributions in Docker Desktop settings.
    
- **Linux:** To run Docker without `sudo`, add your user to the `docker` group:
    
    ```bash
    sudo usermod -aG docker $USER
    ```
    
- **Mac:** Check for Apple Silicon vs Intel version compatibility.
    

---

## **4. Troubleshooting**

|Issue|Solution|
|---|---|
|`docker: command not found`|Ensure Docker is installed and PATH is updated|
|Permission denied (Linux)|Add user to `docker` group or run commands with `sudo`|
|WSL 2 not working (Windows)|Enable Virtualization in BIOS and install WSL 2 kernel update|

---

### **5. Verification**

Run:

```bash
docker --version
docker run hello-world
```

If you see a “Hello from Docker!” message, Docker is installed correctly.

---

I can also **write a condensed version ready for your YAML `docker_topics` frontmatter**, so it fits neatly in your **single-file dashboard** with all other topics.

Do you want me to do that?