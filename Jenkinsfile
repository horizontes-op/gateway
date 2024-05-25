pipeline {
    agent any
    environment {
        K8S_PORT = 80
    }
    stages {
        stage('auth') {
            steps {
                build job: 'auth', wait: true
            }
        }
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
        stage('Trivy scan') {
            steps {
                 sh 'trivy fs --exit-code 1 --severity HIGH,CRITICAL,MEDIUM, LOW . | tee trivy-scan-report.txt'
                
            }
        }
        stage('Deploy on k8s') {
            steps {
                sh "kubectl apply -f ./k8s/gateway.yaml"
            }
        }     
    }
}
