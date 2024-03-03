pipeline {
    agent { label 'Jenkins_Agent' 
     }
    tools {
      jdk 'Java17'
      maven 'Maven3'
     }
    environment {
        APP_NAME = "register_app_pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "m4n0jkum4r"
        DOCKER_PASS = 'Jenkins_Docker'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
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
                     waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins_SonarQube_Token'
                 }
             }            
     }
     stage("Build & Push Docker Image"){
             steps{
                 script{
                     docker.withRegistry('',DOCKER_PASS) {
                         docker_image = docker.build "${IMAGE_NAME}"
                     }
                     docker.withRegistry('',DOCKER_PASS) {
                         docker_image.push("${IMAGE_TAG}")
                         docker_image.push('latest')
                     }            
                 }
             }
     }
     stage("Trivy Scan"){
             steps{
                 script{
                     sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image m4n0jkum4r/register_app_pipeline:latest --no-progress --scanners vuln --exit-code 0 --severity HIGH,CRITICAL --format table')
                 }
             }
     }
     stage("CleanUp Artifacts"){
             steps{
                 script{
                     sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                     sh "docker rmi ${IMAGE_NAME}:latest"
                     }
                 }
             }
     }
   }
 }
