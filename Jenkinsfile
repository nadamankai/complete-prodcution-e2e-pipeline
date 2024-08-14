pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
            APP_NAME = "complete-prodcution-e2e-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "nadamankai"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"

        }

    stages {

        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/nadamankai/complete-prodcution-e2e-pipeline'
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }


       stage('Check Docker Connectivity') {
                   steps {
                       script {
                           // Check Docker version
                           sh 'docker --version'

                           // List running containers (should return an empty list if none are running)
                           sh 'docker ps'
                       }
                   }
               }
    }
}
