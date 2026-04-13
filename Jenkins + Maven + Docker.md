Here’s your **complete README.md WITHOUT Kubernetes** (pure CI pipeline: Jenkins + Maven + Docker) — clean and perfect for submission/demo 👇

---

# 🚀 Jenkins – Docker CI/CD Pipeline (Java Console App)

## 📌 Project Overview

This project demonstrates a **basic CI/CD pipeline** using:

* Jenkins (Continuous Integration)
* Docker (Containerization)
* Maven + JUnit (Build & Testing)

A simple Java console application is:
✔ Built using Maven
✔ Tested using JUnit
✔ Containerized using Docker
✔ Pushed to Docker Hub

---

## 🛠️ Tech Stack

* Java (JDK 17)
* Maven
* JUnit
* Jenkins
* Docker
* GitHub
* VS Code

---

## 📁 Project Structure

```id="q3pt9j"
simple-java-app/
│
├── src/
│   ├── main/java/com/demo/App.java
│   └── test/java/com/demo/AppTest.java
│
├── pom.xml
├── Dockerfile
└── Jenkinsfile
```

---

## 💻 Step 1: Create Maven Project

### 📄 App.java

```java id="v5gx39"
package com.demo;

public class App {

    public static int add(int a, int b) {
        return a + b;
    }

    public static String evenOdd(int n) {
        return (n % 2 == 0) ? "Even" : "Odd";
    }

    public static int multiply(int a, int b) {
        return a * b;
    }

    public static int factorial(int n) {
        int fact = 1;
        for(int i = 1; i <= n; i++) {
            fact *= i;
        }
        return fact;
    }

    public static boolean isPrime(int n) {
        if(n <= 1) return false;
        for(int i = 2; i <= n/2; i++) {
            if(n % i == 0) return false;
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println("Java CI/CD Application Running...");
    }
}
```

---

## 🧪 Step 2: JUnit Tests

### 📄 AppTest.java

```java id="sk9y6i"
package com.demo;

import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class AppTest {

    @Test
    public void testAdd() {
        assertEquals(5, App.add(2,3));
    }

    @Test
    public void testEvenOdd() {
        assertEquals("Even", App.evenOdd(4));
    }

    @Test
    public void testMultiply() {
        assertEquals(6, App.multiply(2,3));
    }

    @Test
    public void testFactorial() {
        assertEquals(120, App.factorial(5));
    }

    @Test
    public void testPrime() {
        assertTrue(App.isPrime(7));
    }
}
```

---

## 📄 Step 3: pom.xml (Key Parts)

```xml id="rfk74c"
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.9.3</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0</version>
        </plugin>
    </plugins>
</build>
```

---

## 🧪 Step 4: Build Locally

```bash id="v3l51q"
mvn clean package
```

📦 Output:

```id="d5qz2q"
target/simple-java-app-1.0.jar
```

---

## 🐳 Step 5: Dockerfile

```dockerfile id="azm8k1"
FROM eclipse-temurin:17-jdk

WORKDIR /app

COPY target/*.jar app.jar

CMD ["java", "-jar", "app.jar"]
```

---

## 📤 Step 6: Push Code to GitHub

```bash id="hlk7v0"
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

---

## 🔐 Step 7: Add Credentials in Jenkins

### 📍 Navigate:

**Manage Jenkins → Manage Credentials → Global**

---

### 🔑 Add DockerHub Credentials

| Field    | Value                    |
| -------- | ------------------------ |
| Kind     | Username with password   |
| Username | Your Docker Hub username |
| Password | Your Docker Hub password |
| ID       | `dockerhub-creds`        |

---

## ⚙️ Step 8: Jenkins Pipeline

### 📄 Jenkinsfile

```groovy id="r8l6cc"
pipeline {
    agent any

    tools {
        jdk 'Java21'
    }

    environment {
        DOCKER_IMAGE = "yourdockerhubusername/java-app:latest"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                url: '<your-repo-url>'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    bat """
                    docker login -u %USER% -p %PASS%
                    docker push %DOCKER_IMAGE%
                    """
                }
            }
        }
    }
}
```

---

## ▶️ Step 9: Run Pipeline

* Go to Jenkins Dashboard
* Click **Build Now**

---

## 🔄 CI Workflow

### 🔹 Continuous Integration

* Developer writes Java code
* Pushes to GitHub
* Jenkins builds project using Maven
* JUnit tests are executed
* Docker image is created
* Image is pushed to Docker Hub

---

## 📊 Jenkins Stage View

Stages visible in Jenkins:

1. Clone Repository
2. Build (Maven)
3. Test (JUnit)
4. Docker Build
5. Docker Push

---

## 🎯 Features

✔ Addition operation
✔ Even/Odd check
✔ Multiplication
✔ Factorial calculation
✔ Prime number check
✔ JUnit test cases
✔ Automated CI pipeline
✔ Docker containerization

---

## 👨‍💻 Author

**Your Name**

---

## ⭐ Bonus (Common Errors & Fixes)

| Issue              | Fix                       |
| ------------------ | ------------------------- |
| JAVA_HOME not set  | Configure JDK in Jenkins  |
| JAR not found      | Use `COPY target/*.jar`   |
| Docker login fails | Check credentials         |
| Build fails        | Verify Maven installation |

---

If you want next:
👉 I can combine **both (with & without Kubernetes) into one report**
👉 Or create **Viva explanation (what to say step-by-step)** 🎤
