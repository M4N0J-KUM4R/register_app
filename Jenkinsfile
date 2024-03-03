pipeline {
    agent { label 'Jenkins_Agent' 
     }
    tools {
      jdk 'Java17'
      maven 'Maven3'
     }
  stages{
     stage("Cleanup Workspace"){
             steps{
             cleanWs()
             }
      }
     stage("Checkout from SCM"){
             steps{
             git branch: 'main', credentialsId: 'Github', url: 'https://github.com/M4N0J-KUM4R/register_app'
             }
      }
     stage("Build Application"){
             steps{
             sh "mvn clean package"
             }
      }
     stage("Test Application"){
             steps{
             sh "mvn test"
             }
      }
     stage("SonarQube Analysis"){
             steps{
                 script{
                     withSonarQubeEnv(credentialsId: 'Jenkins_SonarQube_Token') {
                         sh "mvn sonar:sonar"
                     }
                 }
             }
     }
     stage("Quality Gate"){
             steps{
                 script{
                     waitforQualityGate abortpipeline: false, credentialsId: 'Jenkins_SonarQube_Token'
                 }
             }            
     }
   }
 }
