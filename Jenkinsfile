pipeline {
    agent any
    

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.249.134:8081"
        SONAR_SERVER_URL = "http://192.168.249.134:9000/"
        PROJECT_NAME = "devopsBackend"
        PROJECT_KEY = "devopsBackend"
        SONAR_USERNAME = "admin"
        SONAR_PASSWORD = "123"
        NEXUS_REPOSITORY = "Devops_Project"
        DOCKER_IMAGE_NAME = "dalidas/springboot_devops:latest"
        DOCKER_FRONT_IMAGE_NAME = "dalidas/devops_angular:latest"
	
    }

    stages{

        stage('Checkout Frontend code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mohamedalimouldi/devopsFront.git']]])
            }
        }
	

        
	      stage('Deploy application with monitoring') {
                        steps {
                          sh 'docker-compose up -d'  // Use -d to run containers in the background
                            
                        }
                    }

    }
	 post {
    always {
        script {
            try {
                emailext subject: 'Build Notification', body: 'The build was successful', recipientProviders: [[$class: 'CulpritsRecipientProvider']]
            } catch (Exception e) {
                echo "Error sending email: ${e.getMessage()}"
            }
        }
    }
}

}
