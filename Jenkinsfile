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
                  withCredentials([string(credentialsId: 'nexus_pass', variable: 'docker_pass')]) {
                  sh '''
                     docker build -t 3.131.154.78:8083/springapp:${VERSION} .
                     docker login -u admin -p $docker_pass 3.131.154.78:8083
                     docker push 3.131.154.78:8083/springapp:${VERSION}
                     docker rmi 3.131.154.78:8083/springapp:${VERSION}
                  '''}       
                }
            }
         }
    // ************* Datree Stage **********************     
            stage('Identify mis-config in helm using datree'){
            steps{
                script{    
                  dir('kubernetes/'){
                    sh 'helm datree test myapp/'
                  }     
                }
            }
         }
    // ************* Datree Stage ********************** 
     }
//     post { //*********Configuring email server**********
// 		always {
// 			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "ackermankevi@outlook.com";  
// 		}
// 	}
}