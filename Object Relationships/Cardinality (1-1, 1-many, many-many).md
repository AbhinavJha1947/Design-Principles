# Cardinality (Multiplicity)

Cardinality defines "How many" instances of one class are related to "How many" instances of another class.

## 1. One-to-One (1:1)
One object is related to primarily one other object.
-   **Example**: `User` and `UserProfile`.
-   **Code**:
    ```csharp
    class User { public UserProfile Profile { get; set; } }
    class UserProfile { public User User { get; set; } }
    ```

## 2. One-to-Many (1:N)
One object is related to many others.
-   **Example**: `Blog` and `Posts`. A Blog has many Posts. A Post belongs to one Blog.
-   **Code**:
    ```csharp
    class Blog { public List<Post> Posts { get; set; } }
    class Post { public Blog Blog { get; set; } }
    ```

## 3. Many-to-Many (N:N)
Many objects are related to many others.
-   **Example**: `Student` and `Course`. A Student takes many Courses. A Course has many Students.
-   **Implementation**: This usually requires a **Join Table** (intermediate object) in databases, or just two Lists in code.
    ```csharp
    class Student { public List<Course> Courses { get; set; } }
    class Course { public List<Student> Students { get; set; } }
    ```
