---
date: 2020-09-26
linktitle: Database - PostgreSQL
menu:
  main:
    parent: java
title: PostgreSQL in Java
weight: 13
url: /java/postgresql
description: A complete guide on how to install PostgreSQL in Windows/Linux and use JDBC in Java to connect, create, insert, update, delete records in database. Code examples included.
keywords:
  - java
  - PostgreSQL
  - JDBC
tags: [Java]  
---
<meta property="og:image" content="https://tutswiki.com/images/Java/psql.jpg"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="PostgreSQL in Java" />
<meta name=”twitter:description” content="A complete guide on how to install PostgreSQL in Windows/Linux and use JDBC in Java to connect, create, insert, update, delete records in database. Code examples included." />

## What is PostgreSQL?

PostgreSQL is a free and open-source ORDBMS which is known for its extensibility and SQL compliance.
It is recommended, for storing a high volume of data, thus used in major web development projects, which can produce a large amount of user-generated data.

## Features of PostgreSQL
- Stores high volume of Data efficiently.
- Supports Composite Datatypes.
- Supports GeoSpatial Database.
- Supports Type Inheritance like Table Inheritance, View Inheritance, etc.
- High conformance with SQL standard.

## Installing PostgreSQL

### Installing PostgreSQL 12 in Windows

1. Download GUI setup from the [official website](https://www.postgresql.org/download/windows/)

    ![Windows Installer List](/images/Java/postgresql-download.png "Windows Installer List")

2. After clicking on *Download the Installer*, you'll get the list of different setups. From the list, Select *Windows x86-64* for row *12.X*

    ![postgresql download List](/images/Java/postgresql-download-list.png "Windows Installer List")

3. After completion of Download, open the Setup file, and you'll get the following screen, click on the Next button.

    ![Postgresql Setup Start](/images/Java/postgresql-setup-screen.jpg "Setup")

    - Choose Destination Directory

    ![Installation Directory](/images/Java/postgresql-setup-dir.jpg "Installation Dir")

    - Select Components
        - PostgreSQL Server(Compulsory): This is the core component of the PostgreSQL.
        - pgAdmin4: This provides GUI to interact with PostgreSQL Server.
        - Stack Builder: This is a GUI utility to install additional modules for PostgreSQL.
        - Command Line Tools(Compulsory): This provides the CLI to interact with PostgreSQL Server.

    ![PostgreSQL Components](/images/Java/postgresql-components.jpg "Components")

    - Select Directory for storing data. This folder will have all the Data that the user will store in PostgreSQL.

    ![PostgreSQL Data Store](/images/Java/postgresql-data-dir.jpg "Data Store")

    - Enter the password for the Root User: postgres. Do enter it carefully, because this is the root password.

    ![PostgreSQL Password](/images/Java/postgresql-password.jpg "Password")

    - Configure the Port on which you want to run the PostgreSQL Server. (Default = `5432`). *Do not change if you are a beginner.*

    ![PostgreSQL Port](/images/Java/postgresql-port.jpg "Port")

    - Set your Locale. Locale is a parameter that defines the user's region and hence affects the language of the User interface. Here, the most visible change it will cause is the change in the default date-time format.

    ![PostgreSQL Locale](/images/Java/postgresql-locale.jpg "Locale")

    - Installation Summary

    ![Installation Summary](/images/Java/postgresql-install-summary.jpg "Summary")
    
    - Wait for the process to complete.

    - Setup Complete

    ![Setup Complete](/images/Java/postgresql-setup-finished.jpg)

4. Change the `Path` variable for the system to make PostgreSQL globally available.

    - Search for *Edit Environment Variables* in *Start Menu*

    ![Start Search](/images/Java/edit-env-variables.jpg)

    - Select *Edit Environment Variables*

    ![Edit Environment Variables](/images/Java/env-variables.jpg)

    - Edit *System's Path variable*

    ![Edit PATH Variable](/images/Java/system-variables.jpg)

    - Add a new variable and pass the path to the `bin` folder of the Postgres installation directory.

    ![Add Path Variable](/images/Java/add-postgresql-path.jpg)

    - Click *OK* on all the opened windows.

5. To test the Postgres Installation:

    - Enter command `psql -U postgres`
    - Enter your root password, which you entered in Step `2.4`.
    - If entered without any error, you will see the new prompt of the Postgres `postgres=#`.
    - Enter `\du` to list all the available users.
    - Enter `\l` to list all the databases.
    - Enter `\conninfo` to view connection info.

    ![PostgreSQL Run](/images/Java/psql.jpg)
    
#### Possible Errors

1. Password Authentication Failed: That means the entered password is wrong
2. `psql` not found: That means the environment variable is not correctly setup.

In the above setup, you will also get GUI Software for Database Management called `PGAdmin4`. You can find the executable in the directory `pgadmin/pgadmin4.exe` inside the *Installation Folder*. In the left pane, you can select the server. When selected, it will ask the password for postgres user, you should enter the password from Step `2.4`.

The GUI will open in the browser and will look like this.

![PGAdmin GUI](/images/Java/pgAdmin-GUI.jpg)

### Installing PostgreSQL in Ubuntu Linux

1. First update system repositories
    ```terminal
    sudo apt-get update
    ```

2. Install PostgreSQL
    ```terminal
    sudo apt-get install postgresql
    ```
    We can also add Contrib packages for additional functionalities
    ```terminal
    sudo apt-get install postgresql postgresql-contrib
    ```

3. Open PostgreSQL
    ```terminal
    sudo -u postgres psql
    ```

4. To exit PostgreSQL CUI, use `\q` and hit `Enter`.
    ```terminal
    postgres=# \q
    ```

5. To create a user
    ```terminal
    sudo -u postgres createuser --i
    Enter the name of role to add: my_user
    Shall the new role be a superuser? (y/n) y
    ```

6. To create a database
    ```terminal
    sudo -u postgres createdb my_db
    ```

7. To connect via the created role
    ```terminal
    sudo -u my_user psql
    ```

8. To check Connection Info, after the above command
    ```terminal
    /conninfo
    ```
    Output:
    ```terminal
    You are now connected to database "my_db" as user "my_user" via socket in "<Postgres Installation path>" at port "5432"
    ```

9. To locate config files, use `ls etc/postgresql/<version>/main/`
    ```terminal
    ls etc/postgresql/12/main/
    conf.d   pg_ctl.conf   pg_ident.conf   start.conf   environment   pg_hba.conf   postgresql.conf
    ```

To use GUI for PostgreSQL, you can download `PGAdmin3` from the `Ubuntu Software`.

## JDBC in Java

Java Database Connectivity (JDBC) is the standard API that enables Java applications to connect with the various databases.

- It provides the methods to connect with the databases and execute queries on them which includes queries to get, add, remove, and modify data in a database.
- JDBC is database-independent and uses vendor-specific drivers to connect with databases.
- JDBC also provides a JDBC-to-ODBC bridge to communicate with databases with ODBC.

For the following content, you are expected to have a basic knowledge of Java and DBMS.

## How to use JDBC

To use JDBC, you should have a database with a user which have sufficient privileges on that database. JDBC also needs that user's name and password to create a connection with the database.

### Example
```terminal
postgres=# CREATE DATABASE my_database;
    CREATE DATABASE
postgres=# CREATE USER temp_user WITH ENCRYPTED PASSWORD 'temp_password';
    CREATE ROLE
postgres=# GRANT ALL PRIVILEGES ON DATABASE my_database to temp_user;
    GRANT
postgres=#
```

Apart from this, users must also have the PostgreSQL Drivers for JDBC. These drivers can be downloaded from the [postgresql website](https://jdbc.postgresql.org/download.html).

##  Setup project in Netbeans IDE

Netbeans is the official Java IDE and managed by Oracle.

1. Download and install [Netbeans IDE](https://netbeans.org/downloads/6.1/index.html).
2. Create a new Java Project in Netbeans IDE.
3. Add downloaded PostgreSQL driver in the Project.
    1. Right-click on `Projects` and select `Properties`.
    2. Click on `Add Library` and search for the PostgreSQL driver and select it.
    3. If not found, click on `Add JAR/Folder` and select downloaded Drivers.

## Setup project in IntelliJ

1. Download and install [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/#section=windows).
2. Create a Java Command Line Project.
3. Add downloaded PostgreSQL driver in the Project.<br />
    1. After opening the project, click on `File` and then select `Project Structure`.
    2. Go to `Library` in `Project Settings` Menu in Left Pane.
    3. Click on the `+` Plus button in Main Pane.
    4. Browse for the Downloaded PostgreSQL Drivers and select the jar file.

## Writing First JDBC program

Prerequisites: A Database and UserName and Password for the user with privileges on the Database.

### Code
```java
package com.personal;

import java.sql.Connection;
import java.sql.DriverManager;

public class Main {

    public static void main(String[] args) {
        Connection c = null;
        try {
            Class.forName("org.postgresql.Driver");
            Connection conn = DriverManager.getConnection(
                    "jdbc:postgresql://localhost:5432/my_database", // Connection String
                    "temp_user",                                    // Database User Name
                    "temp_password");                               // Database User Password
            System.out.println("Opened database successfully");
            if (conn.getMetaData().supportsBatchUpdates()) {
                System.out.println("The Database drivers support Batch Updates");
            } else {
                System.out.println("The Database drivers does not support Batch Updates");
            }

        } catch (Exception e) {
            e.printStackTrace();
            System.err.println(e.getClass().getName() + ": " + e.getMessage());
            System.exit(0);
        }
    }
}

```

Output:
```terminal
Opened database successfully
The Database drivers support Batch Updates
```

### Explanation

In the above program:

1. `Class.forName` initializes the PostgreSQL Drivers.
2. The next line creates the connection object which connects to the Database via the forementioned Drivers.<br />
    Here, Connections string is `"<DriverClass>://<Database_Host>:<Database_Port>/<Database_Name>"`
3. For a successful connection, a success message will print. 
    Otherwise, the Exception will be thrown.
4. After successful creation, we also check whether the driver supports the Batch Updates or not.<br />
    This line is used to make use of the connection made in the previous step.

### Possible Errors
- `Password authentication failed for user "<user>"`
 
    __Possible Cause:__ If user name or password is incorrect.
- `database "my_database" does not exist`
    
	__Possible Cause:__ If the database `my_database` is not present.

## Creating a new table using JDBC

### Code
```java
package com.personal;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Main {

    public static void createTable(Connection conn) throws SQLException {
        final Statement statement = conn.createStatement();

        String query = "CREATE TABLE post " +
                "(id SERIAL NOT NULL, " +
                "content VARCHAR(255), " +
                "likes INTEGER, " +
                "PRIMARY KEY (id))";

        statement.executeUpdate(query);
    }

    public static void main(String[] args) {
        Connection conn = null;
        try {
            Class.forName("org.postgresql.Driver");
            conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/my_database","temp_user", "temp_password");

            System.out.println("Creating Table `Post`");
            createTable(conn);
            System.out.println("Created Table `Post`");

        } catch (Exception e) {
            e.printStackTrace();
            System.err.println(e.getClass().getName() + ": " + e.getMessage());
            System.exit(0);
        } finally {
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}

```

Output:
```terminal
Creating Table `Post`
Created Table `Post`
```

### Explanation
- The `createTable()` method creates a table with columns `id`, `content`, and `likes` where `id` is Auto-Generated, Primary key for the table; `content` is a string which contains the content of the posts; `likes` is an integer that will store the total likes given to the post.
- In this method, we used the `Statement` object to create a static statement for a database.
- Then we used `executeUpdate()` to execute the statement. Here, `executeStatement()` will return `0`(zero) by default.
- This method could throw `SQLException` if any database access occurs.

### Possible Errors
- `Relation "post" already exists`: If `post` table is already present in the Database.

### Verification
To verify that the table is created, you can do any of the following:

- Run this program again. If the table already exists, a `PSQLException` is thrown with the message "Table already exists".
- Open `psql` in the terminal with the following command: `psql -U temp_user -d my_database` and then enter the respective password. After this, enter command `\dt;`, and you will get the following output
```terminal
my_database=> \dt;

         List of relations
 Schema | Name | Type  |   Owner   
--------+------+-------+-----------
 public | post | table | temp_user
(1 row)
```

## Entering a new record in the table

### Code

```java
package com.personal;

import java.sql.*;

public class Main {

    public static void addPost(Connection conn, String content, int likes) throws SQLException {

        String query = "INSERT INTO post(content, likes) VALUES (?, ?)";
        PreparedStatement statement = conn.prepareStatement(query);

        statement.setString(1, content);
        statement.setInt(2, likes);

        statement.executeUpdate();
    }

    public static void main(String[] args) {
        Connection conn = null;
        try {
            Class.forName("org.postgresql.Driver");
            conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/my_database", "temp_user", "temp_password");

            System.out.println("Creating `Post`");
            addPost(conn, "This is my First post by JDBC", 1);
            System.out.println("Successfully Created First `Post`");

        } catch (Exception e) {
            e.printStackTrace();
            System.err.println(e.getClass().getName() + ": " + e.getMessage());
            System.exit(0);
        } finally {
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}

```

Output:
```terminal
Creating `Post`
Successfully Created `Post`
```

### Explanation
- Here, `addPost()` takes `Connection` as the parameter with the `content` and `likes` for a Post. And then add the create a new record with passed `content` and `likes` in the post table.
- Here, we have used `PreparedStatement` instead of `Statement` because the Query is dynamic.
- `PreparedStatement` is used when the queries are dynamic or queries are reused.
- Here, we have used `executeUpdate()` method to execute statement. Here, the method should return `1`(One) which represents the addition of one record. User can check the value by enclosing the `executeUpdate()` line inside the `System.out.println()`.

### Verification
To verify that the record is added in the table, you should follow these steps:

- First, open psql by the following command: `psql -U temp_user -d my_database` and then enter the respective password.
- Then the records can be viewed by the following command `SELECT * from post`, and you will get the following output.
```terminal
my_database=> SELECT * from post;

 id |            content            | likes 
----+-------------------------------+-------
  1 | This is my First post by JDBC |     1
(1 row)
```

## Fetching records from the table

### Code
```java
package com.personal;

import java.sql.*;

public class Main {
    public static void printAllPosts(Connection conn) throws SQLException {
        final Statement statement = conn.createStatement();
        final String query = "SELECT * FROM post";
        ResultSet result = statement.executeQuery(query);

        while (result.next()) {
            int id = result.getInt("id");
            String content = result.getString("content");
            int likes = result.getInt("likes");
            System.out.printf("Post with Id = %d had content = '%s' with %d likes\n", id, content, likes);
        }
    }

    public static void main(String[] args) {
        Connection conn = null;
        try {
            Class.forName("org.postgresql.Driver");
            conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/my_database","temp_user", "temp_password");

            System.out.println("Content of table `Post`:");
            printAllPosts(conn);

        } catch (Exception e) {
            e.printStackTrace();
            System.err.println(e.getClass().getName() + ": " + e.getMessage());
            System.exit(0);
        } finally {
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}

```

Output:
```terminal
Content of table `Post`:
Post with Id = 1 had content = 'This is my First post by JDBC' with 1 likes
```

### Explanation
- The method `printAllPosts()` prints all the posts of the table `post`.
- It again uses the `Statement` object to form a query because the required query is static.
- Then, it uses `executeQuery()` method to execute the Statement and unlike `executeUpdate()` method, this method returns a `ResultSet` which has all the records that are produced by the query.
- Initially, `ResultSet` will have a cursor on the first record of the queried data. The Queried record can be fetched by the `next()` method of `ResultSet` which moves the cursor to the next record.
- The `getInt()` and `getString()` methods are used to get the Data of a result where the parameter will specify the column for which method is called, and the return value will be the value of the specified column.

## Update existing record in the table

### Code
```java
package com.personal;

import java.sql.*;

public class Main {
    public static void updateLikes(Connection conn, Integer id, Integer likes) throws SQLException {
        String query = "UPDATE post set likes = ? WHERE id = ?";

        PreparedStatement statement = conn.prepareStatement(query);
        statement.setInt(1, likes);
        statement.setInt(2, id);


        statement.executeUpdate();
    }

    public static void main(String[] args) {
        Connection conn = null;
        try {
            Class.forName("org.postgresql.Driver");
            conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/my_database", "temp_user", "temp_password");

            System.out.println("Updating Likes for Post with id 1 to 5 likes");
            updateLikes(conn, 1, 5);
            System.out.println("Update Successful");

        } catch (Exception e) {
            e.printStackTrace();
            System.err.println(e.getClass().getName() + ": " + e.getMessage());
            System.exit(0);
        } finally {
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}

```

Output:
```terminal
Updating Likes for Post with id 1 to 5 likes
Update Successful
```

### Explanation
- The method `updateLikes()` will take the `Connection` with the `id` of the desired post and new Count of `likes` for that post as parameters. It is used to update the `likes` for the post with `id` passed as parameters.
- In the implementation, `PreparedStatement` is used, since the parameters `id` and `likes` will get updated with each call.
- The `executeUpdate()` will execute the Statement and returns the count of the records affected by the query. In this case, the count should be `1`(One).

### Verification
You can verify the above program by the following methods:

- Use the aforementioned `printAllPosts()` method and find the post with `id` = 1 from the output and check whether its `likes` got updated to 1 or not.
- Can run the following query directly in psql: `SELECT * FROM post WHERE id = 1`, and you will get the following output.
```terminal
my_database=> SELECT * FROM post WHERE id = 1;

 id |            content            | likes 
----+-------------------------------+-------
  1 | This is my First post by JDBC |     5
(1 row)
```

## Deleting a record from the table

### Code
```java
package com.personal;

import java.sql.*;

public class Main {
    public static void deletePost(Connection conn, Integer id) throws SQLException {

        String query = "DELETE FROM \"post\" WHERE id = ?";

        PreparedStatement statement = conn.prepareStatement(query);
        statement.setInt(1, id);

        statement.executeUpdate();
    }

    public static void main(String[] args) {
        Connection conn = null;
        try {
            Class.forName("org.postgresql.Driver");
            conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/my_database", "temp_user", "temp_password");

            System.out.println("Deleting Post with id 1 from table `Post`:");
            deletePost(conn, 1);
            System.out.println("Deletion Successful");

        } catch (Exception e) {
            e.printStackTrace();
            System.err.println(e.getClass().getName() + ": " + e.getMessage());
            System.exit(0);
        } finally {
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}

```

Output:
```terminal
Deleting Post with id 1 from table `Post`:
Deletion Successful
```

### Explanation
- The `deletePost()` method takes `Connection` as a parameter with the `id` of the post for deletion.
- This method also uses `PreparedStatement` to form a statement and uses `executeUpdate()` to execute the statement. 
Here, `executeUpadate()` will return the count of records affected by the execution of the statement and should be equal to `1`(One).

### Verification
User can verify the above program using the following methods

- Can run the above `printAllPost()` method to print the Posts, and try to find the Post with Id = 1, if not found then the program worked correctly.
- Can run the following Query, directly in psql: `SELECT * FROM post WHERE id = 1`, and you will get the following output.
```terminal
my_database=> SELECT * FROM post WHERE id = 1;

 id | content | likes 
----+---------+-------
(0 rows)
```

<br />

## Difference between executeUpdate() and executeQuery()

|executeUpdate()|executeQuery()|
| :-: | :-: |
| executeUpdate() is generally used to execute the statement which doesn't return any column (DML Queries like UPDATE query) | executeQuery() is used when the execution of statements will return one or more records. (SELECT statements) |
| executeUpdate() returns an integer which represents the number of records affected by the executed DML query or in case of other queries it returns 0. | executeQuery() returns the ResultSet which has the records produced by the query and will have the cursor pointing to the first record. |

## Difference between Statement and PreparedStatement

| Statement | PreparedStatement |
| :-: | :-: |
| The Statement is static, and hence will result in the same query in each execution | PreparedStatement is dynamic and can be modified by the parameters which can be set dynamically. |
| The Statement is generally used for one time. The Statement queries will result in the same query every but the output may vary based on the other queries ran in the middle of the two uses of Statement. | PreparedStatement can be reused any number of times. Whenever parameters are changed, the query will change. This is faster than creating a new Statement every time. |

## Features of JDBC

- **Portability**: JDBC code is highly portable as compared to ODBC.
- **Advanced Datatypes Support**: Like BLOB, etc.
- **Supports Batch Processing**: Batch processing aims to reduce Database Visits thus it optimizes the execution time when the Database is stored in a different system than the server and if there is an inherent latency in connection with the Database.
- **Supports Savepoints**: JDBC supports the savepoints which allow users to roll back the changes done till a particular savepoint.


## Key Terms

1. **Relational Database**: Relational Database stores the data in such a way so that it allows information to get accessed or identified by other related information, thus creating a *relationship* between them. The most common way to structure a Relational Database is through related tables.
2. **RDBMS**: Relational Database Management System is the software that enables users to build and manage a relational database. 
3. **ORDBMS**: Object-Relation Database Management System combines the advantages of *Object-Oriented Database Models* and RDBMS to provide a superior DBMS. It allows the usage of complex data types and types of inheritance.
4. **SQL**: Structured Query Language is used to access and manage RDBMS. It is famous for its simplicity, usefulness, and similarity with the English language. It provides functionality to
    
    - Query the stored data (DQL - Data Query Language), 
    - Define a schema for data (DDL - Data Definition Language), 
    - Manipulate data (DML - Data Manipulation Language), and 
    - Control over data (DCL - Data Control Language)
