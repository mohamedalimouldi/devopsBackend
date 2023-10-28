pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code from Git
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mohamedalimouldi/devopsBackend.git']]])
            }
        }
        
        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean install'
            }
        }
    }
}
