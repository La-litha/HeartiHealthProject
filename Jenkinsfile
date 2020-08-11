pipeline {
  environment {
      registry = "lalitha13/heartihealth"
    registryCredential = 'docker_hub_lalitha'
    dockerImage = ''
  }
  agent any
  stages{
    stage('Git'){
      steps{
        git "https://github.com/La-litha/HeartiHealthProject.git"
      }
    }
    stage ('Build') {
      steps{
        echo "Building Project"
        nodejs('nodejs') {
         sh 'npm install'
         sh 'npm run build'
       // sh 'npm rebuild node-sass'
         // sh 'ng serve'
        }
      }
    }
  //  stage ('Archive') {
   //   steps{
   //     echo "Archiving Project"
   //     archiveArtifacts artifacts: '*/.jar', followSymlinks: false
   //   }
  //  } 
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
         sh "docker run -d --name=heartihealth -p 4200:4200 lalitha13/heartihealth"
      }
    }
  }
}
