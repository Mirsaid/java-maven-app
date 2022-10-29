#!/urs/bin/env groovy
pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('test'){
            steps {
                script {
                    echo "Testing the application"
                }
            }
           } 
        stage("build jar") {
            steps {
                script {    
                    echo "Building jar file"
            
                }
            
            }
        
        }
        
        stage("deploy") {
            steps {
                script {
		    def DockerCmd = 'docker run -d -p 80:80 mirsaidalizade/demo-app:1.0'
                    sshagent(['my-key']){
			sh "ssh -o StrictHostKeyChecking=no ec2-user@44.202.141.204 ${dockerCmd}"
			}
                
                }
            
            }
        
        }
        
    }


}
