## 🔹 What is the **JDK (Java Development Kit)?**

- The **JDK** is a full-featured development kit that provides everything needed to **write, compile, test, and run** Java applications.
    
- It includes:
    
    - **Java Compiler (`javac`)**: Converts Java source code into bytecode (.class files).
        
    - **Java Runtime Environment (JRE)**: Allows running Java applications.
        
    - Various tools like `javadoc`, `jar`, and debugging utilities.
        

---

## 🔹 What is the **JRE (Java Runtime Environment)?**

- The **JRE** is a subset of the JDK and contains the **Java Virtual Machine (JVM)** and **core libraries** necessary to **run** Java applications.
    
- It does **not** include tools or compilers, so cannot be used for development.
    
- Useful for machines where only the running of Java programs is required.
    

---

## ⚙️ Detailed Setup Steps

1. **Download JDK**
    
    - Visit [Oracle JDK official page](https://www.oracle.com/java/technologies/downloads/) or use OpenJDK builds from sites like Adoptium.
        
    - Choose version suitable for your OS (Windows, Linux, Mac).
        
2. **Install JDK**
    
    - Run the downloaded installer executable or unpack the archives (depending on OS).
        
    - Default installation usually includes the JRE folder inside it.
        
3. **Set Environment Variables**
    
    - **`JAVA_HOME`**:
        
        - Set this variable to the root folder where JDK is installed.
            
        - Example:
            
            - Windows: `C:\Program Files\Java\jdk-17`
                
            - Linux/Mac: `/usr/lib/jvm/java-17-openjdk`
                
    - **Add to PATH**:
        
        - Update your system `PATH` variable by adding:
            
            - Windows: `%JAVA_HOME%\bin`
                
            - Linux/Mac: `$JAVA_HOME/bin`
                
        - This allows running Java commands from any terminal or command prompt.
            
4. **Verify Installation**
    
    - Open terminal or command prompt and run:
        
        - `java -version` → Displays installed Java runtime version.
            
        - `javac -version` → Displays installed Java compiler version.
            
    - Both commands should confirm correct installation and versions.
        

---

## 📝 Important Points

- **JDK includes JRE**, so if JDK is installed, separate JRE installation is usually unnecessary.
    
- For **developing** Java programs (writing and compiling), the **JDK** is mandatory.
    
- For only **running** Java applications, installing the **JRE** alone is sufficient but less common nowadays.
    
- Always keep JDK updated to latest stable version for security and performance improvements.
    
- When multiple Java versions exist on your machine, ensure your `JAVA_HOME` and `PATH` point to the intended version.