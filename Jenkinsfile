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
        sh 'python app.py > .logs 2>&1 &'
        sh 'cat .logs'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh 'pkill -f app.py'
      }
    }

  }
  post {
    always {
      junit 'test_reports/*.xml'
      sh 'cat .logs'
    }

  }
}