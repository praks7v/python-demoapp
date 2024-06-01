pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/praks7v/python-demoapp.git'
            }
        }
        
        stage('OWASP Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./' , odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Build') {
            steps {
                sh "make image"
            }
        }
        
        stage('Docker Push') {
            steps {
                script {
                    withDockerRegistry(url: 'http://localhost:5000') {
                        sh "make push"
    
                    }
                }
            }
            
        }
        
        stage('Docker Deploy') {
            steps {
                script {
                    withDockerRegistry(url: 'http://localhost:5000') {
                        sh "docker images"
                        sh "docker run -d --rm -it -p 5001:5001 localhost:5000/python-demoapp:latest"
    
                    }
                }
            }
        }
        
    }
}
