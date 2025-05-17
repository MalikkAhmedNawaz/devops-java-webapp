pipeline {
  agent any

  tools {
    maven 'Maven 3'
  }

  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/MalikkAhmedNawaz/devops-java-webapp.git'
      }
    }

    stage('Build with Maven') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t myapp-image .'
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker run -d -p 8081:8080 --name myapp-container myapp-image || true'
      }
    }
  }
}

