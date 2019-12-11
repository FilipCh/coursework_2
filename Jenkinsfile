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
				 timeout(time: 10, unit: 'MINUTES'){
                            waitForQualityGate abortPipeline: true
                        }
            }
        }
        
        
        
        stage('Build Docker Image') {
            steps {
                script {
                    def app = docker.build("filipch/coursework_2")
                   
                }
            }
          }
        }
    }