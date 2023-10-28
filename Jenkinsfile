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
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test' // Modify the test command as needed
            }
            post {
                always {
                    // Archive test results for Jenkins to display
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
    }
}
