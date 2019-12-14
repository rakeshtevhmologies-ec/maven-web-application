node
{
    
    echo "GitHub BranhName ${env.BRANCH_NAME}"
    echo "Jenkins Job Number ${env.BUILD_NUMBER}"
    echo "Jenkins Node Name ${env.NODE_NAME}"
  
    echo "Jenkins Home ${env.JENKINS_HOME}"
    echo "Jenkins URL ${env.JENKINS_URL}"
    echo "JOB Name ${env.JOB_NAME}"
  
  properties([
      buildDiscarder(logRotator(numToKeepStr: '3')),
      pipelineTriggers([
          pollSCM('* * * * *')
      ])
    ])
  
  def mavenHome = tool name: "maven3.6.2"
  
 stage('CodeCheckout')
 {
 git branch: 'development', credentialsId: 'ff096ede-9887-4140-8953-a580da1312a7', url: 'https://github.com/MithunTechnologiesDevOps/maven-web-application.git'
 
 }

 stage('Build')
 {
  sh "${mavenHome}/bin/mvn package"
  
 }
 
 stage('SonarQubeReport')
 {
  sh "${mavenHome}/bin/mvn sonar:sonar"
  
 }
 
 stage('UploadArtifactintoNexus')
 {
  sh "${mavenHome}/bin/mvn deploy"
  
 }
 
stage('DeployAppIntoTomcat')
 {
 sshagent(['f0ef3182-69ef-4169-8368-9ea18a5fe3af']) {
  
sh  "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.159.205:/opt/apache-tomcat-9.0.29/webapps/"
     
 }
 
 }
 
 stage('EmailNotification')
 {
 emailext body: '''
 Build is over!!.

 Regards,
 Mithun Technologies,
 9980923226.''', subject: 'Buid is over...', to: 'devopstrainingblr@gmail.com'
 
 }

}
