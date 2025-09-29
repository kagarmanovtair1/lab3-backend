pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Build stage: compiling project...'
      }
    }
    stage('Test') {
      steps {
        echo 'Test stage: running tests...'
      }
    }
    stage('Package') {
      steps {
        echo 'Package stage: creating JAR...'
      }
    }
    stage('Push images') {
      steps {
        echo 'Push stage: skipping DockerHub'
      }
    }
    stage('Trigger kubernetes') {
      steps {
        echo 'Kubernetes stage: skipping deploy'
      }
    }
  }
}

