node {
	try {
		stage('Build Servlet') { 
            checkout scm
            sh 'mvn clean package' }
	} catch (e) {
		echo 'This will run only if failed'

		throw e
	} finally {
		def currentResult = currentBuild.result ?: 'SUCCESS'
		if (currentResult == 'UNSTABLE') {
			echo 'This will run only if the run was marked as unstable'
		}

		if (currentResult == 'SUCCESS') {
			echo 'Now archiving...'
			archiveArtifacts artifacts: '**/*.war'
		}
	}

	try {
		stage('Deploy Build in Staging Area') { build job : 'PipelineAsCode - Deploy_Artifact_to_Staging_Area' }
	} catch (e) {

		echo 'Fail on Staging'
		throw e
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
			echo 'Fail on Production'
		}

		if (currentResult == 'SUCCESS') {
			echo 'Deploy on PRODUCTION is Successful'
		}
	}
}