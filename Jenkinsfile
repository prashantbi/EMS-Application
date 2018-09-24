node {
        slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} started"
  
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
           sh "mvn install -Dmaven.test.failure.ignore=true"

        /* Archive the test results */
           step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        }
       //stage('SCA'){
           sh "mvn sonar:sonar -Dsonar.organization=pushpendrad-github -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${env.SCA_TOKEN}"
        }
        stage('performance'){
           sh "mvn gatling:execute"
        }
        stage('Archive Artifact') {
        // Archive Artifact after build
          archiveArtifacts artifacts: 'target/EmployeeApplicationSprint4-1.0-SNAPSHOT.war'
        }
        timeout(time:5, unit:'DAYS') {
        input message:'Approve deployment?', submitter: 'admin'
}
        
notifications {
		success {
                slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"
			}
		failure {
                slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER}  failed"
			}
		unstable{
        slackSend color: "warning", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was unable"
        }
}     
}
