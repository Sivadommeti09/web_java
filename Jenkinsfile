pipeline {
  agent any
  stages {
    stage('docker') {
      steps {
        sh '''docker
image maven
args \'-v $HOME/.m2:/root/.m2\'
'''
      }
    }

    stage('test') {
      steps {
        echo ' withSonarQubeEnv(\'sonarserver\')'
      }
    }

  }
  environment {
    Docker_tag = 'getDockerTag()'
  }
}