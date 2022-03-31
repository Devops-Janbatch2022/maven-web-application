node{

buildName 'Dev - ${BUILD_NUMBER}' 
buildDescription 'Pipeline Script - Scriptedway'
 def mavenHome = tool name: "maven3.8.4"

echo "The Node name is:  ${env.NODE_NAME} "
echo "The Job name is:  ${env.JOB_NAME} "
echo " The Build number is: ${env.BUILD_NUMBER}  "

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

//Checkout Stage
stage('CheckoutCode'){
  git branch: 'development', credentialsId: 'd49b6494-caa6-4753-93ba-b0cf4464971a', url: 'https://github.com/Devops-Janbatch2022/maven-web-application.git'
}

//Build Stage
stage('Build'){
sh "$mavenHome/bin/mvn clean package"
}

//Generate SonarQube Report
stage('SonarQube'){
sh "$mavenHome/bin/mvn sonar:sonar"
}

//Upload Artifact into Artifactory repo
stage('UploadArtifactIntoNexus'){
sh "$mavenHome/bin/mvn deploy"
}

//Deploy App into Tomcat Server
stage('DeployAppIntoTomcat'){
sshagent(['a2f21f3d-e023-4eb7-a92b-1a7c6c032468']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@43.204.22.153:/opt/apache-tomcat-9.0.59/webapps"
}
}
}//Node Closing
