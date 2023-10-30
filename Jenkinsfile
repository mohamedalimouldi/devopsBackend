node {
  stage('SCM') {
    git 'https://github.com/mohamedalimouldi/devopsBackend.git'
  }
  stage('SonarQube analysis') {
    withSonarQubeEnv(credentialsId: 'squ_ea8768b6bb299f0176afc819de0f70cf241a11b0', installationName: 'My SonarQube Server') { // You can override the credential to be used
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
    }
  }
}
