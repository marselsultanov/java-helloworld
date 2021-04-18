node {
   stage('Checking out') {
      checkout scm
   }
   
   stage('Building code') {
      sh 'mvn compile'
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
         $class: 'CopyArtifact',
         projectName: 'Child1',
         filter: 'msultanov_dsl_script.tar.gz'
      )
   }
}
