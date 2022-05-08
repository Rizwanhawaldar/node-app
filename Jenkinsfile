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
          stage('Deploy to k8s'){
               steps{

                 sh "chmod +x changeTag.sh"
                 sh "./changeTag.sh ${DOCKER_TAG}"
                 sshagent(['master']){

                      sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml ubuntu@34.212.175.62:/home/ubuntu"
                      sh "sudo su"
                      sh "cd /home/ubuntu"
                      script {
                          try{
                            sh "ssh ubuntu@34.212.175.62 kubectl apply -f node-app-pod.yml"
                             sh "ssh ubuntu@34.212.175.62 kubectl apply -f services.yml"  
                              

                          }catch (error){
                                 sh "ssh ubuntu@34.212.175.62 kubectl create -f node-app-pod.yml"
                                 sh "ssh ubuntu@34.212.175.62 kubectl create -f services.yml"  
                     }
                 }
             }
         }
         }
       } 

}

def getDockertag () {
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
