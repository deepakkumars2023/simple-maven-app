🚀 Static Web App CI/CD (Steps with Code – Updated)
✅ Step 1: Create Static Web Page
📄 index.html
<!DOCTYPE html>
<html>
<head>
    <title>Static Web App</title>
</head>
<body>

<h1>Student Results</h1>
<p>Name: John | Marks: 85 | Grade: A</p>

<h1>Rivera Events</h1>
<ul>
<li>Dance</li>
<li>Music</li>
<li>Drama</li>
<li>Gaming</li>
<li>Fashion Show</li>
</ul>

<h1>Conference Details</h1>
<p>AI Conference - Speaker: Dr. XYZ</p>

<h1>Price List</h1>
<ul>
<li>Pen - ₹10</li>
<li>Notebook - ₹50</li>
</ul>

<h1>Student Timetable</h1>
<p>Monday: Math, Science</p>

</body>
</html>
✅ Step 2: Push Code to GitHub
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
✅ Step 3: Create Dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80
✅ Step 4: Create Kubernetes Deployment
📄 deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
        - name: web-app
          image: <dockerhub-username>/web-app:latest
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
✅ Step 5: Enable Kubernetes
Open Docker Desktop
Go to Settings → Kubernetes
Enable Kubernetes
✅ Step 6: Generate kubeconfig
kubectl config view --raw > newconfig
✅ Step 7: Add Credentials in Jenkins
DockerHub credentials → dockerhub-creds
Kubernetes config → kuberconfig
✅ Step 8: Create Jenkins Pipeline
📄 Jenkinsfile
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "<dockerhub-username>/web-app:latest"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                url: '<your-repo-url>'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Push Docker Image') {
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
                    kubectl config set-cluster docker-desktop --insecure-skip-tls-verify=true
                    kubectl apply -f deployment.yaml --validate=false
                    '''
                }
            }
        }

        stage('Verify') {
            steps {
                withCredentials([file(
                    credentialsId: 'kuberconfig',
                    variable: 'KUBECONFIG'
                )]) {
                    bat '''
                    set KUBECONFIG=%KUBECONFIG%
                    kubectl get pods
                    kubectl get svc
                    '''
                }
            }
        }
    }
}
✅ Step 9: Run Pipeline
Open Jenkins
Click Build Now
✅ Step 10: Verify Deployment
kubectl get pods
kubectl get svc
✅ Step 11: Access Application
http://localhost:30007
