pipeline {
    agent any
    
    stages {
	 
    	 stage('Build') {
    	    steps {
    		    echo 'Running build Automation' 
    		    sh './gradlew build --no-daemon'
    		    archiveArtifacts artifacts: 'dist/trainSchedule.zip'
    	       }		
    	    }

    	stage ('Build Docker Image') {
    		steps {
    		   script {
    			app=docker.build("dochub/node-app")
    			app.inside {
    				sh 'echo $(curl localhost:8081)'
    			 	}
    	       		}
    	    	 }
    	}
    	
    	stage('Push Docker Image') {
    		steps {
    		  script {
    			docker.withRegistry('https://registry.hub.docker.com','dochub') {
    			app.push("${env.BUILD_NUMBER}")
    			app.push("latest")
    			}
    		  }
            }
    	}    
   }
}
