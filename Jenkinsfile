pipeline {
    agent any

    tools{
        jdk 'jdk17'
    }
     environment{
        SCANNER_HOME= tool 'sonar-scanner'
     }


    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/PS2320/python-app-jenkins-demo.git'
            }
        }
    
    
    
        stage('OWASP') {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC' 
                   dependencyCheckPublisher pattern: '**/dependency-check-report.xml'

            }
        }
    
        stage('Trivy Fs Scan') {
            steps {
                sh "trivy fs."
            }
        }
    
    
     stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonar') {
                  sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Python-app -Dsonar.projectKey=python-app "
                }
               
            }
        }
            stage('Docker Build & tag') {
            steps {
                script{
                    withDockerRegistry(credentialsID: 'docker-cred', toolName: 'docker'){
                        sh "make image"
            }
        }
            }
            }

       stage('Trivy Image Scan') {
            steps {
                sh "trivy image ps2320/python-webapp:latest"
            }
        } 

        stage('Docker Push') {
            steps {
                script{
                    withDockerRegistry(credentialsID: 'docker-cred', toolName: 'docker'){
                        sh "make push"
            }
        }
            }
            }
        stage('Deploy to docker container') {
            steps {
                script{
                    withDockerRegistry(credentialsID: 'docker-cred', toolName: 'docker'){
                        sh "docker run -d -p 5000:5000 ps2320/python-app:latest"
            }
        }
            }
            }
}
}
