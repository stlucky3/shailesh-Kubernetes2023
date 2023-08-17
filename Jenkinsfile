pipeline {
    agent any

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
    
    }
}
