properties([parameters([choice(choices: 'master\nfeature-1\nfeature-2', description: 'Select the branch to build', name: 'branch')])])
node ('master'){
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
  stage('Push to Nexus'){
        def mvnHome = tool name: 'apache-maven-3.5.4', type: 'maven'
        sh "${mvnHome}/bin/mvn deploy"
   }
  
   stage('Deploy to Tomcat'){
       sh label: '', script: '''cd /var/lib/jenkins/workspace/PipelineasCode
VERSION=`cat pom.xml | grep version -m1 | awk -F ">" \'{print $2}\' | awk -F "<" \'{print $1}\'`
cd /home/ansadmin/
rm -rf *.war
wget http://34.67.124.137:8081/repository/maven-releases/in/javahome/myweb/"${VERSION}"/myweb-"${VERSION}".war
mv myweb-"${VERSION}".war myweb.war
scp -rp *.war ansadmin@10.128.0.22:/home/ansadmin
'''
        
   }
   
}



