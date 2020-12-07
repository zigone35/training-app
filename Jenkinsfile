node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/walidsaad/training-app.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven3'
   }
 
        
   stage('Build') {
      // Run the maven build
         sh "'${mvnHome}/bin/mvn' -B -DskipTests -Dmaven.test.failure.ignore clean package"
   }
   
   stage('Unit Test') {
         sh "'${mvnHome}/bin/mvn' test"
   }
   
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts  'target/*.jar'
   }   
   stage('Build Docker Image') {
         sh "BUILD_TAG='v1.0' docker-compose -f docker-compose.yml build"

   }
     stage('Deploy App') {
         sh "docker-compose up -d"
         sh "docker logs training-app-jenkinsfile_app_1"

   }
}
