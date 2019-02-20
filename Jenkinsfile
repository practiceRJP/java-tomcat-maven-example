pipeline {
    agent any
    stages {
    
        stage ('Build Servlet') {
            steps {
                sh 'mvn clean package'
            }
        }

            post {
                success {
                    echo 'Now archiving...'
                    
                    archiveArtifacts artifacts : '**/*.war'
                }
        }
    }
}