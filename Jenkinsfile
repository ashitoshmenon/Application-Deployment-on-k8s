pipeline{
    agent any

        stages{
            
            stage("1- Code Quality check-SonarQube"){
                agent {
                    docker {
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
