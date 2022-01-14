pipeline {
    
  environment {
    registry = "narendrakumar02/aqt_practice_data"
    registryCredential = 'Docker_Token'
    dockerImage = ''
     //scannerHome = tool 'SonarScanner 4.0'
  }
    agent any
    stages {
    stage('Build and Analysis') { 
            steps {
               bat 'mvn clean package' 
            }
    }
    
    // stage('SonarQube analysis') {
      //   steps{
    //withSonarQubeEnv('My SonarQube Server') { // If you have configured more than one global server connection, you can specify its name
      //bat "${scannerHome}/bin/sonar-scanner"
    //}
  //}
    // } 
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
}
