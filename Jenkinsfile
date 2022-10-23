pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
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
            stage("Building and pushing docker image "){
            steps{
                script{
                  withCredentials([string(credentialsId: 'nexus_pass', variable: 'docker_password')]) {
                  sh '''
                     docker build -t 3.131.154.78:8083/springapp:${VERSION} .
                     docker login -u admin -p $docker_pass 3.131.154.78:8083
                     docker push 3.131.154.78:8083/springapp:${VERSION}
                     docker rmi 3.131.154.78:8083/springapp:${VERSION}
                  '''}       
                }
            }
         }
     }
}