# Source Code — Spring Boot Field Injection Example

### `pom.xml`
- Maven project configuration: Spring Boot parent, Java 17, and the dependencies supplied for this project.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- Spring Boot parent supplies managed dependency versions. -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>4.1.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>SpringBootDependencyDataFetch_Project</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>SpringBootDependencyDataFetch_Project</name>
    <description/>
    <url/>
    <licenses><license/></licenses>
    <developers><developer/></developers>
    <scm>
        <connection/>
        <developerConnection/>
        <tag/>
        <url/>
    </scm>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <!-- Spring Boot application dependencies. -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>demo</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

### `Data.java`
- The interface contract shared by both data-source classes.
```java
package com.example.demo;

public interface Data {
    // Each data-source implementation must provide this operation.
    abstract void getData();
}
```

### `Oracle.java`
- A Spring-managed `Data` implementation; `@Primary` makes it the preferred bean for injection.
```java
package com.example.demo;

import org.springframework.context.annotation.Primary;
import org.springframework.stereotype.Component;

@Component // Register this class as a Spring bean.
@Primary // Choose this Data bean when multiple implementations match.
public class Oracle implements Data {

    @Override
    public void getData() {
        // Show which data-source implementation Spring selected.
        System.out.println("Fetching data from oracle");
    }

}
```

### `MYSQL.java`
- The second Spring-managed `Data` implementation; it is available but not selected by default because Oracle is `@Primary`.
```java
package com.example.demo;

import org.springframework.context.ApplicationContext;
import org.springframework.stereotype.Component;
import org.springframework.context.support.ClassPathXmlApplicationContext;

@Component // Register this class as a Spring bean.
public class MYSQL implements Data {

    @Override
    public void getData() {
        // Message used if this implementation is selected and called.
        System.out.println("fetch data from sql");
    }

}
```

### `FetchData.java`
- A Spring component that uses **field injection** to receive a `Data` implementation and call it.
```java
package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component // Register FetchData as a Spring bean.
public class FetchData {
    // Field injection: Spring injects the preferred Data bean (Oracle) here.
    @Autowired
    private Data d;

    void show() {
        // Call the injected implementation, then print the completion message.
        d.getData();
        System.out.println("get the data");
    }

}
```

### `SpringBootDependencyDataFetchProjectApplication.java`
- The Spring Boot entry point: starts the application, retrieves `FetchData`, and calls `show()`.
```java
package com.example.demo;

import org.springframework.context.ApplicationContext;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication // Enables component scanning and Spring Boot configuration.
public class SpringBootDependencyDataFetchProjectApplication {

    public static void main(String[] args) {
        // Start Spring Boot and obtain its application context.
        ApplicationContext con = SpringApplication.run(
            SpringBootDependencyDataFetchProjectApplication.class, args);

        // Retrieve the Spring-managed FetchData bean and run its behavior.
        FetchData d = con.getBean(FetchData.class);
        d.show();
    }

}
```
