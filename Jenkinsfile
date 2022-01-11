pipeline {
        agent { docker { image 'maven:3.3.3' } }
        stages {
            stage('build') {
                environment {
                  HOME="."
                }
                steps {
                    sh 'mvn --version'
                }
           }
        }
    }
