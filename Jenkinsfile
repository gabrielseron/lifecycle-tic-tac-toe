pipeline {
  agent {
    docker {
      image 'node:14'
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