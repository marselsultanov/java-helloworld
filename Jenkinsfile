node {
	stage('Checking out') {
		checkout scm
	}
   
	stage('Building code') {
		sh 'mvn package'
	}
   
	stage('Testing code') {
		parallel (
			First: {
				stage('First') {
					sh 'mvn test'
				}
			},
			Second: {
				stage('Second') {
					echo 'fake test'
				}
			}
		)
	}

	stage('Triggering job and fetching artefact') {
		build job: 'Child1', parameters: [string(name: 'Branch', value: 'msultanov')]
		step (
			CopyArtifact projectName: 'Child1',
			filter: 'msultanov_dsl_script.tar.gz'
		)
	}
	
	stage('Packaging') {
		sh 'tar -xzvf msultanov_dsl_script.tar.gz jobs.groovy'
		sh 'tar -czvf pipeline-msultanov-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile -C target java-helloworld-1.0.jar'
		step (
			archiveArtifacts artifacts: 'pipeline-akarzhou-${BUILD_NUMBER}.tar.gz'
		)
	}
}
