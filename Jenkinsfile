pipeline {
    agent any
    environment {
        SONAR_URL = 'http://192.168.0.113:9000'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
         stage('Build and Test') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar -Dsonar.host.url=$SONAR_URL'
                           
                    }
                }
            }
        }
    }
}
