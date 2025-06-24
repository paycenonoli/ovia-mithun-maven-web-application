node{
def mavenHome = tool name: "Maven"
    
echo "Node name is: ${env.NODE_NAME}"
echo "Job name is:  ${env.JOB_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5'))])
    
//Checkout stage
stage('Code checkout'){
git branch: 'ovia-dev', credentialsId: '6366fa87-5931-4d1d-b6c4-dc1ad3581978', url: 'https://github.com/paycenonoli/ovia-mithun-maven-web-application.git'
}

//Build stage
stage('Maven build'){
sh "$mavenHome/bin/mvn clean package"
}
//Generate SonarQube Report
stage('SonarQube analysis'){
sh "$mavenHome/bin/mvn sonar:sonar"
}
//Upload artifact to artifactory/repository
stage('Upload to Nexus'){
sh "$mavenHome/bin/mvn deploy"
}

//Deploy artifact to Tomcat server
stage('DeployToTomcatServer'){
sshagent(['883f69d8-eb13-463e-9b5e-52fe6a787b5a']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@13.220.157.143:/opt/tomcat/webapps"
}
}
}//Node closing
