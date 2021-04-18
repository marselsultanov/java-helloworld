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
         }
         Second: {
            stage('Second') {
               sh 'mvn test'
            }
         }
      )
   }
}
