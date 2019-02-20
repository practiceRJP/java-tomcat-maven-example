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

        stage ('Deploy Build in Production Area') {

            steps {

                timeout (time: 5, unit: 'Day') {
                    input message: 'Approve PRODUCTION Deployment?'
                }

                build job : 'PipelineAsCode - Deploy_Artifact_to_Production_Area'
                
            }

            post {
                success {
                    echo 'Deployment on PRODUCTION is Successful'
                }

                failure {
                    echo 'Deployment Failure on PRODUCTION'
                }
            }
            
        }
    }
}