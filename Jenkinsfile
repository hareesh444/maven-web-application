node{

def mavenHome=tool name: "maven3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutcode'){
git branch: 'development', credentialsId: '770456f3-6e6a-4181-9c82-a4a3e04a87ad', url: 'https://github.com/hareesh444/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSoanrQubeReport')
 {
 sh  "${mavenHome}/bin/mvn sonar:sonar"
 }
 
 stage('UploadArtifactintoNexus')
 {
 sh  "${mavenHome}/bin/mvn deploy"
 }
 
  stage('DeployAppintoTomcat')
 {
 sshagent(['88a376bb-a4e6-458d-8c70-52da466cf06b']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.166.208.102:/opt/apache-tomcat-9.0.48/webapps"
 }
}

 stage('SendEmailNotification')
 {
   emailext body: '''Build is over..!

   Thanks & Regards,

   Hareesh''', subject: 'Build is over..!', to: 'satya01.nerella@gmail.com'
 }
 
}
