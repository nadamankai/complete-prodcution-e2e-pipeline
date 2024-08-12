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
            DOCKER_PASS = 'nadanadou'
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
           stage("Sonarqube Scan") {
                    steps {
                        script {
                            withSonarQubeEnv(installationName: 'sq1') {
                                sh "mvn sonar:sonar"}
                    }}
                }
                stage("Quality Gate") {
                            steps {
                                script {
                                    waitForQualityGate abortPipline : false , credentialsId: 'jenkins-sonarqube'
                            }}
                        }
                            stage("Build & push docker image")     {
                                   steps {
                                      script {
                                         docker.withRegistry('',DOCKER_PASS) {
                                             docker_image = docker.build "${IMAGE_NAME}"
                                         }
                                         docker.withRegistry('',DOCKER_PASS) {
                                              docker_image.push("${IMAGE_TAG}")
                                              docker_image.push('latest')
                                                          }
                                      }
                                   }
                                }
    }
}
