node{
  stage('SCM Checkout'){
    git 'https://github.com/prasadkadam36/javahomecloud.git' 
  }
   stage('Compile-Package'){
    def mvnHome = tool name: 'apache-maven-3.5.4', type: 'maven'
     sh "${mvnHome}/bin/mvn package"
   }
}



