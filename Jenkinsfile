node('node-1')
{
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])
    
def mavenHome = tool name: "maven3.6.3"
stage('CheckoutCode')
{
 git branch: 'development', credentialsId: '80b6db14-8c75-4614-b429-ebbe1cee4f31', url: 'https://github.com/prasadsoftwaresolutions/maven-web-application.git'
}

stage('Build')
{
 sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
 sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
 sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatServer')
{
 sshagent(['f0b2bd04-a968-498a-b5dd-c8a914651f20']) 
{
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.211.35.239:/opt/tomcat9/webapps/"
}
}
stage('SendEmailNotification')
{
 mail bcc: 'prasadg538@gmail.com', body: '''Build over...

Regards,
Prasad Technologies,
9505029933.''', cc: 'prasadg538@gmail.com', from: '', replyTo: '', subject: 'Build Over!!', to: 'prasadg538@gmail.com'
}
}
