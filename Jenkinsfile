pipeline {
  environment {
    dockerimagename = "philiphabib/react-app"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
		checkout scmGit(branches: [[name: 'main']], 
                                userRemoteConfigs: [[url: 'https://github.com/philiphabib/jenkins-kubernetes-deployment.git']])
      }
    }
	stage('Build docker image'){
        steps{
            script{
                sh 'docker build -t philiphabib/jenkins-kubernetes-deployment .'
                }
            }
        }
    stage('Push image to Hub'){
        steps{
            script{
                withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                sh 'docker login -u philiphabib -p ${dockerhubpwd}'
                }
                sh 'docker push philiphabib/jenkins-kubernetes-deployment'
                }
            }
        }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", 
                                         "service.yaml")
        }
      }
    }
  }
}