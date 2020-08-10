pipeline {
   environment {
    registry = "lalitha13/petclinic"
    registryCredential = 'docker_hub_lalitha'
    dockerImage = ''
  }
  agent any
  stages{
     stage('Git CheckOut'){
       steps{
         git 'https://github.com/La-litha/HeartiHealthProject.git'
       }
     }
    stage ('Install dependencies') {
      steps{
        echo "Modules installing"
        sh 'npm install'
      }
    }
    stage ('Build') {
      steps{
        echo "Building Project"
        sh 'npm run ng -- build --prod'
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
         sh "docker rm -f petclinic || true"
        sh "docker run -d --name=petclinic -p 8081:8080 lalitha13/petclinic"
      }
    }
  }
}
