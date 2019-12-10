pipeline {
    
   agent any

    stages {
        stage('Checkout SCM'){
            
            checkout scm
        }
	
        stage('SonarQube Code Analysis') {
            environment {
                scanner = tool 'SonarQubeScanner'
            }
            
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scanner}/bin/sonar-scanner -D sonar.login=admin -D sonar.password=admin"
                }
            }
        }
        
        stage('Quality Gate'){
            steps {
               timeout(time: 5, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
               }
            }
        }
        
        stage('Build & Push Docker Image') {
            steps {
                script {
                    def app = docker.build("FilipCh/coursework_2")
                    docker.withRegistry("https://registry.hub.docker.com", "docker_credentials") {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                }
            }
          }
        }
    }
	}