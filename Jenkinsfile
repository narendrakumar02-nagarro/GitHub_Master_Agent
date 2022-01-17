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
                /* def server = Artifactory.server 'Artifactory'
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
         buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
         server.publishBuildInfo buildInfo      */
         
         def server = Artifactory.server "Artifactory"
  def buildInfo = Artifactory.newBuildInfo()
  buildInfo.env.capture = true
  def rtMaven = Artifactory.newMavenBuild()
  //rtMaven.tool = MAVEN_TOOL // Tool name from Jenkins configuration
  rtMaven.opts = "-Denv=dev"
  rtMaven.deployer releaseRepo:'example-repo-local', snapshotRepo:'example-repo-local', server: server
  //rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server

  rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo

  buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
  // Publish build info.
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
