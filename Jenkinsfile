pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Avinashtalla/java-testing-session-01.git'
                }
            }
        }
        stage('Unit Testing'){
            
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
        
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonartoken') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        
        stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                       waitForQualityGate abortPipeline: false, credentialsId: 'sonartoken'
                    }
                }
        }
        
        stage('Upload jar file to nexus'){
            
            steps{
                
                script{
                    
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'hamcrest-core', 
                            classifier: '', file: 'target/java-testing-session-01.jar', 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: 'Nexus', 
                    groupId: 'org.hamcrest', 
                    nexusUrl: '54.158.228.107:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'demo-pipeline', 
                    version: '0.0.1'
                }
            }
        }
    }
}
