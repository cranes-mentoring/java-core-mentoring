### Building Java Projects with Maven and Gradle: A Detailed Comparison

### Maven:

**Maven** is a popular build tool that simplifies the process of managing and building Java projects. It follows the convention over configuration principle, providing a standard project structure. Maven uses an XML-based configuration file named `pom.xml`.

**Pros of Maven:**
1. **Convention over Configuration:** Maven follows conventions, reducing the need for explicit configuration.
2. **Central Repository:** Maven has a vast central repository of libraries, making dependency management straightforward.
3. **Build Lifecycle:** Maven defines a set of build phases (compile, test, package, etc.), simplifying the build process.

**Cons of Maven:**
1. **Verbosity:** XML configuration can be verbose and may require boilerplate code.
2. **Learning Curve:** Understanding and customizing Maven's lifecycle may have a learning curve.

#### Maven Example:

**pom.xml:**
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-project</artifactId>
    <version>1.0.0</version>
</project>
```

**Basic Commands:**
- `mvn clean`: Cleans the target directory.
- `mvn compile`: Compiles the source code.
- `mvn test`: Runs tests.
- `mvn package`: Packages the project into a JAR or other specified formats.
- `mvn install`: Installs the project artifact into the local repository.

### Gradle:

**Gradle** is a build automation tool that combines flexibility with simplicity. It uses a Groovy-based DSL or Kotlin for build scripts. Gradle allows fine-grained control over the build process and offers incremental builds.

**Pros of Gradle:**
1. **Flexibility:** Gradle provides flexibility in defining custom build logic and tasks.
2. **Performance:** Gradle's incremental builds enhance performance for large projects.
3. **Conciseness:** The build scripts are often more concise compared to Maven's XML.

**Cons of Gradle:**
1. **Learning Curve:** While the learning curve is generally reasonable, some complexity arises from the flexibility.
2. **Plugin Variability:** Plugin quality and compatibility may vary.

#### Gradle Example:

**build.gradle:**
```groovy
plugins {
    id 'java'
}

group 'com.example'
version '1.0.0'
```

**Basic Commands:**
- `gradle clean`: Cleans the build directory.
- `gradle build`: Builds the project.
- `gradle test`: Runs tests.
- `gradle assemble`: Assembles the project.
- `gradle install`: Installs the project into the local repository.

### Key Differences:

1. **Syntax:**
    - Maven: XML-based configuration.
    - Gradle: Groovy-based DSL or Kotlin.

2. **Convention vs. Configuration:**
    - Maven follows convention over configuration.
    - Gradle allows more flexibility and is less opinionated.

3. **Plugin Ecosystem:**
    - Both support a rich ecosystem of plugins.
    - Maven plugins are often configured in XML, while Gradle plugins are scripted in Groovy or Kotlin.

### Dependency Management:

#### Maven:
```xml
<dependencies>
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>library</artifactId>
        <version>1.2.3</version>
    </dependency>
</dependencies>
```

#### Gradle:
```groovy
dependencies {
    implementation 'com.example:library:1.2.3'
}
```

### Managing Versions:

#### Maven:
```xml
<properties>
    <project.version>1.0.0</project.version>
</properties>
```

#### Gradle:
```groovy
version '1.0.0'
```

### Adding a Plugin:

#### Maven:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
        </plugin>
    </plugins>
</build>
```

#### Gradle:
```groovy
plugins {
    id 'java'
}
```

### Creating a JAR File:

#### Maven:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.1.0</version>
            <configuration>
                <archive>
                    <manifest>
                        <addClasspath>true</addClasspath>
                        <mainClass>com.example.App</mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>
    </plugins>
</build>
```

#### Gradle:
```groovy
jar {
    manifest {
        attributes(
            'Main-Class': 'com.example.App'
        )
    }
}
```

### Building and Running:

1. **Build with Maven:**
   ```bash
   mvn clean install
   ```

2. **Build with Gradle:**
   ```bash
   gradle build
   ```

3. **Run JAR File:**
   ```bash
   java -jar target/my-project-1.0.0.jar
   ```

### Conclusion:

Both Maven and Gradle are powerful build tools, and the choice depends on project requirements and personal preferences. Maven's convention over configuration simplifies standard projects, while Gradle's flexibility makes it suitable for complex builds. Understanding the differences helps in making an informed decision based on the specific needs of a project.