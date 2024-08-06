pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'nada', credentialsId: 'github', url: 'https://github.com/nadamankai/complete-prodcution-e2e-pipeline'
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

        stage("Sonarqube Analysis") {
            steps {
                script{
                    def sonarQubeUrl = "http://localhost:9000" // Replace with your SonarQube server URL if different
                                        def sonarQubePing = sh(script: "curl -s -o /dev/null -w '%{http_code}' $sonarQubeUrl", returnStdout: true)
                                        if (sonarQubePing != "200") {
                                            error "SonarQube server is not reachable: ${sonarQubePing}"
                                        }

                                        withSonarQubeEnv(credentialsId: "jenkins-sonarqube-token") {
                                            sh 'mvn clean package sonar:sonar'
                                        }
                }
            }
        }
    }
}
