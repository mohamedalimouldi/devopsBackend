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
                        // Run tests and collect test results
                        sh 'mvn test' // Modify the test command as needed

                        // Archive test results for Jenkins to display
                        junit '**/target/surefire-reports/*.xml'
                    }
                }
                    stage('Build and Analyze') {
                               steps {
                                   script {
                                       // Run the SonarQube analysis
                                       sh 'mvn clean verify sonar:sonar ' +
                                          '-Dsonar.projectKey=sonar ' +
                                          '-Dsonar.projectName=\'sonar\' ' +
                                          '-Dsonar.host.url=http://192.168.249.128:9000/ ' +
                                          '-Dsonar.token=squ_ea8768b6bb299f0176afc819de0f70cf241a11b0'
                                   }
                               }
                           }

        
        
    }
}
