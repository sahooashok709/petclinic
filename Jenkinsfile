pipeline {
agent any
     stages{
        
        stage('Git_checkout'){
            
            steps {
                git branch: 'master', url: 'https://github.com/sahooashok709/petclinic.git'
            }
        }
        
        stage('Maven_Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('SonarAnalysis') {
            steps {
                withSonarQubeENV('sonarqube_mine') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage("deploy_in_tomcat"){
            steps{
                sshagent(['jenkins_declarative']) {
                sh """
                    ssh -o StrictHostKeyChecking=no C:\JENKINS_HOME\workspace\jenkins_declarative_pipeline\web\target\myweb.war  ec2-user@172.31.45.158:/opt/tomcat8/webapps/
                    ssh ec2-user@172.31.45.158 /opt/tomcat8/bin/shutdown.sh
                    ssh ec2-user@172.31.45.158 /opt/tomcat8/bin/startup.sh
                   """ 
                }
        }
    } 
}
