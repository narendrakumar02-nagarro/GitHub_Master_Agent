pipeline {

    agent {
   
            label 'java-docker-slave'
    }
   /* options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }       */

    stages {
        
        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/dovelop/test']], 
                    userRemoteConfigs: [[url: 'https://github.com/narendrakumar02/AQTPracticeData.git']]
                ])
            }
        }

        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                bat """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage(' Unit Testing') {
            steps {
                bat """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
            steps {
                bat """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Build Deploy Code') {
            when {
                branch 'develop'
            }
            steps {
                bat """
                echo "Building Artifact"
                """

                bat """
                echo "Deploying Code"
                """
            }
        }

    }   
}
