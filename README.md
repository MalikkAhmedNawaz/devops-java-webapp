**DevOps Java Application Project Documentation**

---

### **Project Title:** End-to-End DevOps Pipeline with Jenkins, Docker, and Kubernetes for a Java Web Application

### **Project Overview:**

This project demonstrates the implementation of a complete CI/CD pipeline using Jenkins, Docker, and Kubernetes to automate the building, testing, packaging, and deployment of a Java-based web application. It simulates a real-world DevOps environment and highlights essential industry tools and practices for software delivery.

---

### **Technologies Used:**

* **Programming Language:** Java
* **Build Tool:** Maven
* **CI/CD Tool:** Jenkins
* **Containerization:** Docker
* **Container Registry:** Docker Hub
* **Orchestration Platform:** Kubernetes (Minikube)
* **Version Control:** Git & GitHub

---

### **System Requirements:**

* Kali Linux / Debian OS
* Docker installed and configured
* Minikube with Kubernetes v1.32.0+
* Jenkins (latest stable)
* Java JDK and Maven

---

### **Project Workflow Breakdown:**

#### **Day 1 – Java Application Setup**

* Created a basic Maven Java web application with a simple `Hello, World!` servlet.
* Used `pom.xml` to define dependencies and build configurations.
* Tested the application locally to ensure successful build and run.

#### **Day 2 – Docker Integration**

* Wrote a `Dockerfile` to containerize the Java web app.
* Built the Docker image locally and tested it using `docker run`.
* Tagged and pushed the image to Docker Hub under a personal repository.

#### **Day 3 – Jenkins CI Integration**

* Installed Jenkins and created a Freestyle project.
* Configured GitHub repository polling.
* Defined Jenkins pipeline stages to:

  * Clone the repo
  * Run Maven build
  * Build Docker image
  * Push Docker image to Docker Hub

#### **Day 4 – Kubernetes Deployment Setup**

* Wrote `kube-deploy.yaml` with Kubernetes Deployment and NodePort Service.
* Used Minikube to deploy the Docker image to a Kubernetes pod.
* Exposed the app on a NodePort and verified via `minikube service`.

#### **Day 5 – Jenkins Docker Push Automation**

* Secured Docker Hub credentials in Jenkins.
* Updated the Jenkins pipeline to push Docker image to Docker Hub automatically.
* Validated the pushed image and cleaned up old images with `docker system prune`.

#### **Day 6 – Jenkins Deploys to Kubernetes (CD)**

* Enabled Jenkins to access Kubernetes using `kubectl`.
* Updated Jenkinsfile to include `kubectl apply` command.
* Ensured Jenkins user had permissions by copying `.kube/config`.
* Final pipeline now triggers full CI/CD from code push to live deployment.

---

### **Jenkinsfile Used:**

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "malikkahmednawaz786/myapp-image:latest"
    }

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/your-username/devops-java-app.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f kube-deploy.yaml'
            }
        }
    }
}
```

---

### **Outcomes and Achievements:**

* Built a fully functional CI/CD pipeline from scratch.
* Automated every step from code commit to live deployment.
* Used industry-standard tools and best practices.
* Understood containerization and orchestration deeply.
* Learned to troubleshoot Minikube, Docker storage issues, Jenkins credentials, and deployment failures.

---


