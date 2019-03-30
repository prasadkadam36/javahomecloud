  node{
   stage('SCM Checkout'){
     git 'https://github.com/javahometech/my-app'
   }
   stage('Compile-Package'){
    
      def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"
   }
    stage('Email Notification'){
     mail bcc: '', body: 'Hello from Jenkins', cc: '', from: '', replyTo: '', subject: 'Jenkins Subject', to: 'prasadforplaystore@gmail.com' 
    }
   
}


