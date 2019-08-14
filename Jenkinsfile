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
      junit '**/target/surefire-reports/TEST-*.xml'
     //junit '**/something/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
  
      //gsutil cp 'target/*.jar' 'gs://jenkins--bucket'
     // scp 'target/*.jar' 'https://console.cloud.google.com/storage/browser/jenkins--bucket'
   }
   stage('Post') {
      //gsutil cp 'target/*.jar' 'gs://jenkins--bucket'
    googleStorageUpload bucket: 'gs://jenkins--bucket', credentialsId: 'daniyal-248906', pattern: 'target/*.jar'
   }
  
 stage('Ansible Deploy') {
  sh 'ls -lrt'
  sh 'ansible --version'
  //sh 'ansible all -m ping -c 3 -i inventory'
  sh 'ansible-playbook playbook.yml -i inventory'
}
}
