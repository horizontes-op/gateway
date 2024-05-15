pipeline {
    agent any
    environment {
        K8S_PORT = 80
    }
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

        stage('Deploy on k8s') {
            steps {
                withCredentials([ string(credentialsId: 'minikube-credential', variable: 'api_token') ]) {
                    sh "kubectl --token $api_token --server https://host.docker.internal:${env.K8S_PORT}  --insecure-skip-tls-verify=true apply -f ./k8s/gateway.yaml"
                   
                }
            }
        }     
    }
}