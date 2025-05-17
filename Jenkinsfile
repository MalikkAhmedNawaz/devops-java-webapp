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
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
          sh 'docker tag myapp-image $DOCKER_USER/myapp-image:latest'
          sh 'docker push $DOCKER_USER/myapp-image:latest'
        }
      }
    }
  }
}
