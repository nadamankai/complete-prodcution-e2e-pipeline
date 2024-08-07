pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
       APP_NAME= "complete_production-e2e-pipline"
       RELEASE = "1.0.0"
       DOCKER_USER ="nadamankai"
       DOCKER_PASS = 'dockerhub'
       IMAGE_NAME = `${DOCKER_USER}/${APP_NAME}`
       IMAGE_TAG = `${RELEASE}-${BUILD_NUMBER}`
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


    }
}
