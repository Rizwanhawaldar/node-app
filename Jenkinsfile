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
               sh "sudo docker build . -t rizwanhawaldar/nodeapp:${DOCKER_TAG}"
           }
    } 

         stage("DockerHub push"){

             steps{
                 withCredentials([string(credentialsId: "docker-hub", variable: 'dockerHubPwd')]){
                   sh "sudo docker login -u rizwanhawaldar -p ${dockerHubPwd}"
                   sh "sudo docker push  rizwanhawaldar/nodeapp:${DOCKER_TAG} "
             }
             }
                 
         }
       } 

}

def getDockertag () {
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
