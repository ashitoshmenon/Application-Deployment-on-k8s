pipeline{
    agent any
        stages{
            stage("1- Code Quality check-SonarQube"){
                steps{
                    script{
                       withSonarQubeEnv(credentialsId: 'sonar-jenkins') {
                       sh 'chmod +x gradlew'
                       sh './gradlew sonarqube'
                        }
                       timeout(time: 1, unit: 'HOURS')
                       def ab = waitForQualityGate() //ab is a variable and storing qualit gate function
                       if (ab.status != 'OK') {   //checking status of variable
                        error "Pipeline failed due to ${ab.status}"
                       } 
                    }
                }
            }
        }
    }
