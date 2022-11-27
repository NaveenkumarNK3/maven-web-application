node{
    def maven = tool name: 'Maven 3.8.6'
    echo "Job name is: ${env.JOB_NAME}"
    echo "BUild Number is: ${env.BUILD_NUMBER}"
    echo "Node Name is: ${env.NODE_NAME}"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    stage('checkout')
    {
    git branch: 'development', credentialsId: 'f75fc46a-bf3a-422b-9bf3-9dc112a2f2c0', url: 'https://github.com/NaveenkumarNK3/maven-web-application.git'
    }
    stage('build')
    {
        sh "${maven}/bin/mvn clean package"
    }
    stage('Executesonarqubereport')
    {
        sh "${maven}/bin/mvn clean sonar:sonar"
    }
    stage('uploadArtifacts')
    {
        sh "${maven}/bin/mvn clean deploy"
    }
    stage('deployApp'){
        sshagent(['1e212ae2-7e4d-4fdc-a682-b90e7cbdee94']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.127.192:/opt/apache-tomcat-9.0.69/webapps"
}
    }
}
