pipeline {
    agent any
    environment {
        EC2_USER = 'ubuntu'        
        EC2_HOST = '51.21.180.170'  
        SSH_CREDENTIALS_ID = 'UBUNTU (SERVER)'  
        JAR_FILE = 'java -jar app-20250210-094221.jar'  
    }
    stages {
        stage('Build JAR') {
            steps {
                script {
                    sh '.mvnw clean package'
                }
            }
        }
        stage('Deploy JAR to EC2') {
            steps {
                script {
                    sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                        sh """
                        scp -o StrictHostKeyChecking=no target/${JAR_FILE} ${EC2_USER}@${EC2_HOST}:/home/${EC2_USER}/
                        """
                    }
                }
            }
        }
        stage('Run JAR on EC2') {
            steps {
                script {
                    sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} "nohup java -jar /home/${EC2_USER}/${JAR_FILE} > app.log 2>&1 &"
                        """
                    }
                }
            }
        }
    }

