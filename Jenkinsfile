node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/ayushchoudhary1214/microservice1997.git'
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
   stage('Sonarqube Analysis'){
       withEnv(["MVN_HOME=$mvnHome"]){
           bat(/"%MVN_HOME%\bin\mvn" sonar:sonar/)
       }
       
   }
   stage('Nexus Analysis'){
       nexusPublisher nexusInstanceId: 'firstnexus', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: './pom.xml']], mavenCoordinate: [artifactId: 'spring-boot-web-jsp', groupId: 'org.springframework.boot', packaging: 'war', version: '1.0']]]
   }
}
