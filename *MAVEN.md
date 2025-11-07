Here’s a **complete and detailed set of notes on Apache Maven** — perfect for DevOps engineers, developers, and interview preparation.
We’ll go from **basics to advanced**, including **commands, architecture, and integration with CI/CD tools like Jenkins**.

---

## 🧱 1. What is Maven?

**Apache Maven** is a **build automation and project management tool** primarily used for **Java projects**.

It helps you:

* Compile source code
* Run tests
* Package your code (e.g., `.jar` or `.war`)
* Manage dependencies
* Deploy artifacts (to Nexus, Artifactory, etc.)

---

## 🔹 2. Why use Maven?

| Feature                           | Description                                                                          |
| --------------------------------- | ------------------------------------------------------------------------------------ |
| **Dependency Management**         | Automatically downloads and manages required libraries from remote repositories.     |
| **Consistent Build System**       | Uses a standard directory structure and lifecycle for all projects.                  |
| **Reproducible Builds**           | Same result every time (due to versioned dependencies).                              |
| **Integration**                   | Works seamlessly with Jenkins, Git, Docker, Kubernetes, etc.                         |
| **Convention over Configuration** | Reduces the need for custom scripts — follows a standard layout and build lifecycle. |

---

## 📁 3. Maven Project Structure

```
my-app/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/
    │   └── resources/
    └── test/
        ├── java/
        └── resources/
```

### Explanation:

| Folder               | Purpose                                                |
| -------------------- | ------------------------------------------------------ |
| `src/main/java`      | Application source code                                |
| `src/main/resources` | Configuration, property files                          |
| `src/test/java`      | Unit test code                                         |
| `pom.xml`            | Project Object Model file — Maven’s core configuration |

---

## ⚙️ 4. POM.XML (Project Object Model)

`pom.xml` is the **heart of Maven** — defines how the project is built, what dependencies are used, and what plugins to run.

### Example:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myapp</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <name>My Sample Maven App</name>

    <dependencies>
        <!-- JUnit Dependency -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.10.1</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

---

## 🧩 5. Maven Coordinates (Artifact Identity)

Each Maven project or dependency is identified by:

| Tag            | Description                       | Example          |
| -------------- | --------------------------------- | ---------------- |
| **groupId**    | Organization or company name      | `com.amazon.aws` |
| **artifactId** | Project name                      | `s3client`       |
| **version**    | Version number                    | `1.0.2`          |
| **packaging**  | Output type (`jar`, `war`, `ear`) | `jar`            |

Together, these form the artifact’s unique identity:

```
com.amazon.aws:s3client:1.0.2
```

---

## 🪶 6. Maven Lifecycle Phases

### a) **Clean Lifecycle**

* `mvn clean` → removes old build files (`target/` directory).

### b) **Default (Build) Lifecycle**

Common phases:

| Phase      | Description                                      |
| ---------- | ------------------------------------------------ |
| `validate` | Check if project structure is correct            |
| `compile`  | Compile source code                              |
| `test`     | Run unit tests                                   |
| `package`  | Create `.jar` or `.war`                          |
| `verify`   | Run integration tests                            |
| `install`  | Install artifact to local repository             |
| `deploy`   | Upload to remote repository (Nexus, Artifactory) |

### c) **Site Lifecycle**

* `mvn site` → generates project documentation.

---

## 💻 7. Common Maven Commands

| Command                  | Description                                |
| ------------------------ | ------------------------------------------ |
| `mvn clean`              | Clean the `target` directory               |
| `mvn compile`            | Compile the project                        |
| `mvn test`               | Run unit tests                             |
| `mvn package`            | Create jar/war file under `target/`        |
| `mvn install`            | Copy built artifact to local repo          |
| `mvn deploy`             | Upload to remote repository                |
| `mvn verify`             | Run verification steps                     |
| `mvn dependency:tree`    | Shows dependency hierarchy                 |
| `mvn help:effective-pom` | Shows final POM including inherited values |

---

## 🧰 8. Maven Repositories

| Type                   | Location                            | Description                                      |
| ---------------------- | ----------------------------------- | ------------------------------------------------ |
| **Local Repository**   | `~/.m2/repository`                  | Cached JARs on your machine                      |
| **Central Repository** | Maven’s default public repo         | Hosted at `https://repo.maven.apache.org/maven2` |
| **Remote Repository**  | Company repo like Nexus/Artifactory | Used to host internal artifacts                  |

---

## 🧠 9. Dependency Scope

| Scope        | Available In              | Description                                |
| ------------ | ------------------------- | ------------------------------------------ |
| **compile**  | All phases                | Default scope, needed for build & runtime  |
| **provided** | Compile, not runtime      | Provided by container (e.g. `servlet-api`) |
| **runtime**  | Runtime only              | JDBC drivers, etc.                         |
| **test**     | Test only                 | JUnit, Mockito                             |
| **system**   | External JARs             | Rarely used                                |
| **import**   | For dependency management | Used in BOMs                               |

---

## 🧩 10. Maven Plugins

Maven uses **plugins** to execute build phases.

| Plugin                  | Purpose                 |
| ----------------------- | ----------------------- |
| `maven-compiler-plugin` | Compiles Java code      |
| `maven-surefire-plugin` | Runs unit tests         |
| `maven-jar-plugin`      | Packages JAR            |
| `maven-war-plugin`      | Packages WAR            |
| `maven-deploy-plugin`   | Uploads artifacts       |
| `maven-clean-plugin`    | Cleans target directory |

---

## 🧠 11. Maven Settings (Global Config)

Located in:

* **Global:** `${MAVEN_HOME}/conf/settings.xml`
* **User:** `~/.m2/settings.xml`

You can configure:

* Repository URLs
* Proxy
* Credentials
* Mirrors

Example:

```xml
<settings>
  <mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>https://nexus.company.com/repository/maven-public/</url>
    </mirror>
  </mirrors>
</settings>
```

---

## 🔄 12. Maven in CI/CD (DevOps Usage)

### In Jenkins:

* Add **Maven installation** under *Global Tool Configuration*.
* In Jenkinsfile:

  ```groovy
  pipeline {
      agent any
      tools {
          maven 'M3'
      }
      stages {
          stage('Build') {
              steps {
                  sh 'mvn clean package'
              }
          }
      }
  }
  ```
* Artifacts can then be deployed to **Nexus/Artifactory** automatically.

### In Docker:

```Dockerfile
FROM maven:3.9.6-eclipse-temurin-17
WORKDIR /app
COPY . .
RUN mvn clean package
CMD ["java", "-jar", "target/myapp.jar"]
```

---

## 🧮 13. Build Lifecycle Summary

| Command       | Internally Runs                            | Description                     |
| ------------- | ------------------------------------------ | ------------------------------- |
| `mvn package` | clean → compile → test → package           | Builds the JAR/WAR              |
| `mvn install` | clean → compile → test → package → install | Builds & installs in local repo |
| `mvn deploy`  | All + deploy                               | Uploads to remote repo          |

---

## 🧩 14. Interview Questions on Maven

| Question                                   | Example Answer                                                                                                      |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| What is Maven?                             | Maven is a build automation tool for Java projects that manages dependencies and builds using a standard lifecycle. |
| What is the POM file?                      | It’s an XML file that defines project structure, dependencies, and plugins.                                         |
| Difference between `install` and `deploy`? | `install` → adds artifact to local repo; `deploy` → uploads to remote repo.                                         |
| What is the `target/` folder?              | Maven’s output directory containing compiled code and packaged artifacts.                                           |
| What is a plugin in Maven?                 | A plugin executes a specific build goal or phase (like compiling, packaging, testing).                              |
| What is the role of the local repository?  | It stores downloaded dependencies to avoid repeated network downloads.                                              |
| What are Maven coordinates?                | groupId, artifactId, version — unique identifiers of an artifact.                                                   |
| How to skip tests?                         | `mvn package -DskipTests` or `-Dmaven.test.skip=true`.                                                              |

---

## 🧠 15. Common Issues

| Issue                    | Cause                        | Solution                                  |
| ------------------------ | ---------------------------- | ----------------------------------------- |
| `dependency not found`   | Wrong version or no internet | Check version or repo URL                 |
| `OutOfMemoryError`       | Large project build          | Increase memory: `MAVEN_OPTS="-Xmx1024m"` |
| `mvn: command not found` | Maven not installed          | Add Maven to PATH                         |
| Snapshot not updating    | Cached dependency            | `mvn clean install -U`                    |

---

## ✅ Summary

| Concept             | Description                                             |
| ------------------- | ------------------------------------------------------- |
| **Tool**            | Apache Maven                                            |
| **Used For**        | Build automation, dependency management                 |
| **Core File**       | `pom.xml`                                               |
| **Build Lifecycle** | clean → compile → test → package → install → deploy     |
| **Repositories**    | local, central, remote                                  |
| **Common Commands** | `mvn clean`, `mvn package`, `mvn install`, `mvn deploy` |
| **Integration**     | Jenkins, Nexus, Docker, Kubernetes                      |

---

Would you like me to give you a **one-page Maven quick reference sheet (cheat sheet)** next — with commands, folder structure, and lifecycle in compact form (for interviews)?
