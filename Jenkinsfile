pipeline {
  agent {
    docker {
      image 'node:14'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
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