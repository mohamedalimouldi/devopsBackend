pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    // Run tests and collect test results
                    sh 'mvn test' // Modify the test command as needed

                    // Archive test results for Jenkins to display
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
    }
}
