pipeline{
    agent any
    // environment{
    //     VERSION = "${env.BUILD_ID}"
    // }
        stages{
            stage("1- Code Quality check-SonarQube"){
                steps{
                    script{
                       withSonarQubeEnv(credentialsId: 'sonar-token') {
                       sh 'chmod +x gradlew'
                       sh './gradlew sonarqube'
                        }
                       timeout(time: 10, unit: 'MINUTES') {
                       def ab = waitForQualityGate() //ab is a variable and storing qualit gate function
                       if (ab.status != 'OK') {   //checking status of variable
                        error "Pipeline failed due to ${ab.status}"
                       }
                       } 
                    }
                }
            }
    //         stage("Creating docker image ")
    //         steps{
    //             script{
    //               sh '''
    //               docker build -t http://3.14.177.29:8083/springapp:${VERSION} .
                  
    //               docker
    //               '''
                 
    //             }
    //         }
         }
     }
