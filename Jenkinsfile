pipeline {
    agent any
    stages {
        stage('Jenkins Gateway') {
            steps {
                echo 'Jenkins Gateway'
            }
        }
        stage('Build') { 
            steps {
                sh 'mvn clean install'
            }
        }      
    }
}