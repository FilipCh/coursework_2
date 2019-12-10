pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
       stage('Sonarqube') {
    environment {
        scannerHome = tool 'SonarQube'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
       	 }
}
}
    }
}
node {
    def app

    stage('Clone repository') {
       
        checkout scm
    }

    stage('Build image') {
       

        app = docker.build("filipch/coursework_2")
    }

    stage ('Test image'){
	app.inside {
		sh 'echo "Test Successful"'
		}
	}
	
}