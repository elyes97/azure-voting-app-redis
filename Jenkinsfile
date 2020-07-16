pipeline {
    
     //def imageName = "elyes97/rfc:${env.BUILD_NUMBER}"
 /*    def servicePrincipalId = '<your-service-principal>'
    def resourceGroup = 'Elyes-Othmani-PFE01'
    def aks = 'test-aks'*/

   
    environment {
    registry = "elyes97/rfc"
    registryCredential = 'dockerhub'
    dockerImage = ''
        
        imageName = "elyes97/rfc:${env.BUILD_NUMBER}"
    
        servicePrincipalId = 'ServicePrincipalIdCredential'
         resourceGroup = 'Elyes-Othmani-PFE01'
        sshCredential = 'Ssh'
         aks = 'test-aks'
        
       IMAGE_TAG = "${imageName}"
        
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
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                     IMAGE_TAG = "TestV$BUILD_NUMBER"
                     env.TEST_VARIABLE = docker.build registry + ":TestV$BUILD_NUMBER"
                      echo "TEST_VARIABLE = TestV$BUILD_NUMBER"
                     
                  echo "FOO = ${env.IMAGE_TAG}"
                     sh ' pwd '
                     sh 'cd /var/lib/jenkins/workspace/'
                     sh'ls'
                    /* env.TEST_VARIABLE = docker.build registry + ":TestV$BUILD_NUMBER"
                      echo "TEST_VARIABLE = TestV$BUILD_NUMBER"
                      
*/
                 }
        }
      }
        
            stage('Pushing  image') 
        {
             
            steps{
                echo "TEST_VARIABLE = TestV$BUILD_NUMBER"
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
                  script
                  {
                      def imagetag = "registry + :TestV$BUILD_NUMBER"
               sh 'az login '
               sh ' az aks get-credentials --name test-aks --resource-group Elyes-Othmani-PFE01 '
                  sh'echo $dockerImage'
                  sh ' pwd '
                   sh'ls /var/lib/jenkins/workspace/test-azure-vote-pipeline'
                   sh'cat /var/lib/jenkins/workspace/test-azure-vote-pipeline/azure-vote-all-in-one-redis.yaml'
                  sh'kubectl get all'
                  

             //     sh'kubectl apply -f /var/lib/jenkins/workspace/test-azure-vote-pipeline/azure-vote-all-in-one-redis.yaml --validate=false'
               //       sh'kubectl set image deployment azure-vote-front azure-vote-front=elyes97/rfc:TestV$BUILD_NUMBER'
                //  sh'kubectl get all'
                  
                  
                  }
                
              }
        }
        stage('Deploy') {
            steps{
        // Apply the deployments to AKS.
        // With enableConfigSubstitution set to true, the variables ${TARGET_ROLE}, ${IMAGE_TAG}, ${KUBERNETES_SECRET_NAME}
        // will be replaced with environment variable values
        acsDeploy azureCredentialsId: servicePrincipalId,
                  resourceGroupName: resourceGroup,
                  containerService: "${aks} | AKS",
                  configFilePaths: './azure-vote-all-in-one-redis.yaml',
                  enableConfigSubstitution: true,
                  sshCredentialsId:sshCredential,
                  containerRegistryCredentials: [[credentialsId: registryCredential, url: "https://hub.docker.com/"]]
            }
    }

        }
}
