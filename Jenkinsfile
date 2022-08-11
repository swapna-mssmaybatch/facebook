node
{
  
def mavenHome = tool name: "maven3.8.5"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "the job name is: ${env.JOB_NAME}"
echo "the node name is: ${env.NODE_NAME}"
echo "the workspace path is: ${env.WORKSPACE}"
echo "the node label is: ${env.NODE_LABLES}"
echo "the build number is: ${env.BUILD_NUMBER}"

stage('CheckoutCode'){
git branch: 'development', credentialsId: '540637fe-1744-4d4e-8809-e5520a908dad', url: 'https://github.com/swapna-mssmaybatch/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExcuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"   or (mvn sonar:sonar)"
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatserver'){
sshagent(['e0a6baab-32c1-43a4-a4d3-74e420749672']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.93.166:/opt/apache-tomcat-9.0.64/webapps/"
}
}

}
