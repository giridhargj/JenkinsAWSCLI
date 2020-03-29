node {
    def useEnvironment = env.ENVIRONMENT
    def serviceName = env.SERVICE_NAME
    def useVersion = env.VERSION
    def cfnBucketName = ""
    stage('Checkout') {
    echo "in checkout"
       // Clone repo
	git branch: 'master', 
	url: 'https://github.com/giridhargj/JenkinsAWSCLI.git'
    
    echo "After checkout"
    
    echo "After checkout 11111"
    }
    stage('ReadParamsFile') {
		def props = readJSON file: "parameters/${useEnvironment}.json"
		props.each { 
			if (it.ParameterKey == 'CloudFormationS3Bucket') {
				cfnBucketName = it.ParameterValue
			}
		}
	}
	stage('EunAWSCMD') {
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
