node{
   stage('SCM Checkout'){
     git 'https://github.com/kumari10/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
 
   stage('Build Docker Imager'){
   sh 'docker build -t kumarianandhan/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u kumarianandhan -p ${dockerPassword}"
    }
   sh 'docker push kumarianandhan/myweb:0.0.2'
   }
   
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest kumarianandhan/myweb:0.0.2' 
   }
}
}


