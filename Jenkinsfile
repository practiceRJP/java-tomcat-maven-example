pipeline {
    agent any
    stages {

        stage ('Build Servlet') {

            steps {
                sh 'mvn clean package'
            }

            post {
                success {
                    echo 'Now archiving...'

                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage ('Deploy Build in Staging Area') {

            steps {

                build job : 'PipelineAsCode - Deploy_Artifact_to_Staging_Area'
                
            }
        }
    }
}