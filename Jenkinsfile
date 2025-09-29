pipeline {
  agent any

  environment {
    IMAGE_BASE       = "<DOCKERHUB_USER>/microservices-backend"
    IMAGE_TAG        = "v${env.BUILD_NUMBER}"
    IMAGE_NAME       = "${env.IMAGE_BASE}:${env.IMAGE_TAG}"
    IMAGE_NAME_LATEST= "${env.IMAGE_BASE}:latest"
    DOCKERFILE_NAME  = "Dockerfile-packaged"
  }

  stages {
    stage('Build') {
      steps {
        checkout scm
        sh 'mvn -q compile'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn -q test'
        junit '**/target/surefire-reports/TEST-*.xml'
      }
    }
    stage('Package') {
      steps {
        sh 'mvn -q package -DskipTests'
      }
    }

    stage('Push images') {
      steps {
        echo "Skip: no DockerHub push configured"
      }
    }

    stage('Trigger kubernetes') {
      steps {
        echo "Skip: no Kubernetes cluster configured"
      }
    }
  }
}

