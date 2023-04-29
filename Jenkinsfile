pipeline {
    agent any
    tools {
        maven "maven 3.9.1"
    }
    stages {
        stage("build maven"){
            steps {
                sh "mvn clean install"
            }
        }
        stage("static code analasis"){
            steps {
                withSonarQubeEnv("sonarqube 8.9.9"){
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage("Quality Gate Status"){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_pwd'
                }
            }
        }
    }
}
