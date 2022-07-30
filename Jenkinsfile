pipeline { agent any 
    tools{ 
        maven "Maven 3.6.3"}
        stages { 
            stage('SCM'){
                steps{
                    git 'https://github.com/Mahoney0101/spring-petclinic.git'
                }
            }
            stage('Build') { 
                steps { 
                    bat "mvn -Dmaven.test.failure.ignore=true clean package" } 
            }
            stage('Parallel Junit and SonarQube'){
                parallel{
                    stage('JUnit Tests'){
                        steps{
                            junit '**/target/surefire-reports/TEST-*.xml'
                            archiveArtifacts 'target/*.jar'
                        }
                    }
                    stage('SonarQube analysis'){
                        steps {
                            withSonarQubeEnv() {
                                bat "mvn -Dsonar.qualitygate=true sonar:sonar"
                            }
                        }
                    }
                }
            }
            stage('Build Docker Image'){
                steps{
                    bat 'docker build -t sivadommeti09/devops-training:$Docker_tag:latest .'
                }
            }
            stage('Push Docker Image To DockerHub'){
                steps{
                    withCredentials([string(credentialsId: 'docker_password', variable: 'docker')]) {
                    bat "docker login -u sivadommeti09 -p ${dockerHubPwd}"
                    }
                    bat 'docker push sivadommeti09/devops-training:$Docker_tag:latest'
                }
            }
            stage('Deploy Docker Image To EC2 Instance'){
                steps{
                    script{
                        def deploy = "docker run --publish 80:8080 -d --name petclinic mahoney0101/spring-petclinic:latest"
                        def stop = "docker stop petclinic || true"
                        def remove = "docker rm petclinic || true"
                        def instance = "ec2-user@ec2-43-205-129-58.ap-south-1.compute.amazonaws.com"
                        sshagent(['ec2-ssh-id']) {                                                
                        sh "ssh -T -o StrictHostKeyChecking=no ${instance} ${stop}"
                        sh "ssh -T -o StrictHostKeyChecking=no ${instance} ${remove}"
                        sh "ssh -T -o StrictHostKeyChecking=no ${instance} ${deploy}"
                        }
                    }
                }
            }
            stage('Email Build Status'){
                steps{
                    emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} More info at: ${env.BUILD_URL}", recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}" 
                }
            }
        }
}
