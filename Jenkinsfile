node{
    echo "the job name is :${env.JOB_NAME}" 
    echo "the build number is :${env.BUILD_NUMBER}" 
    echo "the node name is :${env.NODE_NAME}" 
    echo "the jenkins home directory is :${env.JENKINS_HOME}" 
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
def mavenHome = tool name: 'maven3.8.7'    
stage('CheckOutCode'){
git branch: 'development', credentialsId: 'bdce5357-7493-4170-a2bd-f6a8d9fedea0', url: 'https://github.com/pavanmojo/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
/*
stage('SonarQubeReport')
{
    withSonarQubeEnv('sonarqubeserver'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"

}
  }
stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}
    stage('DeployAppIntoTomcatServer'){
       sshagent(['ff8d4da6-b952-4f32-a45f-7e35989a93a1']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.43.140:/opt/apache-tomcat-9.0.71/webapps/"
} 
      }
      */
}
