pipeline {
  agent {
    docker {
      image 'node:19-alpine3.16'
    }
  }

  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy Here'
      }
    }
  }
}