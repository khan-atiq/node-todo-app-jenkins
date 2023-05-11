pipeline {
    agent {
        label "dev-server"
    }
    stages {
        stage("code clone") {
            steps {
                git url: "https://github.com/khan-atiq/node-todo-app-jenkins.git", branch: "main"
            }
        }
        stage("build & test") {
            steps {
                sh "docker build . -t node-app-test"
            }
        }
        stage("push") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass" ,usernameVariable:"dockerhubuser")]) {
                    sh "docker tag node-app-test ${env.dockerhubuser}/node-todo-app-new:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/node-todo-app-new:latest"
                }
            }
        }
        stage("deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
