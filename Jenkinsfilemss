node
{
    def mavenhome = tool name : "maven 3.6.3"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    stage('code checkout')
    {
        git branch: 'development', credentialsId: '9d9307a7-db79-4c9a-a5cb-8873f68daca8', url: 'https://github.com/Muni-Prathap/maven-web-application.git'
    }
    
    stage('Build')
    {
        sh "${mavenhome}/bin/mvn clean package"
    }
    
    stage('execute Sonar Report')
    {
        sh "${mavenhome}/bin/mvn sonar:sonar"
    }
    
    stage('upload Artifact in Nexus')
    {
        sh "${mavenhome}/bin/mvn deploy"
    }
    
    stage('deploye to Tomcat')
    {
        sshagent(['e182c041-d635-4a41-bd54-8d16624eacfa']) {
    
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.17.189://opt/apache-tomcat-9.0.37/webapps"
    
       }
    }
    
    stage('email Notification')
    {
        emailext body: '''Build Is Over

    Regards
    muni.''', subject: 'Build Is Over', to: 'munipratapmech@gmail.com'
    }
}
