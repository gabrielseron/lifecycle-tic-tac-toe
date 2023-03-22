pipeline {
  agent any

  tools {nodejs "node"}

  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy Should Be Here'
      }
    }
  }
}