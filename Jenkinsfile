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
                                    waitForQualityGate abortPipline : false , installationName: 'sq1'
                            }}
                        }
    }
}
