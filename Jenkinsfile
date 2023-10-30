pipeline {
    agent any

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.249.128:8081"
    }

    stages {
        stage('Checkout Backend code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/mohamedalimouldi/devopsBackend.git']])
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
                    withSonarQubeEnv('sonrserver') {
                        def sonarUsername = "admin"
                        def sonarPassword = "123"

                        withSonarQubeEnv(credentialsId: 'sonartoken') {
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

        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    def nexusRepository = "Devops_Project"
                    pom = readMavenPom file: "pom.xml"
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}")
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path
                    artifactExists = fileExists artifactPath

                    if (artifactExists) {
                        echo "* File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}"
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: nexusRepository,
                            credentialsId: 'nexus-cred',
                            artifacts: [
                                [artifactId: pom.artifactId, classifier: '', file: artifactPath, type: pom.packaging],
                                [artifactId: pom.artifactId, classifier: '', file: "pom.xml", type: "pom"]
                            ]
                        )
                    } else {
                        error "* File: ${artifactPath}, could not be found"
                    }
                }
            }
        }

        stage('Build image spring') {
            steps {
                script {
                    // Build the Docker image for the Spring Boot app
                    sh "docker build -t dalidas/devops_backend ."
                }
            }
        }
    }
}
