pipeline {
  agent any 
  
  environment{
      registryName = "acrkubernates"
      registryUrl = "acrkubernates.azurecr.io"
      registryCredentials = "acr"
      dockerImage = "acrkubernates"
  }
  stages {
      
    stage('checkout') {
        steps{
            git credentialsId: 'github', url: 'https://github.com/nourhr/aks-docker.git'
            
      }
    }
  
  stage('Build Docker image') {
        steps{
            script {
            dockerImage = docker.build registryName + ":$BUILD_NUMBER"
            }
      
        }
        }
    stage('Uploade image to ACR') {
        steps{
            script {
            docker.withRegistry("http://${registryUrl}", registryCredentials ){
                dockerImage.push()
            }
            }
      
    }
    }
     stage ('download and connect to AKS Cluster') {
        steps {
            sh 'sudo -S az login -u cp-tic-cs@esprit.tn -p &&tunis&&tunis&&'
            sh 'sudo -S az aks install-cli --install-location=./kubectl'
            sh 'sudo -S az aks get-credentials --resource-group prod-rg --name terraform-aks'
            echo 'connected'
        }
    }
    stage ('deploy image to AKS'){
        steps{
            sh 'sudo -S kubectl apply -f deployment.azure.yaml '
        }
    }
    
  
  
}}
