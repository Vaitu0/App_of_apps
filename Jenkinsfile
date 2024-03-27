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
    }
    parameters {
        string 'backendDockerTag'
        string 'frontendDockerTag'
    }

}