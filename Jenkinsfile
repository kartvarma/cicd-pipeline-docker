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
    		when {
    			branch 'master'
    		     }
    		steps {
    		   script {
    			app=docker.build("dochubcred/node-app")
    			app.inside {
    				sh 'echo $(curl 54.244.72.37:8081)'
    			 }
    	       }
    	     }
    	}
    	
    	stage('Push Docker Image') {
    		when {
    			branch 'master'
    		    }
    		steps {
    		  script {
    			docker.withRegistry('https://registry.hub.docker.com','dochubcred') {
    			app.push("${env.BUILD_NUMBER}")
    			app.push("latest")
    			}
    		  }
            }
    	}    
   }
}
