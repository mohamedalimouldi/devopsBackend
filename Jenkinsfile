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
                sh 'sudo sudo /usr/bin/mvn clean install'
            }
        }
        
        
    }
}
