pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockertag()
    }
    stages{
        stage ('build Docker Image'){
           steps{
               sh "docker build -t rizwan/nodeapp: ${DOCKER_TAG}"
           }
    } 
       } 

}

def getDockertag() {

    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}