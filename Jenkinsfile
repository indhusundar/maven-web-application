node{
    
echo "The job name is : ${env.JOB_NAME}" 
echo "The job name is : ${env.BUILD_NUMBER}" 
echo "The job name is : ${env.JOB_URL}"    
    
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name : 'maven3.8.6'  

stage('checkOutCode'){
git branch: 'development', credentialsId: 'e35e668b-92eb-4d77-94ab-29c301a2dbf6', url: 'https://github.com/indhusundar/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecutionSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactintoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployApplicationIntoTomcatServer'){
sshagent(['eeed690c-f8fe-462a-9755-6920a8c99d50']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.21.247:/opt/apache-tomcat-9.0.70/webapps/"
}
}

}
