pipeline {
   agent any


   stages {
      stage('Build') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/fhoubart/cicd-A-2022.git'

            // Run Maven on a Unix agent.
            bat "mvn -Dmaven.test.failure.ignore=true clean package"

            // To run Maven on a Windows agent, use
            // bat "mvn -Dmaven.test.failure.ignore=true clean package"
         }

      }
      stage("Test") {
         steps {
            bat "mvn checkstyle:checkstyle"
         }
         post {
            // If Maven was able to run the tests, even if some of the test
            // failed, record the test results and archive the jar file.
            success {
               junit '**/target/surefire-reports/TEST-*.xml'
               recordIssues(tools: [junitParser()])
               recordIssues(tools: [checkStyle()])
               archiveArtifacts 'target/*.jar'
            }
         }
      }
   }
}
