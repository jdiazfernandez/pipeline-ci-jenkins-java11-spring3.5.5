pipeline {
   
   agent any
   environment {
        JAVA_HOME = "/var/lib/jenkins/tools/hudson.model.JDK/jdk_11/amazon-corretto-11.0.26.4.1-linux-x64/"
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

   tools {
      jdk 'jdk_11' // Use the name you configured for JDK 11 in the Jenkins tools configuration
      maven 'maven'
   }

   stages {
        
      stage('Checkout') {
         steps {
               // Checkout code from version control including submodules using the scm variable
               checkout([
                  $class: 'GitSCM', 
                  branches: [[name: "${GIT_BRANCH}"]],
                  extensions: [[$class: 'SubmoduleOption', recursiveSubmodules: true]], 
                  userRemoteConfigs: scm.userRemoteConfigs
               ])
         }
      }

      stage('Build') {
            steps {
                sh 'mvn clean compile' // Remove the target directory and compile the code
            }
      }

      stage('Test') {
         steps {
               script {
                  sh 'mvn test' // Run the tests
               }
         }
      }

      stage('Package') {
         steps {
               sh 'mvn package' // Package the project (produces .jar or .war)
         }
      }

      stage('Archive Artifacts') {
         steps {
            // Archive the generated artifacts
            archiveArtifacts artifacts: 'target/*.jar, target/*.war', allowEmptyArchive: true
         }
      }
   }
}
