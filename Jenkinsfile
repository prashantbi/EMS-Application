node {
  
        stage('Checkout') {
        // Cloning Repo
          git url: 'https://github.com/pushOrganization/EMS-Application.git'
        }    
        
        stage('build') {
        // Building Code
           sh "mvn clean install -DskipTests"
        }
        stage('Test') {
        // Testing Code
          // runTests()
            sh "mvn install -Dmaven.test.failure.ignore=true"

  /* Archive the test results */
            step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        }
        
        stage('Archive Artifact') {
            // Archive Artifact after build
          archiveArtifacts artifacts: 'target/EmployeeApplicationSprint4-1.0-SNAPSHOT.war'
        }
 timeout(time:5, unit:'DAYS') {
    input message:'Approve deployment?', submitter: 'admin'
}
slackSend baseUrl: 'https://jenkinsbuild.slack.com/services/hooks/jenkins-ci/', botUser: true, channel: '#jenkinsbuild', color: '#439FE0', failOnError: true, message: 'slack build failure', tokenCredentialId: 'jenkinsSlack'
}

void runTests(def args) {
  /* Call the Maven build with tests. */
  mvn "install -Dmaven.test.failure.ignore=true"

  /* Archive the test results */
  step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
}


