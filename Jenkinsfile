pipeline {
    
  environment {
    registry = "narendrakumar02/aqt_practice_data"
    registryCredential = 'Docker_Token'
    dockerImage = ''
  }
    agent any
    stages {
    stage('Build') { 
            steps {
                bat 'mvn clean package' 
            }
    }
    stage('Test') {
            steps {
                bat 'mvn test'
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
}
