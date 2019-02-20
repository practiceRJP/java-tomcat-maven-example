pipeline {
    agent any
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                echo "PATH = ${PATH}"
                '''
                echo "Initializing the code file"
            }
        }

        stage ('Build') {
            steps {
                echo 'Hello World'
            }
        }

        stage ('Deploy') {
            steps {
                echo 'Deployed an Artifact'
            }
        }
    }
}