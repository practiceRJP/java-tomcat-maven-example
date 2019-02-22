node {


	try {
		stage('Test') { sh 'echo "Fail!"; exit 1' }
		echo 'This will run only if successful'
	} catch (e) {
		echo 'This will run only if failed'

		// Since we're catching the exception in order to report on it,
		// we need to re-throw it, to ensure that the build is marked as failed
		throw e
	} finally {
		def currentResult = currentBuild.result ?: 'SUCCESS'
		if (currentResult == 'UNSTABLE') {
			echo 'This will run only if the run was marked as unstable'
		}

		def previousResult = currentBuild.previousBuild?.result
		if (previousResult != null && previousResult != currentResult) {
			echo 'This will run only if the state of the Pipeline has changed'
			echo 'For example, if the Pipeline was previously failing but is now successful'
		}

		echo 'This will always run'
	}


	/*
	 stage ('Build Servlet') {
	 sh 'mvn clean package'
	 post {
	 success {
	 echo 'Now archiving...'
	 archiveArtifacts artifacts: '**\/*.war'
	 }
	 }
	 }
	 stage ('Deploy Build in Staging Area') {
	 build job : 'PipelineAsCode - Deploy_Artifact_to_Staging_Area'
	 }
	 stage ('Deploy Build in Production Area') {
	 timeout (time: 5, unit: 'DAYS') {
	 input message: 'Approve PRODUCTION Deployment?'
	 }
	 build job : 'PipelineAsCode - Deploy_Artifact_to_Production_Area'
	 post {
	 success {
	 echo 'Deployment on PRODUCTION is Successful'
	 }
	 failure {
	 echo 'Deployment Failure on PRODUCTION'
	 }
	 }
	 }
	 stage ('Example') {
	 if (env.BRANCH_NAME == 'master') {
	 echo 'I only execute on the master branch'
	 } else {
	 echo 'I execute elsewhere'
	 }
	 }
	 */
}