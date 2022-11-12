pipeline {
  agent {
    docker {
      image 'node:lts-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        dependencyCheck(additionalArguments: '--format HTML --format XML ')
      }
    }

    stage('Test') {
      environment {
        CI = 'true'
      }
      steps {
        sh './jenkins/scripts/test.sh'
        dependencyCheckPublisher()
      }
    }

    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input 'Finished using the web site? (Select "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }

  }
}