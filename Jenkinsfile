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
        
    }
}