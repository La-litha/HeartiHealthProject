node() {
    environment {
    registry = "lalitha13/heartihealth"
    registryCredential = 'docker_hub_lalitha'
    dockerImage = ''
  }
    stage('Install dependencies') {
        nodejs('nodejs') {
            sh "npm install --save-dev --unsafe-perm node-sass"
            sh "npm start"
            sh "npm stop"
            echo 'Modules installed'
        }
        
    }
    stage('Build') {
        nodejs('nodejs') {
            sh "npm run ng -- build --prod"
            echo 'Build completed'
        }
        
    }
    stage ('Build Docker Image') {
       nodejs('nodejs'){
        echo "Building Docker Image"
          script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          }
        }
    }
    stage ('Push Docker Image') {
       nodejs('nodejs'){
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
        echo "Deploying to Dev Environment"
         sh "docker rm -f heartihealth || true"
         sh "docker run -d --name=heartihealth -p 8081:8080 lalitha13/heartihealth"
      }
    }
