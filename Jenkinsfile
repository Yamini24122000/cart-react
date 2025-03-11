pipeline {
    agent any

    stages {
        stage('Run JAR file') {
            steps {
                withEnv(['JAR_FILE=app-20250210-094221.jar']) 
                {
                sh 'java -jar app-20250210-094221.jar'
            }
        }
    }
}
