pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package'  
                archiveArtifacts artifacts: '**/*.jar', allowEmptyArchive: true
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def ec2User = "ubuntu"
                    def ec2Host = "16.171.237.202"
                    def sshKeyPath = "~/.ssh/pipeline.pem"
                    def jarFile = "target/app-20250210-094221.jar"
                    def remotePath = "/home/ubuntu/app-20250210-094221.jar"

                    sh """
                        scp -i ${sshKeyPath} ${jarFile} ${ec2User}@${ec2Host}:${remotePath}
                    """

                    sh """
                        ssh -i ${sshKeyPath} ${ec2User}@${ec2Host} 'nohup java -jar ${remotePath} > /dev/null 2>&1 &'
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    def ec2Host = "16.171.237.202"
                    def appPort = "8080"
                    
                    sh """
                        curl http://${ec2Host}:${appPort}
                    """
                }
            }
        }
    }
}

