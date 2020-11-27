pipeline {
  agent {
    dockerfile {
      filename 'Dockerfile'
      args '--publish 2115:1337'
    }

  }
  stages {
    stage('Test') {
      steps {
        sh 'python test.py'
      }
    }

    stage('Deliver') {
      steps {
        sh 'python app.py'
      }
    }

  }
}