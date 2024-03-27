def frontendImage="<nazwa_repozytorium>/frontend"
def backendImage="<nazwa_repozytorium>/backend"
def dockerRegistry=""
def registryCredentials="dockerhub"
def backendDockerTag = ""
def frontendDockerTag = ""


pipeline {
    agent {
        label 'agent'
    }
    stages{
        stage('Get Code'){
            steps{
                checkout scm
            }
        }
        stage('Adjust version'){
            steps {
                script {
                    backendDockerTag = params.backendDockerTag.isEmpty() ? "latest" : params.backendDockerTag
                    frontendDockerTag = params.frontendDockerTag.isEmpty() ? "latest" : params.frontendDockerTag
                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}" 
                }
            }
        }
        stage('Clear running containers'){
            steps{
                sh "docker rm -f frontend backend"
            }
        }
        stage('Deploy application') {
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                       docker.withRegistry("$dockerRegistry", "$registryCredentials") {
                            sh "docker-compose up -d"
                        }
                    }
                }
            }
        }    
    parameters {
        string 'backendDockerTag'
        string 'frontendDockerTag'
    }
    post{
        always{
            sh "docker-compose down"
            cleanWS()
        }
    }

}