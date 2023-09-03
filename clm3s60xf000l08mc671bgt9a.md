---
title: "Master JDBC: Connect Java to Databases for CRUD Operations"
seoTitle: "Java Database Connectivity: Essential Steps for CRUD Operations"
seoDescription: "Learn how to seamlessly connect Java to databases for CRUD operations with these 5 essential steps. Master Java JDBC for efficient data management."
datePublished: Sun Sep 03 2023 18:22:46 GMT+0000 (Coordinated Universal Time)
cuid: clm3s60xf000l08mc671bgt9a
slug: connect-java-to-databases
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693764125038/38abf379-958d-428e-9f77-3d4727ac2dd2.png
tags: java, databases, jdbc, crud-operations, jdbc-tutorial

---

Java Database Connectivity, or JDBC, is a powerful and essential technology for Java developers when it comes to interacting with databases. Whether you're building a web application, a desktop application, or any software that needs to store and retrieve data, JDBC provides a standardized way to connect to various databases and perform CRUD (Create, Read, Update, Delete) operations. In this article, we'll explore JDBC and walk you through step-by-step implementations of CRUD operations using Java and JDBC.

## **Understanding JDBC**

JDBC is a Java-based API (Application Programming Interface) that enables Java applications to interact with relational databases. It provides a bridge between Java code and database systems, allowing developers to send SQL queries to the database, retrieve data, and manipulate database records.

### **Key Components of JDBC**

1. **Driver:** JDBC drivers are platform-specific implementations that allow Java applications to connect to a particular database system. Each database vendor usually provides its own JDBC driver. These drivers can be categorized into four types: Type 1, Type 2, Type 3, and Type 4, based on their architecture and how they connect to the database.
    
2. **Connection:** JDBC provides the `java.sql.Connection` interface to establish a connection between the Java application and the database. This connection is essential for sending SQL statements and managing transactions.
    
3. **Statement:** The `java.sql.Statement` interface allows you to execute SQL queries and statements against the database. There are three types of statements: `Statement`, `PreparedStatement`, and `CallableStatement`, each serving different purposes.
    
4. **ResultSet:** The `java.sql.ResultSet` interface represents the result of a database query. It allows you to fetch and manipulate the data retrieved from the database.
    

## **CRUD Operations with JDBC**

Now, let's dive into implementing CRUD operations using JDBC. We'll demonstrate each operation with a simple Java program.

### **Prerequisites**

Before you begin, make sure you have the following set up:

1. A Java Development Kit (JDK) is installed on your machine.
    
2. A relational database (e.g., MySQL, PostgreSQL, Oracle) is installed and running.
    
3. The JDBC driver for your database(In this tutorial we will be using the PostgreSQL database driver).
    

### **Step-by-Step Implementation**

#### 1\. Create (Insert) Operation

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class CreateOperation {

    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:3306/postgres";
        String username = "admin";
        String password = "admin2023";

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            String query = "INSERT INTO users (username, email) VALUES (?, ?)";
            PreparedStatement preparedStatement = connection.prepareStatement(query);

            preparedStatement.setString(1, "hari");
            preparedStatement.setString(2, "hari@gmail.com");

            int rowsInserted = preparedStatement.executeUpdate();
            if (rowsInserted > 0) {
                System.out.println("A new user was inserted successfully!");
            }

            preparedStatement.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

In this program, we establish a database connection, prepare an SQL insert statement, and execute it to create a new user record in the database.

#### 2\. Read Operation

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ReadOperation {

    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:3306/postgres";
        String username = "admin";
        String password = "admin2023";

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            String query = "SELECT * FROM users WHERE username = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);

            preparedStatement.setString(1, "hari");

            ResultSet resultSet = preparedStatement.executeQuery();
            while (resultSet.next()) {
                String fetchedUsername = resultSet.getString("username");
                String email = resultSet.getString("email");
                System.out.println("Username: " + fetchedUsername + ", Email: " + email);
            }

            resultSet.close();
            preparedStatement.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

This program connects to the database, executes an SQL select statement to retrieve user data based on the username, and prints the results.

#### 3\. Update Operation

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class UpdateOperation {

    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost/postgres";
        String username = "admin";
        String password = "admin2023";

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            String query = "UPDATE users SET email = ? WHERE username = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);

            preparedStatement.setString(1, "john@example.com");
            preparedStatement.setString(2, "hari");

            int rowsUpdated = preparedStatement.executeUpdate();
            if (rowsUpdated > 0) {
                System.out.println("User information updated successfully!");
            }

            preparedStatement.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

This program connects to the database and updates the email address of a user with a specified username.

#### 4\. Delete Operation

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class DeleteOperation {

    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost/postgres";
        String username = "admin";
        String password = "admin2023";

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            String query = "DELETE FROM users WHERE username = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);

            preparedStatement.setString(1, "hari");

            int rowsDeleted = preparedStatement.executeUpdate();
            if (rowsDeleted > 0) {
                System.out.println("User deleted successfully!");
            }

            preparedStatement.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

In this program, we establish a database connection and delete a user record based on their username.

## **Conclusion**

JDBC is a fundamental technology for Java developers when working with databases. It provides a standardized way to perform CRUD operations, as demonstrated in this article. By following the step-by-step examples provided, you can connect to your database, and insert, retrieve, update, and delete records with ease. JDBC enables Java applications to seamlessly interact with relational databases, making it an essential tool for developing robust and data-driven applications.

%[https://twitter.com/SChennupalle/status/1698398223986225357?s=20]