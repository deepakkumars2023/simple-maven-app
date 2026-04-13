Here’s your **complete, clean, professional README.md** with **Jenkins + Docker + Kubernetes (Full CI/CD)** 👇 — ready for submission/demo.

---

# 🚀 Jenkins – Docker – Kubernetes CI/CD Pipeline (Java Console App)

## 📌 Project Overview

This project demonstrates a **complete CI/CD pipeline** using:

* Jenkins (Continuous Integration)
* Docker (Containerization)
* Kubernetes (Deployment & Scaling)
* Maven + JUnit (Build & Testing)

A simple Java console application is:
✔ Built using Maven
✔ Tested using JUnit
✔ Containerized using Docker
✔ Pushed to Docker Hub
✔ Deployed on Kubernetes

---

## 🛠️ Tech Stack

* Java (JDK 17)
* Maven
* JUnit
* Jenkins
* Docker
* Kubernetes
* GitHub
* VS Code

---

## 📁 Project Structure

```id="7y2y6t"
simple-java-app/
│
├── src/
│   ├── main/java/com/demo/App.java
│   └── test/java/com/demo/AppTest.java
│
├── pom.xml
├── Dockerfile
├── deployment.yaml
└── Jenkinsfile
```

---

## 💻 Step 1: Create Maven Project

### 📄 App.java

```java id="mt9e41"
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

    public static void main(String[] args) throws Exception {
        System.out.println("Java CI/CD Application Started...");
        while(true){
            Thread.sleep(5000);
            System.out.println("Hello from CI/CD Pipeline!");
        }
    }
}
```

---

## 🧪 Step 2: JUnit Tests

### 📄 AppTest.java

```java id="7qrskz"
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

```xml id="2h6iwq"
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

```bash id="m8lb9g"
mvn clean package
```

📦 Output:

```id="v12glv"
target/simple-java-app-1.0.jar
```

---

## 🐳 Step 5: Dockerfile

```dockerfile id="9nd9y1"
FROM eclipse-temurin:17-jdk

WORKDIR /app

COPY target/*.jar app.jar

CMD ["java", "-jar", "app.jar"]
```

---

## ☸️ Step 6: Kubernetes Deployment

### 📄 deployment.yaml

```yaml id="3yqzqk"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: java-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: java-app
  template:
    metadata:
      labels:
        app: java-app
    spec:
      containers:
        - name: java-app
          image: yourdockerhubusername/java-app:latest
---
apiVersion: v1
kind: Service
metadata:
  name: java-service
spec:
  type: NodePort
  selector:
    app: java-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30008
```

---

## ⚙️ Step 7: Setup Docker Desktop & Kubernetes

Using Docker Desktop:

1. Open Docker Desktop
2. Go to **Settings → Kubernetes**
3. Enable **Enable Kubernetes**
4. Click **Apply & Restart**

---

## 🔐 Step 8: Add Credentials in Jenkins

### 📍 Go to:

**Manage Jenkins → Manage Credentials → Global**

---

### 🔑 1. DockerHub Credentials

| Field | Value                  |
| ----- | ---------------------- |
| Kind  | Username with password |
| ID    | `dockerhub-creds`      |

---

### 📦 2. Kubernetes Config File

📍 Path:

```id="aqz5pn"
C:\Users\<username>\.kube\config
```

Upload as:

* Kind: Secret file
* ID: `kuberconfig`

---

## ⚙️ Step 9: Jenkins Pipeline

### 📄 Jenkinsfile

```groovy id="96h0g3"
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

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(
                    credentialsId: 'kuberconfig',
                    variable: 'KUBECONFIG'
                )]) {
                    bat '''
                    set KUBECONFIG=%KUBECONFIG%
                    kubectl apply -f deployment.yaml
                    '''
                }
            }
        }
    }
}
```

---

## ▶️ Step 10: Run Pipeline

* Go to Jenkins Dashboard
* Click **Build Now**

---

## 🔍 Step 11: Verify Deployment

```bash id="dw4zmr"
kubectl get pods
kubectl get svc
```

---

## 🔄 CI/CD Workflow

### 🔹 CI (Continuous Integration)

* Code pushed to GitHub
* Jenkins builds using Maven
* JUnit tests executed

### 🔹 CD (Continuous Deployment)

* Docker image built
* Image pushed to Docker Hub
* Kubernetes deploys application

---

## 🎯 Features

✔ Maven project structure
✔ JUnit testing
✔ Automated CI/CD pipeline
✔ Docker containerization
✔ Kubernetes deployment (2 replicas)
✔ Secure credential handling

---

## 👨‍💻 Author

**Your Name**

---

## ⭐ Bonus (For Viva)

**Common Errors & Fixes**

| Issue                   | Fix                      |
| ----------------------- | ------------------------ |
| JAVA_HOME not set       | Configure JDK in Jenkins |
| JAR not found           | Use `COPY target/*.jar`  |
| Docker push fails       | Check credentials        |
| Kubernetes deploy fails | Verify kubeconfig        |

---

If you want, I can next:
👉 Convert this into **PPT slides (very high scoring)**
👉 Or give **viva explanation script (what to speak)** 🎤
