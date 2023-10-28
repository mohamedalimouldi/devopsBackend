
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
        // Checkout your source code from Git
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://your-git-repo-url.git']]])
    }
        }
        
        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean package'
            }
        }
        
        stage('Unit Tests') {
            steps {
                // Execute unit tests using Maven
                sh 'mvn test'
            }
        }
    }
    
}


