pipeline {
  agent any

  tools {
    maven 'Maven 3'
  }

  stages {
    stage('Clone') {
      steps {
        git branch: 'main', url: 'https://github.com/MalikkAhmedNawaz/devops-java-webapp'
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
        script {
            sh 'docker rm -f myapp-container || true'
            sh 'docker run -d -p 8081:8080 --name myapp-container myapp-image'
        }
    }
}

    stage('Docker Push') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            sh '''
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                docker tag myapp-image $DOCKER_USERNAME/myapp-image:latest
                docker push $DOCKER_USERNAME/myapp-image:latest
            '''
        }
    }
}
