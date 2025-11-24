A **custom Docker image** is an image you create based on your application’s needs.  
It contains:

- Your application source code
    
- Runtime environment (Node, Python, Java, etc.)
    
- Dependencies and libraries
    
- Configurations
    
- Tools or utilities needed by the app
    

Custom images ensure **consistent environments** across development, testing, and production.

---

# 🧱 **2. How Custom Images Are Built**

Custom images are built using a **Dockerfile**, which contains step-by-step instructions.

Typical steps include:

1. **Select a base image (FROM)**
    
2. **Set working directory (WORKDIR)**
    
3. **Copy files (COPY / ADD)**
    
4. **Install dependencies (RUN)**
    
5. **Expose ports (EXPOSE)**
    
6. **Define startup command (CMD or ENTRYPOINT)**
    

---

# 📄 **3. Sample Dockerfile (Example: Python App)**

```dockerfile
# 1. Base image
FROM python:3.11-slim

# 2. Set working directory
WORKDIR /app

# 3. Copy dependency file
COPY requirements.txt .

# 4. Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# 5. Copy all project files
COPY . .

# 6. Expose app port
EXPOSE 8000

# 7. Start application
CMD ["python", "app.py"]
```

---

# 🛠 **4. Building the Image**

Use the `docker build` command:

```bash
docker build -t myapp:latest .
```

Explanation:

- `-t myapp:latest` → tags the image
    
- `.` → build context (current directory)
    

After building, you can list the image:

```bash
docker images
```

---

# ▶️ **5. Running the Custom Image**

Use `docker run`:

```bash
docker run -p 8080:8000 myapp:latest
```

This will start the container and map port `8080` on the host to `8000` in the container.

---

# 🎯 **6. Best Practices for Building Custom Images**

- Use **lightweight base images** (e.g., `alpine`, `slim`)
    
- Combine commands to reduce layers:
    
    ```dockerfile
    RUN apt-get update && apt-get install -y curl
    ```
    
- Use `.dockerignore` to prevent unwanted file uploads
    
- Prefer **COPY** over ADD (unless special features are needed)
    
- Use **multi-stage builds** for smaller images
    
- Avoid storing secrets in Dockerfile
    

---

# 📦 **7. Multi-Stage Build Example (Optimized Image)**

```dockerfile
# Stage 1: Build
FROM node:18 as builder
WORKDIR /app
COPY package*.json .
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Run
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
```

Benefits:

- Much smaller final image
    
- Only necessary files included
    
- Faster deployments
    

---

# 🔍 **8. Verify Image Layers**

```bash
docker history myapp:latest
```

Shows how layers were created and their sizes.

---

# 📁 **9. Build Context & .dockerignore**

Docker sends all files in the build directory as a "context".  
Use `.dockerignore` to exclude unnecessary files:

```
node_modules/
.git/
*.log
.env
```

This speeds up builds and makes images smaller.

---

# 📝 **Summary**

|Concept|Description|
|---|---|
|Custom Image|Your app + dependencies in a Docker image|
|Dockerfile|Instructions to build the image|
|`docker build`|Command to create the image|
|Multi-stage builds|Optimize image size|
|Best practices|Lightweight, secure, minimal layers|

---

If you want, I can also give you:  
✅ A **Node.js**, **Java**, or **Go** version of the Dockerfile  
✅ A **diagram** explaining the build process  
✅ A **cheat sheet** for Docker image commands