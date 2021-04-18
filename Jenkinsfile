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
  }
}
