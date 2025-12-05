A small project to demonstrate how Java handles files, serialization, reading/writing data, and storing objects.

---

## **1. Goal**

Create a simple **Student Management System** that can:

- Add students
    
- View students
    
- Search students
    
- Save students to a file
    
- Load students automatically when the program starts
    

This shows practical usage of:

- File handling
    
- Serialization
    
- Object streams
    
- Exception handling
    
- Collections
    

---

## **2. Student Class**

```java
import java.io.Serializable;

public class Student implements Serializable {
    private static final long serialVersionUID = 1L;

    private int id;
    private String name;
    private String course;

    public Student(int id, String name, String course) {
        this.id = id;
        this.name = name;
        this.course = course;
    }

    @Override
    public String toString() {
        return id + " | " + name + " | " + course;
    }
}
```

**Key points**

- Implements `Serializable` so objects can be saved and restored.
    
- `serialVersionUID` avoids issues during deserialization.
    
- `toString()` helps display student data.
    

---

## **3. File Handler**

Handles saving and loading all students.

```java
import java.io.*;
import java.util.ArrayList;

public class StudentFileHandler {

    private static final String FILE_NAME = "students.dat";

    public static void saveStudents(ArrayList<Student> students) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(FILE_NAME))) {
            oos.writeObject(students);
        } catch (IOException e) {
            System.out.println("Error saving data: " + e.getMessage());
        }
    }

    public static ArrayList<Student> loadStudents() {
        File file = new File(FILE_NAME);

        if (!file.exists()) return new ArrayList<>();

        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) {
            return (ArrayList<Student>) ois.readObject();
        } catch (Exception e) {
            System.out.println("Error loading data: " + e.getMessage());
            return new ArrayList<>();
        }
    }
}
```

**What this does**

- Saves the entire list using serialization.
    
- Loads the list if the file exists.
    
- Avoids errors by returning an empty list if file missing.
    

---

## **4. Main Application**

Menu-driven console program.

```java
import java.util.*;

public class StudentManagementApp {

    public static void main(String[] args) {
        ArrayList<Student> students = StudentFileHandler.loadStudents();
        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("\nStudent Management");
            System.out.println("1. Add Student");
            System.out.println("2. View Students");
            System.out.println("3. Search Student");
            System.out.println("4. Save & Exit");
            System.out.print("Choice: ");

            int choice = sc.nextInt();
            sc.nextLine();

            switch (choice) {

                case 1:
                    System.out.print("ID: ");
                    int id = sc.nextInt();
                    sc.nextLine();

                    System.out.print("Name: ");
                    String name = sc.nextLine();

                    System.out.print("Course: ");
                    String course = sc.nextLine();

                    students.add(new Student(id, name, course));
                    break;

                case 2:
                    for (Student s : students) {
                        System.out.println(s);
                    }
                    break;

                case 3:
                    System.out.print("Search ID: ");
                    int sid = sc.nextInt();
                    sc.nextLine();

                    boolean found = false;
                    for (Student s : students) {
                        if (s.toString().startsWith(sid + "")) {
                            System.out.println("Found: " + s);
                            found = true;
                        }
                    }
                    if (!found) System.out.println("Not found");
                    break;

                case 4:
                    StudentFileHandler.saveStudents(students);
                    System.out.println("Saved.");
                    return;

                default:
                    System.out.println("Invalid choice");
            }
        }
    }
}
```

---

## **5. Concepts Used**

- File creation and checking
    
- Binary file writing
    
- Binary file reading
    
- Serialization
    
- Deserialization
    
- ArrayList for storage
    
- try-with-resources for safe closing
    
- Basic menu and input handling
    

---

## **6. Improvements**

- Delete or update students
    
- Save data in text/CSV/JSON
    
- Add sorting and filtering
    
- Add GUI (JavaFX)
    
- Convert to JDBC later
    

---

## **7. Interview Questions**

**1. Why use serialization here?**  
To save and restore whole objects without converting them manually.

**2. Why is try-with-resources used?**  
It closes the file streams automatically.

**3. Why store data in a binary file instead of text?**  
Binary files preserve the structure of objects.

**4. What happens if the file is missing?**  
We return an empty list safely.

**5. What happens if serialVersionUID changes?**  
Old files may become incompatible.

---
