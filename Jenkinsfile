pipeline {
    agent any
    tools {
        maven 'maven 3.9.1'
    }
    stages {
        stage("build maven"){
            steps {
                sh 'mvn clean install'
            }
        }
        stage("test"){
            steps{
                sh 'mvn test'
            }
        }
        stage("static code analasis"){
            steps {
                withSonarQubeEnv("sonarqube 8.9.9"){
                    sh "mvn sonar:sonar"
                }
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deployments'){
            parallel {
                stage ("Deploy to Staging"){
                    steps {
                        deploy adapters: [tomcat7(credentialsId: 'tomcat_pwd', path: '', url: 'http://3.109.121.131:8080/')], contextPath: null, war: '**/*.war'
                    }
                }
            }
        }
    }
}
