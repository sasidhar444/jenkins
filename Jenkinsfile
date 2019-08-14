node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/iamdaaniyaal/simple-maven-project-with-tests.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('Results') {
      //junit '**/target/surefire-reports/TEST-*.xml'
       junit '*TEST-*.xml'
      archiveArtifacts '*.jar'
  
      //gsutil cp 'target/*.jar' 'gs://jenkins--bucket'
     // scp 'target/*.jar' 'https://console.cloud.google.com/storage/browser/jenkins--bucket'
   }
   stage('Post') {
    googleStorageUpload bucket: 'gs://jenkins--bucket', credentialsId: 'daniyal-248906', pattern: '*.jar'
   }
  
 // stage('Ansible Deploy') {
   //   junit '**/target/surefire-reports/TEST-*.xml'
     // archiveArtifacts 'target/*.jar'
 //}
}
