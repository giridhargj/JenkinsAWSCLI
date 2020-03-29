node {
  
	stage('RunAWSCMD') {
	    def awscliImage = docker.build("awscli-image", ".") 
	    try {
		    withCredentials([usernamePassword(credentialsId: 'aws-cli', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
			 docker.image("awscli-image:latest").inside("--entrypoint \"\" -e $AWS_ACCESS_KEY_ID -e $AWS_SECRET_ACCESS_KEY -e AWS_DEFAULT_REGION") {
		    	 sh "aws s3 ls"
			}
		    }
		   
        } catch( ClassNotFoundException e ) {
        		throw new Exception("Docker Pipeline plugin required")
    	}
	}
	  
    cleanWs()
}
