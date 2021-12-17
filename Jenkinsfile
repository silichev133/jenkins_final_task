pipeline {
    agent any

    tools {
        maven "Maven_default"
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/silichev133/spring-webapp.git'
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Results') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                archive 'target/*.war'
            }
        }
        stage('Publish') {
            steps {
                nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'jenkuns_maven', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/home/vagrant/task/workspace/final_pipeline/target/webapptest.war']], mavenCoordinate: [artifactId: 'target', groupId: 'org.jenkins-ci.main', packaging: 'war', version: '2.23']]]
            }
        }
        stage('Upload from nexus to tomcat') {
            steps {
                sh '''
                    wget --user=admin --password=197666 http://172.28.128.67:8081/repository/jenkuns_maven/org/jenkins-ci/main/target/2.23/target-2.23.war
                   cp target-2.23.war /opt/tomcat/webapps 
                '''
            }
        }
        stage('Check file') {
            steps {
                sh '''
                    status=$(curl -o /dev/null -s -w "%{http_code}" http://172.28.128.67:8080/target-2.23/)
                    if [[ $status -eq 200 ]]
                    then
                        echo "Good"
                    fi
                '''
            }
        post {  
        failure {  
             mail bcc: '', body: "Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br>", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "Ilya_Silichev@epam.com";  
         }
        }
        }
    }
}
