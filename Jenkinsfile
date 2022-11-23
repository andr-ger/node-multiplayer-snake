node ('Ubuntu-App-Server') {  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    stage('SAST') {
	build 'SECURITY-SAST-SNYK'
    }
	
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
	 app = docker.build("andrger/snake")
    }
    stage('Post-to-dockerhub') {
	
	docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("latest")
        			}
         }
       
    stage('SECURITY-IMAGE-SCANNER') {
	build 'Container-Security-Trivy'	
    }
	
    stage('Pull-image-server') {
		
         sh "docker-compose down"
         sh "docker-compose up -d"
      }
	
    stage('DAST') {
	build 'Container-Security-Trivy'
    }
}
