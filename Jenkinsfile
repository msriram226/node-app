pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()     
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t msriram226/nodeapp:${DOCKER_TAG}"
            }
        }    
    stage(' Docker push Image'){
            steps{
                withCredentials([string(credentialsId: 'Docker_Hub', variable: 'DockerHubPasswd')]) {
                sh "docker login -u msriram226 -p ${DockerHubPasswd}"
                    sh "docker push msriram226/nodeapp:${DOCKER_TAG}"    
                }
            } 
        }
                
    stage('Deploy on Kubernetes'){
            steps{
                sh "chmod +x changeTag.sh"
                sh "./changeTag.sh ${DOCKER_TAG}"
                sshagent(['kops-machine']) {
                sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml msriram208@10.128.15.210:/home/"
                script{
                    try {
                        sh "ssh msriram208@10.128.15.210 kubectl apply -f ."

                    } catch(error){
                        sh "ssh msriram208@10.128.15.210 kubectl create -f ."                   }
                }
            }
        }
    }         
}
}
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
