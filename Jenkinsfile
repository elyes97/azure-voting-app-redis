pipeline {
    
    environment {
    registry = "elyes97/rfc"
    registryCredential = 'dockerhub'
  }
    
    
    agent any

    stages {
          stage('SCM') {
              steps{
        checkout scm
    }
          }
      
      stage('Building image') {
    steps{
      script {
        docker.build registry + ":$BUILD_NUMBER"
      }
    }
  }
      
        stage('Chek env') {
            steps {
              
    sh 'az login '
    sh ' az aks get-credentials --name test-aks --resource-group Elyes-Othmani-PFE01 '
    
    

            }
        }

    }
}
