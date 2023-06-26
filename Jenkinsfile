@Library('jenkins_shared_lib') _

pipeline{
    
    agent any 

    parameters{
        choice(name: 'action', choices: 'create\ndelete', description: 'choose create/Destroy')
        string(name: 'ImageName', description: "name of the docker build" , defaultValue: 'javaapp')
        string(name: 'ImageTag', description: "tag of the docker build" , defaultValue: 'v1')
        string(name: 'AppName', description: "name of the Application" , defaultValue: 'springboot')
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
            when {expression { params.action == 'create' } }
                
                steps{
                    
                    script{
                        def credentialsId = 'sonar-api' 
                        QualityGateStatus(credentialsId)
                    }
                }
            }
            stage('Maven build'){
            when {expression { params.action == 'create' } }    
               steps{
                
                   script{
                     
                      dockerBuild("${params.ImageName}","${params.ImageTag}","${params.AppName}")
                   }
               }
            }
        }
        
}
