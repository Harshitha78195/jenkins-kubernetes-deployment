pipeline {

  environment {
    dockerimagename = "image/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Harshitha78195/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install' // Install necessary node packages for your React app
      }
    }

    stage('Build React App') {
      steps {
        sh 'npm run build' // Build the React application
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhub-credentials'
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        script {
          sh 'kubectl apply -f deployment.yaml' // Apply the deployment configuration
          sh 'kubectl apply -f service.yaml'    // Apply the service configuration
        }
      }
    }

  }

}
