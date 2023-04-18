node{
    echo"the job name is : ${JOB_NAME}"
echo"the build number   is : ${BUILD_NUMBER}"
echo"the node name is : ${NODE_NAME}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
def mavenHome= tool name: 'maven 3.8.8'
stage('ChekoutCode'){
git credentialsId: 'e4459bea-0e6f-4589-a6b6-da5c350345f4', url: 'https://github.com/kayamvasu/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"

}
stage('ExecutingSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}
stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('DeployAppInTomcatSErver'){
sshagent(['0b1b9b54-ff9e-4bbf-b645-e8d5eecc4eef']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.42.54:/opt/apache-tomcat-9.0.73/webapps"
}
}
}


