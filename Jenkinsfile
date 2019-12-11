pipeline {
    
   agent any

    stages {
	stage('Checkout SCM'){
            
            steps {
                checkout([$class: 'GitSCM',
				branches: [[name: '*/master']],
				doGenerateSubmoduleConfigurations: false,
				extensions: [], 
				submoduleCfg: [], 
				userRemoteConfigs: [[url: 'https://github.com/FilipCh/coursework_2.git']]])            }
        }
        
	
        stage('SonarQube Testing') {
            environment {
                scanner = tool 'SonarQubeScanner'
            }
            
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scanner}/bin/sonar-scanner -D sonar.login=admin -D sonar.password=admin"
                }
				 
            }
        }
       
        
        
        stage('Build Docker Image And Push To Dockerhub') {
            steps {
                script {
                    def app = docker.build("filipch/coursework_2")
                   docker.withRegistry("https://registry.hub.docker.com", "docker-hub-credentials") {
                   app.push("${env.BUILD_NUMBER}")
                    
                }
            }
          }
        }
    }
	}