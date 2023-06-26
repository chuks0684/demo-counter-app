@Library('jenkins_shared_lib') _

pipeline{
    
    agent any 

    parameters{
        choice(name: 'actions', choices: 'create\delete', description: 'choose create/Destroy')
    }
    
    stages {
        when {expression { param.action == 'create' } }
        
        stage('Git Checkout'){
                
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/Tobirachel/demo-counter-app.git"
            )
            
                    
            }
        }
        stage('UNIT testing'){
        when {expression { param.action == 'create' } }
            
            steps{
                
                script{
                    
                    mvnTest()
                }
            }
        }
        stage('Integration testing'){
        when {expression { param.action == 'create' } }    
            steps{
                
                script{
                    
                    mvnIntegrationTest()
                }
            }
        }
        stage('Maven build'){
        when {expression { param.action == 'create' } }    
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
