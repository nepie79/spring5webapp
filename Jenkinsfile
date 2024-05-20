pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/nepie79/spring5webapp.git'
            }
            
        }
        stage('Build'){
            steps{
                 // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                
                stash includes: 'target/*.jar', name: 'builtJars'
            
                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
            
        }
        
        stage('Test') { 
            steps {
                unstash 'builtJars'
                sh 'mvn clean test' 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
        
        stage('Archive artifacts'){
            steps{
                unstash 'builtJars'
                archiveArtifacts 'target/*.jar'
            }
        }
    }
}
