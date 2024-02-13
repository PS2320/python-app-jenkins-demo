pipeline {
    agent any
    
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
    
    
       stage('sonar') {
            steps {
                sh "trivy fs."
            }
        }
    
    
        stage('Trivy Fs Scan') {
            steps {
                sh "trivy fs."
            }
        }
    
    
     stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv(sonar) {
                  sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Python-app -Dsonar.projectKey=python-app "
                }
               
            }
        }
        
}
}
