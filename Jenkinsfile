pipeline {
    
    environment {
    registry = "elyes97/rfc"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
    
    
    agent any

    stages {
   
       stage('Cloning Git') 
         {
           steps {
                 git 'https://github.com/elyes97/azure-voting-app-redis.git'
                 }
         }
      
      stage('Building image') 
        {
            steps{
                 script {
                    dockerImage = docker.build registry + ":TestV$BUILD_NUMBER"
  
                 }
        }
      }
        
              stage('Pushing  image') 
        {
            steps{
                 script {
                    docker.withRegistry( '', registryCredential ) 
                     {
            dockerImage.push()
                     }
  
                 }
       }
      }
      
      stage('Chek env') 
        {
            steps 
              {
               sh 'az login '
               sh ' az aks get-credentials --name test-aks --resource-group Elyes-Othmani-PFE01 '
                  sh ' ls var/lib/jenkins/workspace/test-azure-vote-pipeline/ '
                
              }
        }

        }
}
