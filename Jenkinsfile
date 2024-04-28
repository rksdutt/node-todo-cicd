pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/rksdutt/node-todo-cicd.git", branch: "master"
                echo 'code cloned from repo'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-catalog ."
                echo 'code build is done'
            }
        }
        stage("scan image"){
            steps{
                echo 'image scanning has done'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhubRepo",passwordVariable:"dockerhubRepoPass",usernameVariable:"dockerhubRepoUser")]){
                sh "docker login -u ${env.dockerhubRepoUser} -p ${env.dockerhubRepoPass}"
                sh "docker tag node-app-catalog:latest ${env.dockerhubRepoUser}/node-app-catalog:latest"
                sh "docker push ${env.dockerhubRepoUser}/node-app-catalog:latest"
                echo 'image pushed to repo'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment has done'
            }
        }
    }
}
