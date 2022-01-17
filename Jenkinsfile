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
        
stage('upload') {
           steps {
               script { 
                 def server = Artifactory.server 'Artifactory'
                 def buildInfo = Artifactory.newBuildInfo()
                 buildInfo.env.capture = true
                 buildInfo.env.collect()

                 def uploadSpec = """{
                    "files": [{
            "pattern": "**/target/*.jar",
            "target": "example-repo-local"
          }, {
            "pattern": "**/target/*.pom",
            "target": "example-repo-local"
          }, {
            "pattern": "**/target/*.war",
            "target": "example-repo-local"
          }]
                 }"""

         server.upload spec: uploadSpec, buildInfo: buildInfo
         
         server.publishBuildInfo buildInfo      
         
                  
               
    
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
           // docker run -d -p 8090 ${dockerImage}
          }
        }
      }
    }
    }
}
