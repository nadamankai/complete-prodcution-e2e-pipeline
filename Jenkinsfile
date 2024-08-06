pipeline{
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'nada', credentialsId: 'github', url: 'https://github.com/nadamankai/complete-prodcution-e2e-pipeline'
            }

        }
        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }
         stage("Test Application"){
              steps {
                 sh "mvn test"
              }

              }
        }

}