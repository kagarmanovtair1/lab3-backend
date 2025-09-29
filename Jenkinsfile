pipeline {
  agent none

  options {
    skipStagesAfterUnstable()
    skipDefaultCheckout()
  }

  environment {
    IMAGE_BASE       = "<DOCKERHUB_USER>/microservices-backend"
    IMAGE_TAG        = "v${env.BUILD_NUMBER}"
    IMAGE_NAME       = "${env.IMAGE_BASE}:${env.IMAGE_TAG}"
    IMAGE_NAME_LATEST= "${env.IMAGE_BASE}:latest"
    DOCKERFILE_NAME  = "Dockerfile-packaged"
  }

  stages {
    stage("Prepare container") {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-11'
          args '-v $HOME/.m2:/root/.m2'
        }
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
      }
    }

    stage('Push images') {
      agent any
      when { branch 'master' }
      steps {
        script {
          def dockerImage = docker.build("${env.IMAGE_NAME}", "-f ${env.DOCKERFILE_NAME} .")
          docker.withRegistry('', 'dockerhub-creds') {
            dockerImage.push()
            dockerImage.push("latest")
          }
        }
        sh "docker rmi ${env.IMAGE_NAME} ${env.IMAGE_NAME_LATEST} || true"
      }
    }

    stage('Trigger kubernetes') {
      agent any
      when { branch 'master' }
      steps {
        echo "Skip: no Kubernetes cluster configured"
      }
    }
  }
}
