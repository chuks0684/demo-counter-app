@Library('jenkins_shared_lib') _

pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
                
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/Tobirachel/demo-counter-app.git"
            )
            
                    
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    mvnTest()
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    mvnIntegrationTest()
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    mvnBuild()
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
        }
        
}
