node{
    echo "the job name is :${env.JOB_NAME}" 
    echo "the build number is :${env.BUILD_NUMBER}" 
    echo "the node name is :${env.NODE_NAME}" 
    echo "the jenkins home directory is :${env.JENKINS_HOME}" 
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
def mavenHome = tool name: 'maven3.8.7'  
    try{
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
catch(e){
currentBuild.result="FAILURE"
}
finally{
sendslacknotifications(currentBuild.result)
}
}//node closing

def sendslacknotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}

