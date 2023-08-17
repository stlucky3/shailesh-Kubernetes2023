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
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh 'cd /var/jenkins_home/workspace/emb-prod/'
                        sh 'mvn sonar:sonar -Dsonar.host.url=http://192.168.0.113:9000 -Dsonar.login=sqa_b29a30437ebc0c8686a056234f11e94bee685bb1'
                           
                    }
                }
            }
        }
    }
}
