pipeline{
    agent any
    tools {
        maven 'maven-3.9.2'
    }
    stages{
        stage('Code_Pull'){
            steps{
                git branch: 'master', url: 'https://github.com/GatlaSowmya/Task_1.git'
            }
        }
        stage('Code_Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Code_Quality_Analysis'){
            steps{
                sh 'mvn sonar:sonar'
            }
        }
        stage('Image_Build'){
            steps{
                sh 'docker build -t sowmya018/tomcat:$BUILD_ID .'
            }
        }
        stage('Image_Push'){
            steps{
            withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
           sh 'docker push sowmya018/tomcat:$BUILD_ID'
             }
          }
        }
        stage('Deploy'){
            steps{
                sshagent(['staging_server']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@35.180.32.241 << EOF
                    sudo docker pull sowmya018/tomcat:$BUILD_ID
                    sudo docker run -d --name webapp$BUILD_ID -p ${i}:8080 sowmya018/tomcat:$BUILD_ID
                    '''
             }
            }
        }
        
    }
}
