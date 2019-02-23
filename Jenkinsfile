node {
	try {
		stage('Build Servlet') { 
            checkout scm
            sh 'mvn clean package' 
        }
	} catch (e) {
		echo 'Failed to build servlet'

		throw e
	} finally {
		def currentResult = currentBuild.result ?: 'SUCCESS'
		if (currentResult == 'UNSTABLE') {
			echo 'Failed to build servlet'
		}

		if (currentResult == 'SUCCESS') {
			echo 'Success! Now archiving...'
			archiveArtifacts artifacts: '**/*.war'
		}
	}

	try {
		stage('Deploy Build in Staging Area') { build job : 'PipelineAsCode - Deploy_Artifact_to_Staging_Area' }
	} catch (e) {

		echo 'Fail on STAGING'
		throw e
	} finally {
        def currentResult = currentBuild.result ?: 'SUCCESS'
		if (currentResult == 'UNSTABLE') {
			echo 'Fail on STAGING'
		}

		if (currentResult == 'SUCCESS') {
			echo 'Deploy on STAGING is Successful'
		}
    }

	try {
		stage ('Deploy Build in Production Area') {
			timeout (time: 5, unit: 'DAYS') { input message: 'Approve PRODUCTION Deployment?' }
			build job : 'PipelineAsCode - Deploy_Artifact_to_Production_Area'
		}
	} catch (e) {

		echo 'Fail on Production'
		throw e
	} finally {
		def currentResult = currentBuild.result ?: 'SUCCESS'
		if (currentResult == 'UNSTABLE') {
			echo 'Fail on PRODUCTION'
		}

		if (currentResult == 'SUCCESS') {
			echo 'Deploy on PRODUCTION is Successful'
		}
	}
}