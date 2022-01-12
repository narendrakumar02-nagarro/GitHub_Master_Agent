pipeline {
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
        stage('Deliver') {
            steps {
                bat './jenkins/scripts/deliver.bat'
            }
        }
    }
}
