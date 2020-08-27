node
{
    def mavenhome = tool name : "maven-3.6.3"
    
    echo "Github branch name ${env.BRANCH_NAME}"
    echo "jenkins Home dir ${env.JENKINS_HOME}"
    echo "jenkins Job no ${env.BUILD_NUMBER}"
     echo "Job name ${env.JOB_NAME}"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
   stage('code checkout')
    {
        git branch: 'development', credentialsId: 'c356fd57-dd8e-4076-a14c-c047c0e6624e', url: 'https://github.com/Muni-Prathap/maven-web-application.git'
    }
    
    stage('build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('DeploytoTomcat')
    {
        deploy adapters: [tomcat9(credentialsId: '760ff8d3-5bd6-42ed-9736-b151969a0fc4', path: '', url: 'http://13.235.31.5:9980/')], contextPath: null, war: '**/*.war'
    }

  /*  
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
    */
}
