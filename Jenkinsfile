pipeline {
  agent any
  stages {
    stage('log tool version') {
      parallel {
        stage('log tool ver') {
          steps {
            sh '''mvn --version
git --version
java --version'''
          }
        }

        stage('check for pom') {
          steps {
            fileExists 'pom.xml'
          }
        }

      }
    }

    stage('build with maven') {
      steps {
        sh 'mvn compile test package'
      }
    }

    stage('post build steps') {
      steps {
        writeFile(file: 'status.txt', text: 'Hey it worked!')
      }
    }

  }
}