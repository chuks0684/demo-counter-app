@Library('jenkins_shared_lib') _

pipeline{
    
    agent any 

    parameters{
        choice(name: 'action', choices: 'create\ndelete', description: 'choose create/Destroy')
    }
    
    stages {
       
        
        stage('Git Checkout'){
        when {expression { params.action == 'create' } }
                
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/Tobirachel/demo-counter-app.git"
            )
            
                    
            }
        }
        stage('UNIT testing'){
        when {expression { params.action == 'create' } }
            
            steps{
                
                script{
                    
                    mvnTest()
                }
            }
        }
        stage('Integration testing'){
        when {expression { params.action == 'create' } }    
            steps{
                
                script{
                    
                    mvnIntegrationTest()
                }
            }
        }
        stage('Maven build'){
        when {expression { params.action == 'create' } }    
            steps{
                
                script{
                    
                    mvnBuild()
                }
            }
        }
        stage('Static code analysis: Sonarqube'){
        when {expression { params.action == 'create' } }
            
            steps{
                
                script{
                   def credentialsId = 'sonar-api' 
                   statiCodeAnalysis(credentialsId)
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
