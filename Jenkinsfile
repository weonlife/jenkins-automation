pipeline {
    agent any
    tools{
        maven "maven_3.9.1"
    }
    stages{
        stage("Build with maven"){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/weonlife/jenkins-automation.git']])
                sh "mvn clean install"
            }
        }
        stage("Build docker image"){
            steps{
                script{
                    sh "docker build -t weonlife/devops-integration:latest ."
                }
            }
        }
        stage("Push to Hub"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-cred', variable: 'dockerpwd')]) {
                    sh 'docker login -u weonlife -p ${dockerpwd}'
}
                    sh "docker push weonlife/devops-integration:latest"
                }
            }
        }
    }
}
