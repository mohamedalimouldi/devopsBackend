pipeline {
    agent any
    tools {
    nodejs 'node'
    }


    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://192.168.249.128:8081"
        SONAR_SERVER_URL = "http://192.168.249.128:9000/"
        PROJECT_NAME = "devopsBackend"
        PROJECT_KEY = "devopsBackend"
        SONAR_USERNAME = "admin"
        SONAR_PASSWORD = "123"
        NEXUS_REPOSITORY = "Devops_Project"
        DOCKER_IMAGE_NAME = "dalidas/springboot_devops:latest"
        DOCKER_FRONT_IMAGE_NAME = "dalidas/devops_angular:latest"
	
    }

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
                sh 'mvn test'
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('Checkout Frontend code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mohamedalimouldi/devopsFront.git']]])
            }
        }
	

        stage('Build Angular') {
            steps {
                dir('frontend') {
                    sh 'npm version'
                    sh 'npm install'
                    sh 'npm install -g @angular/cli'
                    sh 'ng build '
                }
            }
        }

    }
}
