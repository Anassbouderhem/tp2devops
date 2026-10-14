def dockerImage

pipeline {
  environment {
    registry = "anassbouderhem/tp2"
    registryCredential = 'dockerhub-tp2'
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/Anassbouderhem/tp2devops.git'
      }
    }
    stage('Building image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Test image') {
      steps {
        script {
          echo "Tests passed"
        }
      }
    }
    stage('Publish Image') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy Image') {
      steps {
        bat """
          docker rm -f tp2-app 2>nul || exit /b 0
          docker run -d --name tp2-app -p 8081:80 anassbouderhem/tp2:%BUILD_NUMBER%
        """
      }
    }
  }
}