pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages{
        stage('Git checkout') {
            steps{
                git url : 'https://github.com/Faseeha001/CICD-pipeline-Build-and-park.git',branch :'main'
            }
            
        }
        stage('Build Maven Application') {
            steps{
                sh 'mvn clean package'
            }
        }
        stage ('Uplaod war file to Nexus') {
          steps{
            nexusArtifactUploader artifacts: [
               [
                   artifactId: 'hiring',
                   classifier: '',
                   file: 'target/hiring.war',
                   type: 'war'
               ]
            ],
            credentialsId: 'nexus-credentials',
            groupId: 'in.javahome',
            nexusUrl: '15.223.120.78:8081',
            nexusVersion: 'nexus3',
            protocol: 'http',
            repository: 'testdemo-app',
            version: '0.1'
          }
        }
    } 
}