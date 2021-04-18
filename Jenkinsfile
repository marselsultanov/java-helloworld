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

    stage ('Triggering job') {
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

    stage ('Packaging') {
      steps {
        sh 'tar -xzvf msultanov_dsl_script.tar.gz jobs.groovy'
        sh 'tar -czvf pipeline-msultanov-${BUILD_NUMBER}.tar.gz jobs.groovy Jenkinsfile -C target java-helloworld-1.0.jar'
        archiveArtifacts (
          artifacts: "pipeline-msultanov-${BUILD_NUMBER}.tar.gz"
        )
      }
    }
  }
}
