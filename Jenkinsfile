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
		sh 'echo "Successful"'
		}
	}

 
}
