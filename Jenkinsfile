pipeline{
    // agent {label 'sonar'}
    agent any
    stages{
       /*stage('Git Checkout Stage'){
            steps{
                git branch: 'main', url: 'https://github.com/tranju664/Sonar-Qube-war-example.git'
            }
         }*/       
       stage('Build Stage'){
            steps{
                sh 'mvn clean install'
            }
         }
    }
        post {
    success {
        script {
            def server = Artifactory.newServer(
                url: 'http://35.154.174.146:8081/artifactory',
                credentialsId: 'abc'
            )

            def rtMaven = Artifactory.newMavenBuild()

            rtMaven.deployer(
                server: server,
                releaseRepo: 'libs-release',
                snapshotRepo: 'libs-snapshot'
            )

            rtMaven.deployer.deployArtifacts = true
            rtMaven.tool = 'maven'

            // DO NOT rebuild — just publish build-info
            rtMaven.run(
                pom: 'pom.xml',
                goals: 'install'
            )
        }
    }
}
}
