pipeline {
   environment {
    registry = "lalitha13/heartihealth"
    registryCredential = 'docker_hub_lalitha'
    dockerImage = ''
  }
  agent any
  stages{
    stage ('Build Install dependencies') {
      steps{
        echo "Modules installed"
        powershell 'npm install'
      }
    }
    stage ('Build completed') {
      steps{
        echo "Build completed"
        powershell 'npm run ng -- build --prod'
      }
    }
    stage ('Build Docker Image') {
      steps{
        echo "Building Docker Image"
          script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage ('Push Docker Image') {
      steps{
        echo "Pushing Docker Image"
         script {
              docker.withRegistry( '', registryCredential ) {
              dockerImage.push()
              dockerImage.push('latest')
          }
        }
      }
    }
    stage ('Deploy to Dev') {
      steps{
        echo "Deploying to Dev Environment"
         sh "docker rm -f heartihealth || true"
        sh "docker run -d --name=heartihealth -p 8081:8080 lalitha13/heartihealth"
      }
    }
  }
}
