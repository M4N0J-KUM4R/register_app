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
             cleanws()
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
   }
}
