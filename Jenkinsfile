#!/urs/bin/env groovy


pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
	stage('incrementing app version'){
		steps {
			script {
				echo 'incrementing app version ...'
				sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit'
				def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
				def version = matcher[0][1]
				env.IMAGE_NAME = "$version-$BUILD_NUMBER"
			
			}	
		}
	}
        
        stage("build jar") {
            steps {
                script {    
                    echo "building the app"
			sh 'mvn clean package'
            
                }
            
            }
        
        }
       stage("build image") {
            steps {
                script {    
			echo "building the docker image"
        		withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASSWORD', usernameVariable: 'USER')]){
        			sh 'docker build -t mirsaidalizade/demo-app:${IMAGE_NAME} .'
        			sh "echo $PASSWORD | docker login -u $USER --password-stdin"
                		sh "docker push mirsaidalizade/demo-app:${IMAGE_NAME}"
       			 }	
                                      
                }
            
            }
        
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploying app to EC2"
                
                }
            
            }
        
        }
	stage("commit version update"){
	  steps {
	    script{
	      withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]){
		sh 'git config --global user.email "mirsaidaliazde6@gmail.com"'
		sh 'git config --global user.name "mirsaid"'
		sh 'git status'
		sh 'git branch'
		sh 'git config --list'
		
		sh "git remote set-url origin https://${USER}:${PASS}@github.com/Mirsaid/java-maven-app.git"
		sh 'git add .'
		sh 'git commit -m "ci: version bump"'
		sh 'git push origin HEAD:jenkins-jobs'
}	
}
}
}
        
    }


}

