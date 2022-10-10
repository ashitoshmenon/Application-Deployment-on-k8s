pipeline{
    agent any
        stages{
            
            stage("Code Quality check-SonarQube")
                
                steps{
                    
                    script{
                       withSonarQubeEnv(credentialsId: 'sonar-jenkins') {
                       sh 'chmod +x gradlew'
                       sh './gradlew sonarqube'
                        } 
                    }
                }
            }
        }
