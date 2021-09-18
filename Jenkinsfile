pipeline{  
  environment {
    registry = "daus2936/nodejsapp"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
    stages {
        stage('clone') {
            steps {
                git 'https://github.com/daus2936/nodejshello.git'
            }
        }
        stage('Building image') {
            steps{
                 script {
                     dockerImage = docker.build registry + ":$BUILD_NUMBER"
                  }
                }
             }
          stage('Push Image') {
              steps{
                  script {
                       docker.withRegistry( '', registryCredential){                            
                       dockerImage.push()
                      }
                   }
                } 
          }
          stage('deploy on web server'){
              steps{
                  sshagent(['Web-Server']) {
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@192.168.100.124 "docker run -d -p 3000:3000 --name docker-$BUILD_NUMBER daus2936/nodejsapp:$BUILD_NUMBER"'

                  }
            }    
        }
    }
}

