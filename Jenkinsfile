pipeline {
  agent any

  options {
    skipDefaultCheckout true
  }
  
  stages {
    stage ('Checking out') {
      steps {
        checkout scm
      }
    }

    stage ('Building code') {
      steps {
        sh 'mvn package'
      }
    }

    stage ('Testing code') {
      parallel {
        stage ('Test') {
          steps {
            sh 'mvn test'
          }
        }
        stage ('Fake') {
          steps {
            echo 'fake'
          }
        }
      }
    }
    
    stage ('Triggering job and fetching artefact') {
      steps {
        build (
          job: 'Child1',
          parameters: [string(name: 'Branch', value: 'msultanov')]
        )
        copyArtifacts (
          projectName: 'Child1',
          filter: 'msultanov_dsl_script.tar.gz'
        )
      }
    }
  }
}
