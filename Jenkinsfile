pipeline {
    agent any
    


    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.249.128:8081"
        SONAR_SERVER_URL = "http://192.168.249.128:9000/"
        PROJECT_NAME = "devopsBackend"
        PROJECT_KEY = "devopsBackend"
        SONAR_USERNAME = "admin"
        SONAR_PASSWORD = "123"
        NEXUS_REPOSITORY = "Devops_Project"
        DOCKER_IMAGE_NAME = "dalidas/springboot_devops:latest"
        DOCKER_FRONT_IMAGE_NAME = "dalidas/devops_angular:latest"
	
    }

    

        stage('Checkout Frontend code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mohamedalimouldi/devopsFront.git']]])
            }
        }
	

        stage('Build Angular') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build '
                }
            }
        }
	 stage('Build image Angular') {
            steps {
                sh "docker build -t ${DOCKER_FRONT_IMAGE_NAME} ."
            }
        }

        stage('Push image Angular') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push ${DOCKER_FRONT_IMAGE_NAME}"
                }
            }
        }

    }
}
