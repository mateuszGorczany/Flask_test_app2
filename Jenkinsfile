pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building image...'
        script {
          docker.build("${imageName}")
        }

      }
    }

    stage('Test') {
      agent {
        docker {
          image "${imageName}"
          args '--publish 2115:1337'
        }

      }
      post {
        always {
          junit 'test_reports/*.xml'
        }

      }
      steps {
        sh 'python test.py'
      }
    }

    stage('Deliver') {
      agent {
        docker {
          image "${imageName}"
          args '--publish 2115:1337'
        }

      }
      post {
        always {
          archiveArtifacts 'flask.log'
          sh 'cat flask.log'
        }

      }
      steps {
        sh 'python app.py > flask.log 2>&1 &'
        sh 'cat flask.log'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh 'pkill -f app.py'
      }
    }

    stage('Deploy') {
      agent any
      steps {
        echo 'Publishing created dockerimage on Dockerhub...'
        script {
          docker.withRegistry('', "${registryCredential}")
          {
            imageToDeploy = docker.image("${imageName}")
            imageToDeploy.push()
            echo 'Image pushed to your dockerhub repository'}
          }

        }
      }

      stage('Cleaning') {
        steps {
          sh 'docker image prune'
          echo 'Pruning completed'
        }
      }

    }
    environment {
      registry = 'mgorczany/docker-flask-test'
      registryCredential = 'dockerhub'
      imageName = "${registry}:${env.BUILD_ID}"
    }
    post {
      always {
        sh "docker image rm \$(docker image ls | grep -F -e ${registry} | awk \'NR>1 {print \$3}\') || echo \"\""
        echo 'Finished - last build is still available in the docker:dind'
      }

    }
  }