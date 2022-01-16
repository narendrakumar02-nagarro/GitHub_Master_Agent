pipeline {
    
  environment {
    registry = "narendrakumar02/aqt_practice_data"
    registryCredential = 'Docker_Token'
    dockerImage = ''
     //scannerHome = tool 'SonarScanner 4.0'
  }
    agent any
    stages {
    stage('Build') { 
            steps {
               bat 'mvn clean package' 
            }
    }
    
    stage('SonarQube analysis') {
       steps{
    
    bat 'mvn sonar:sonar'
    
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
            docker run -d -p 8090 ${dockerImage}
          }
        }
      }
    }
    }
}
