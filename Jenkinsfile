pipeline {
    agent any
    tools {
        maven '3.8.6'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: 'target/simple-app-3.0.0.war', 
                            type: 'war']
                            ], 
                            credentialsId: 'nexuscredenciales', 
                            groupId: 'in.javahome', 
                            nexusUrl: 'localhost:8081', 
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: 'http://localhost:8081/repository/app-init/', 
                            version: '3.0.0'
            }
        }
    }
}
}
