pipeline {
    agent any
    stages {
        stage('gateway') {
            steps {
                echo 'gateway'
            }
        }
        stage('build gateway') { 
            steps {
                sh 'mvn clean install'
            }
        } 

        stage('build image gateway') {
            steps {
                script {
                    account = docker.build("fernandowi55/gateway:${env.BUILD_ID}", "-f Dockerfile .")
                }
            }
        }
        stage('push image gateway') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credential') {
                        account.push("latest")
                        account.push("${env.BUILD_ID}")
                       
                    }
                }
            }
        }     
    }
}