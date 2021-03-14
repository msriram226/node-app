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
    
    }         
}
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

