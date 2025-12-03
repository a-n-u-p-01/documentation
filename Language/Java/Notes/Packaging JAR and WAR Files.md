In Java development, applications are often bundled and distributed in standardized formats. Two of the most important packaging formats are:

- **JAR (Java ARchive)** – for standalone Java applications or libraries
    
- **WAR (Web Application Archive)** – for web applications that run on Java EE/Jakarta EE servers
    

Both formats are built on the ZIP specification but serve different purposes.

---

### **1. JAR Files (Java ARchive)**

#### **1.1 Definition**

A **JAR file** is a compressed archive containing Java classes, metadata, and resources needed to run a Java application or library.  
It typically contains compiled `.class` files and a **manifest**.

---

#### **1.2 When are JAR Files Used?**

- For packaging **desktop or console applications**
    
- For distributing **Java libraries or frameworks**
    
- For bundling **utility code**, configuration files, images, etc.
    
- For use with build tools (Maven, Gradle)
    
- For running standalone programs using:
    
    ```
    java -jar filename.jar
    ```
    

---

#### **1.3 Internal Structure of a JAR**

A typical JAR file contains:

```
MyApp.jar
 ├── META-INF/
 │     └── MANIFEST.MF
 ├── com/
 │     └── example/
 │           └── MyClass.class
 ├── resources/
 │     └── config.properties
 └── lib/ (optional)
```

#### **Manifest File (MANIFEST.MF)**

The manifest contains metadata. Common entries:

```
Manifest-Version: 1.0
Main-Class: com.example.MainApp
Class-Path: lib/library1.jar lib/library2.jar
```

**Main-Class** is required for executable JARs.

---

## **1.4 Creating a JAR File**

### **Using the JDK command line**

Compile:

```
javac -d out src/*.java
```

Create manifest (optional):

```
echo Main-Class: com.example.MainApp > manifest.txt
```

Package:

```
jar cfm MyApp.jar manifest.txt -C out .
```

### **Using Maven**

`pom.xml`:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

To create a JAR:

```
mvn package
```

### **Using Gradle**

```
jar {
    manifest {
        attributes "Main-Class": "com.example.MainApp"
    }
}
```

---

## **1.5 Types of JAR Files**

### **1. Runnable (Executable) JAR**

Contains Main-Class entry.

### **2. Library JAR**

Used as dependency by other projects.

### **3. Fat JAR / Uber JAR / Shaded JAR**

Contains application + all third-party dependencies.

Tools:

- Maven Shade plugin
    
- Spring Boot repackage
    
- Gradle shadow plugin
    

---

# **2. WAR Files (Web Application Archive)**

## **2.1 Definition**

A **WAR file** packages a Java web application (Servlet/JSP/Spring MVC, etc.) for deployment to an application server like:

- Apache Tomcat
    
- Jetty
    
- WildFly / JBoss
    
- GlassFish / Payara
    

---

## **2.2 When are WAR Files Used?**

- For servlet-based web applications
    
- For enterprise Java web deployments
    
- When deploying to containers (Tomcat, WildFly)
    
- When using older Java EE / Jakarta EE frameworks
    

Modern Spring Boot apps use JARs, but WAR is still important.

---

## **2.3 Internal Structure of a WAR**

Typical structure:

```
MyWebApp.war
 ├── WEB-INF/
 │     ├── web.xml
 │     ├── classes/
 │     │     └── com/example/*.class
 │     ├── lib/
 │     │     └── dependencies.jar
 │     └── views/
 ├── META-INF/
 ├── index.jsp
 ├── assets/
 │     └── css , js, images
```

### **Key components**

- **WEB-INF/web.xml** – deployment descriptor (optional for modern frameworks)
    
- **WEB-INF/classes/** – compiled classes
    
- **WEB-INF/lib/** – dependency JARs
    
- **JSP pages**, HTML, CSS, JS
    
- **META-INF/** – metadata
    

---

## **2.4 Creating a WAR File**

### **Using Maven**

Add packaging type:

```xml
<packaging>war</packaging>
```

To build:

```
mvn package
```

### **Using Gradle**

```
apply plugin: 'war'
```

Build:

```
gradle build
```

---

## **2.5 Deployment**

WAR files are deployed to an application server:

### **On Tomcat**

Copy `.war` file into:

```
TOMCAT_HOME/webapps/
```

Server deploys it automatically.

---

# **3. Major Differences Between JAR and WAR**

|Feature|JAR|WAR|
|---|---|---|
|Purpose|Standalone apps or libraries|Web applications|
|Contains|Class files, resources, manifest|JARs, classes, JSP, HTML, web.xml|
|Deployed Where?|Java runtime (JRE), command line|Application server (Tomcat, WildFly)|
|Executable?|Yes (if manifest has Main-Class)|No — must be deployed|
|Build Tools|jar, Maven, Gradle|Maven (war plugin), Gradle|
|Structure|Flat archive|Has WEB-INF and web resources|

---

# **4. EAR Files (For completeness)**

**EAR (Enterprise Archive)** packages large enterprise apps and contains WAR + JAR modules.  
Used in Java EE but less common today.

---

# **5. JAR vs WAR in Modern Java Development**

- **Spring Boot uses executable JARs**  
    The web server (like Tomcat) is embedded inside the JAR.
    
- WAR is used when:
    
    - Deploying to external servers
        
    - Supporting old enterprise infrastructures
        

---

# **6. Summary**

- **JAR files** package **general Java applications or libraries**.
    
- **WAR files** package **web applications** for deployment to servers.
    
- Both use ZIP format and contain metadata.
    
- Build tools (Maven, Gradle) simplify packaging.
    

---