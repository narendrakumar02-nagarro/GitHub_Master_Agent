pipeline {
    
  environment {
    registry = "narendrakumar02/aqt_practice_data"
    registryCredential = 'Docker_Token'
    dockerImage = ''
  }
    agent any
    stages {
    stage('Build and Analysis') { 
            steps {
               bat 'mvn clean package' 
            }
    }
     stage('SonarQube analysis') {
    withSonarQubeEnv(credentialsId: '0339bade8f18d1aa50ed6b6f7dba5fdc7d288577', installationName: 'Jenkins') { // You can override the credential to be used
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
    }
  }       
    }
    stage('Cloning Git') {
     steps {
        git 'https://github.com/narendrakumar02/AQTPracticeData.git'
      }
    }
    
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            
          }
        }
      }
    
    }
}
