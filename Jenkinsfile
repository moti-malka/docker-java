pipeline {
    agent any
    
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
    stages{
        stage('Clone') {
            steps{
                
                // Clean before build
                cleanWs()
             
                echo 'Clone repo'
            
                sh 'git clone https://github.com/moti-malka/docker-java.git'
                
            }
    
        }
        
        stage('Build') {
            steps{
             
                echo 'Set directory ctx'
                
                dir('docker-java/app') {
                 echo 'Build the docker image'
                 sh 'sudo docker build -t motiio/demo-java:${BUILD_NUMBER} .'
                }
                
            }
    
        }
        stage('Login') {

			steps {
			    echo 'Login to registry'
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
        stage('Push') {

			steps {
			    echo 'Push image motiio/demo-java:${BUILD_NUMBER}'
				sh 'docker push motiio/demo-java:${BUILD_NUMBER}'
			}
		}
		
		
	    stage('Apply Kubernetes files') {
	        steps{
	            dir('docker-java/helm') {
                   withKubeConfig([credentialsId: 'kubeConfig', serverUrl: 'https://aks-pm8-dev-api-6f14079e.hcp.westeurope.azmk8s.io/']) {
                   sh 'helm upgrade --namespace pm8-dev --install --recreate-pods --values ./demo-java/values.yaml --set image.repository=motiio/demo-java --set image.tag=${BUILD_NUMBER}  demo-java ./demo-java'
                }
                }
	        }
	        
	    }
		
   }
}