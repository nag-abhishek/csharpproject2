pipeline {
  agent any
  environment {
   scannerHome = tool name: 'sonar_scanner_dotnet', type: 'hudson.plugins.sonar.MsBuildSQRunnerInstallation'
    registry = "abhishek3150089/netprjct"
    registryCredential = "dockerHub"
    dockerImage = ""
  }
  stages {
      

stage('restore nuget'){
 steps{
     bat 'dotnet restore WebApplication4.sln'
}
}
stage('sonarQube Analysis'){
    steps{
        script {
       
           withSonarQubeEnv("Test_Sonar") {
           bat """
           ${scannerHome}\\SonarScanner.MSBuild.exe  begin /k:"my.project" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="9d26a2e7700339dd08509b738568b49c2ababd8b"
          
          """
               }
           }
    }
}
stage('build'){
steps{
bat "dotnet build WebApplication4.sln -c Release -o app/publish"
    
}
}
stage('sonarQube Analysis end'){
    steps{
        script {
       
           withSonarQubeEnv("Test_Sonar") {
           bat """
           ${scannerHome}\\SonarScanner.MSBuild.exe  end /d:sonar.login="9d26a2e7700339dd08509b738568b49c2ababd8b"
          
          """
               }
           }
    }
}
stage('dockerimage'){
    steps{
         script {
        bat  "docker build --no-cache -t i_abhishekchadha_master:${BUILD_NUMBER} -f WebApplication4/Dockerfile ."
        }
    }
}
    stage('Push DTR') {
      steps{
        script {
       bat "docker tag i_abhishekchadha_master:${BUILD_NUMBER}  dtr.nagarro.com:443/i_abhishekchadha_master:${BUILD_NUMBER}"
       bat "docker push  dtr.nagarro.com:443/i_abhishekchadha_master:${BUILD_NUMBER}"
          }
        }
      }
        stage('Docker Deployment') {
      steps{
        script {
       bat "docker run --name c_abhishekchadha_master -d -p 6000:80  dtr.nagarro.com:443/i_abhishekchadha_master:${BUILD_NUMBER}"
     
          }
        }
      }
    
}
}
