pipeline {
  agent any
  stages {
    stage('docker') {
      steps {
        sh '''maven:3.8.1-adoptopenjdk-11\'
args \'-v $HOME/.m2:/root/.m2\''''
      }
    }

    stage('test') {
      steps {
        echo 'Quality Gate Status Check'
        echo 'withSonarQubeEnv(\'sonarserver\')'
        sh 'mvn sonar:sonar'
        echo 'timeout(time: 1, unit: \'HOURS\')'
        sh 'mvn clean install'
      }
    }

    stage('build') {
      steps {
        sh '''docker build . -t sivadommeti09/devops-training:$Docker_tag
withCredentials([string(credentialsId: \'docker_password\', variable: \'docker_password\')]) {
    // some block
}'''
        sh '''\'\'\'docker login -u sivadommeti09 -p $docker_password
                docker push sivadommeti09/devops-training:$Docker_tag'''
      }
    }

  }
}