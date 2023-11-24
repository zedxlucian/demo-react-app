pipeline {

  environment {
    dockerimagename = "slydien/demo-react-app"
    dockerImage = ""
  }

  agent any

  stages {

//    stage('Checkout Source') {
//      steps {
//        git 'https://github.com/zedxlucian/demo-react-app.git'
//      }
//    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build(dockerimagename, "./app") 
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-slydien'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
