pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'yasindu-git', url: 'https://github.com/yasindugunasekara/GitHub-Docker-and-Jenkins-CI-CD-Pipeline.git']])

                    git branch: 'main', url: 'https://github.com/yasindugunasekara/GitHub-Docker-and-Jenkins-CI-CD-Pipeline.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t yasindugunasekara/github-pipline:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'yasindu-dockerhub-pass', variable: 'yasindu-dockerhub-pass')]) {

                    script {
                        bat "docker login -u yasindugunasekara -p %yasindu-dockerhub-pass%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push yasindugunasekara/github-pipline:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
