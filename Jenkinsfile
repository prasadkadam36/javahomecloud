properties([parameters([choice(choices: 'master\nfeature-1\nfeature-2', description: 'Select the branch to build', name: 'branch')])])
node ('Linux_Slave'){
  stage('SCM Checkout'){
    git url: 'https://github.com/prasadkadam36/javahomecloud.git', branch: "${params.branch}"
  }
   stage('Compile-Package'){
     
    def mvnHome = tool name: 'apache-maven-3.5.4', type: 'maven'
     sh "${mvnHome}/bin/mvn package"
   }
  stage('Code-Analysis'){
    def mvnHome = tool name: 'apache-maven-3.5.4', type: 'maven'
     withSonarQubeEnv('Sonar-7')
    {
      sh "${mvnHome}/bin/mvn sonar:sonar"
    }
   }
  stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }        
    stage('Deploy to Nexus'){
       def pom = readMavenPom file: 'pom.xml'
      echo "${pom.version}"
    def mvnHome = tool name: 'apache-maven-3.5.4', type: 'maven'
      sh "${mvnHome}/bin/mvn deploy"
   }
   
}



