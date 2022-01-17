pipeline {
    environment {
        registry = "narendrakumar02/aqt_practice_data"
        registryCredential = 'Docker_Token'
        dockerImage = ''
        //scannerHome = tool 'SonarScanner 4.0'
               }
    
    agent any
    
    stages {
        stage('Git Checkout') {
     steps {
        git 'https://github.com/narendrakumar02/AQTPracticeData.git'
      }
    }
        stage('Building Project') {
            steps {
                bat 'mvn clean package'
                  }   
                       }
    
        stage('SonarQube Analysis') {
                steps{
    
    bat 'mvn sonar:sonar'
    
       }
       }
        
stage('Uploading Artifacts') {
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
    
    
    stage('Building Docker Image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Pushing Docker-Image to DockerHub') {
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
