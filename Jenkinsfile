node {

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '2', daysToKeepStr: '365', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])

echo "Build Nubmer is: ${env.BUILD_NUMBER}"
echo "Job Name is: ${env.JOB_NAME}"
echo "Jenkins Home Dir is: ${env.JENKINS_HOME}"
echo "Build URL is: ${env.BUILD_URL}"

def mavenHome = tool name: 'Maven-3.9.6'

stage('CheckoutCode'){
git branch: 'development', credentialsId: '97e68fd4-9508-4de8-96ea-2970d850ee2d', url: 'https://github.com/learningdevops06/maven-web-application.git'	
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat'){
sshagent(['c4d62bfb-2898-44aa-8d7d-5a6f78453c79']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war root@192.168.6.11:/opt/tomcat9/webapps"
}
}

} 
