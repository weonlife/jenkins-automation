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
        stage("approvalGate"){
            steps{
                script{
                    mail bcc: '', body: 'Docker image has been build, need approval to push to hub', cc: '', from: '', replyTo: '', subject: 'Approve to push to hub', to: 'mail.obareki@gmail.com'
                    timeout(time:1 , unit:"DAYS"){
                        input message: "Approve to push to dockerHub"
                    }
                }
            }
        }
        stage("Pushing to dockerHub"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-cred', variable: 'dockerpwd')]) {
                    sh 'docker login -u weonlife -p ${dockerpwd}'
}
                    sh 'docker push weonlife/devops-integration:latest'
                }
            }
        }
    }
}
