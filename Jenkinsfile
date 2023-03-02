pipeline{
    
    agent any
    environment {
        PATH = "/opt/maven3.6/bin/:$PATH"
    }
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'master', url: 'https://github.com/buddipammu2616/march2ndhelloworld.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('build code') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('sonar qube analysis') {
           steps { 
               script {
                    withSonarQubeEnv(credentialsId: 'sonar-credentials') {
                     sh 'mvn clean package sonar:sonar'
             }
         }
    }
}
        stage('quality gate check') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-credentials'
                }
            }
        }
        stage('artifacts upload') {
            steps {
                script {
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'buddipammu', 
                            classifier: '', 
                            file: 'target/buddipammu.war', 
                            type: 'war']
                    ], 
                        credentialsId: 'nexus-credentials', 
                        groupId: 'com.ustglobal', 
                        nexusUrl: '13.126.219.99:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'helloworldapp-release', 
                        version: '1.0.0'
    }
            }
        }
    }
}
