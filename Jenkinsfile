pipeline {
    agent any
    environment {
        SONAR_URL = 'http://192.168.0.113:9000'
        NEXUS_URL = 'http://192.168.0.113:8081'
        ECR_REPO = '076984480633.dkr.ecr.us-east-1.amazonaws.com' 
        Repo_name = 'embitel-ecr'
	    region = 'us-east-1'
    }
    stages {
        stage('Checkout From GitHub Repository') {
            steps {
                checkout scm
            }
        }
         stage('Maven Tool Build and Test Code') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }
        stage('SonarQube Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar -Dsonar.host.url=$SONAR_URL'
                           
                    }
                }
            }
        }
         stage('Deploy to Nexus Repository') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'spring-boot-starter-parent', classifier: '', file: 'target/spring-boot-web.jar', type: 'jar']], credentialsId: 'Nexus-Maven', groupId: 'org.springframework.boot', nexusUrl: '192.168.0.113:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'embitel-maven', version: '1.0'
            }
        }
       stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $ECR_REPO/$Repo_name:latest .'
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
		    withAWS( credentials: 'aws-credentials-id') {
                    sh "aws ecr get-login-password --region $region | docker login --username AWS --password-stdin $ECR_REPO"
                    sh "docker push $ECR_REPO/$Repo_name:latest"
                }
		}	
            }
        }  
    }
}
