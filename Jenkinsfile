pipeline {
  agent any
  stages {
    stage('docker') {
      steps {
        echo 'image \'maven:3-openjdk-11\''
      }
    }

    stage('test') {
      steps {
        echo 'Quality Gate Status Check'
      }
    }

    stage('sonar') {
      steps {
        echo 'withSonarQubeEnv(\'sonarserver\') {  			      sh "mvn clean sonar:sonar"                        	     	} 			      timeout(time: 1, unit: \'HOURS\') { 			      def qg = waitForQualityGate() 				      if (qg.status != \'OK\') { 					   error "Pipeline aborted due to quality gate failure: ${qg.status}"'
      }
    }

  }
}