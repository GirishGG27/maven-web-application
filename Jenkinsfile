node{
    
def mavenHome = tool name: 'maven3.9.2'

echo "Job Name is: $JOB_NAME"
echo "Node Name is: $NODE_NAME"
echo "Jenkins Home Path is: $JENKINS_HOME"
echo "Build Number is: $BUILD_NUMBER"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckOutCode'){
git branch: 'development', url: 'https://github.com/GirishGG27/maven-web-application.git'
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

stage('DeployAppIntoTomcatServer'){
sshagent(['c0db8500-b6d0-44a0-a317-46949666fdee']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.40.250:/opt/apache-tomcat-9.0.75/webapps/"
}
}

}
