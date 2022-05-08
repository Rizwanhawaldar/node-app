pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockertag ()
    }
    stages{
        stage ('build Docker Image'){
           steps{
               sh "sudo su"
               sh "cd /home/ubuntu"
               sh "sudo docker build . -t rizwan/nodeapp:${DOCKER_TAG}"
           }
    } 

         stage("DockerHub push"){


             withCredentials([string(credetialsId: "docker-hub", variable: 'dockerHubPwd')]){
                 sh "docker login -u rizwanhawaldar -p ${dockerHubPwd}"
                 sh "docker push  rizwan/nodeapp:${DOCKER_TAG} "
             }
         }
       } 

}

def getDockertag () {
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
