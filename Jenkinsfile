node {
  
        stage('Checkout') {
        // Cloning Repo
          git url: 'https://github.com/pushOrganization/EMS-Application.git'
        }    
        
        stage('build') {
        // Building Code
           sh "mvn clean install"
        }
        
        stage('Archive Artifact') {
            // Archive Artifact after build
          archiveArtifacts artifacts: 'target/EmployeeApplicationSprint4-1.0-SNAPSHOT.war'
        }
}
