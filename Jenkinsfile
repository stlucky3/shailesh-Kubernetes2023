pipeline {
    agent any
    environment {
        SONAR_URL = 'http://192.168.0.113:9000'
        NEXUS_URL = 'http://192.168.0.113:8081'
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
         stage('Deploy to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'spring-boot-starter-parent', classifier: '', file: 'target/spring-boot-web-1.0.jar', type: 'jar']], credentialsId: 'Nexus-Maven', groupId: 'org.springframework.boot', nexusUrl: '192.168.0.113:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'embitel-maven', version: '1.0'
            }
        }
    }
}
