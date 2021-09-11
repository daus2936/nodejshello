pipeline{  
  environment {
    registry = "daus2936/nodejsapp"
    registryCredential = '0b96ac80-1ff0-4643-a45d-e8af270841ac'
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
                  sshagent(['49e68d2e-8243-4106-b062-a333ab7ba0fb']) {
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@192.168.100.124 "docker run -d -p 3000:3000 --name $BUILD_NUMBER daus2936/nodejsapp:$BUILD_NUMBER"'

                  }
            }    
        }
    }
}
