pipeline{
    agent any

        stages{
            
            stage("1- Code Quality check-SonarQube"){
                agent {
                    docker {
                        sh 'chmod 777 /var/run/docker.sock'
                        image 'openjdk:11'
                    }
                }
                steps{
                    script{
                        withSonarQubeEnv(credentialsId: 'sonar-jenkins-pwd') {
                            sh 'chmod+x gradlew'
                            sh './gradlew sonarqube'
                          }

                    }
                }
            }
        }
    }
