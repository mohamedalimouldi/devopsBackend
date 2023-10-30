pipeline {
    agent any
    stages {
        stage('Checkout Backend code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mohamedalimouldi/devopsBackend.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('test'){
            steps {
                sh 'mvn test'
            }
        }
        
        
    }
}
