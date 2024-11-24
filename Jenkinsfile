pipeline {
  environment {
    dockerImage = ''
    registry = 'philiphabib/react-app'
    registryCredential = 'philiphabib'
  }

  agent {
    kubernetes {
      yamlFile 'deployment.yaml, service.yaml'
    }
  }	
  agent any
  stages {
    stage('Checkout Source'){
      steps {
		checkout scmGit(branches: [[name: 'main']], 
                                userRemoteConfigs: [[url: 'https://github.com/philiphabib/jenkins-kubernetes-deployment.git']])
      }
    }
	stage('Build docker image'){
        steps{
            script{
                dockerImage = docker.build registry
                }
            }
        }
    stage('Push image to Hub'){
        steps{
                script{
                    docker.withRegistry('', registryCredential){
                        dockerImage.push()
                    }
                    
                }
            }
			}
    stage('Deploying React.js container to Kubernetes') {
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
            sh 'kubectl apply -f deployment.yaml',
            sh 'kubectl apply -f service.yaml'		    
        }
      }
    }
  }
}
