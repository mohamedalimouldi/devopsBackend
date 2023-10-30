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
                junit '**/target/surefire-reports/*.xml'
            }
        }
        stage("Create SonarQube Project") {
            steps {
                script {
                    def sonarServerUrl = "http://192.168.249.128:9000/"
                    def projectName = "devopsBackend"
                    def projectKey = "devopsBackend"

                    sh """
                        curl -X POST "${sonarServerUrl}/api/projects/create?name=${projectName}&project=${projectKey}"
                    """
                }
            }
        }
        stage("Run SonarQube Analysis") {
            steps {
                script {
                    withSonarQubeEnv('test') {
                        def sonarUsername = "admin"
                        def sonarPassword = "123"

                        withCredentials([usernamePassword(credentialsId: 'sonartoken', usernameVariable: 'sonarUsername', passwordVariable: 'sonarPassword')]) {
                            sh """
                                set +x
                                mvn sonar:sonar -Dsonar.projectKey=devopsBackend
                                set -x
                            """
                        }
                        echo 'Static Analysis Completed'
                    }
                }
            }
        }
    }
}
