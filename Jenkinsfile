
node {
   
def jdk = tool name: 'java8'
  env.JAVA_HOME = "${jdk}"

  echo "jdk installation path is: ${jdk}"

  // next 2 are equivalents
  sh "${jdk}/bin/java -version"

  // note that simple quote strings are not evaluated by Groovy
  // substitution is done by shell script using environment
  sh '$JAVA_HOME/bin/java -version'
	// This is to demo github action	
  
   def mvn = tool (name: 'maven3', type: 'maven') + '/bin/mvn'
   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
	credentialsId: 'github', 
	url: 'https://github.com/upretiramesh78/my-app-ramesh'
   
   }
   
  
      
   
   
	
   stage('Mvn Package'){
	   // Build using maven
	   
	   sh "${mvn} clean package"
   }
   
   stage('deploy-dev'){
       def tomcatDevIp = '3.81.114.181'
	   def tomcatHome = '/home/ec2-user/tomcat9/'
	   def webApps = tomcatHome+'webapps/'
	   def tomcatStart = "${tomcatHome}bin/startup.sh"
	   def tomcatStop = "${tomcatHome}bin/shutdown.sh"
	   
	   sshagent (credentials: ['tomcat-dev']) {
	      sh "scp -o StrictHostKeyChecking=no target/myweb*.war ec2-user@${tomcatDevIp}:${webApps}myweb.war"
          sh "ssh ec2-user@${tomcatDevIp} ${tomcatStop}"
		  sh "ssh ec2-user@${tomcatDevIp} ${tomcatStart}"
       }
   }
   stage('Email Notification'){
		mail bcc: '', body: """Hi Team, You build successfully deployed
		                       Job URL : ${env.JOB_URL}
							   Job Name: ${env.JOB_NAME}

Thanks,
DevOps Team""", cc: '', from: '', replyTo: '', subject: "${env.JOB_NAME} Success", to: 'upreti.ramesh@gmail.com'
   
   }
}

