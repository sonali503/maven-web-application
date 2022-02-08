node{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
    pipelineTriggers([pollSCM('* * * * *')])])
    
    
    
    def mavenHome = tool name:"maven3.8.4"
    
stage('CheckoutCode'){
    
git branch: 'development', credentialsId: 'f1295f6a-b318-45da-ae39-1fbfe695a634', url: 'https://github.com/sonali503/maven-web-application.git'    
}

stage('Build'){
    
sh "${mavenHome}/bin/mvn clean package"

}
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('UploadArtifactIntoNexusRepo'){
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployAppIntoTomcat'){
sshagent(['f6fdd961-faf5-408b-a8a5-6f4b8e12f6d1']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.86.231://opt/apache-tomcat-9.0.58/webapps/"
}
}
}
