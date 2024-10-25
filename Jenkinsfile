pipeline {
    agent any
    stages {
        stage('Code') {
            steps {
                git url: 'https://github.com/rksdutt/node-todo-cicd.git', branch: "master"
            }
        }
        
        stage('Build') {
            steps {
                sh "docker build . -t node-app-test-new"
            }
        }
        
        stage('Test') {
            steps {
                echo 'Hello World'
            }
        }
        
        stage('Push To Repo') {
            steps {
                withCredentials([usernamePassword(credentialsId:"docker-cred",passwordVariable:"dockerPass",usernameVariable:"dockerUser")]){
                    sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                    sh "docker tag node-app-test-new ${env.dockerUser}/node-app-test-new:latest"
                    sh "docker push ${dockerUser}/node-app-test-new:latest"
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
