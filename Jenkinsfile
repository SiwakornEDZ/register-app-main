pipeline {
    agent { label "Jenkins-Agent" }
    tools {
        jdk 'Java17'
        maven 'Maven3'  
    }
   stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
         stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/SiwakornEDZ/register-app-main'
               }
        }
         stage("Build Appliction"){
             steps {
               sh "mvn clean package"
             }
         }
        stage("SonarQube Analysis"){
            steps {
	              script {
		                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                    sh "mvn sonar:sonar"
		              }
	            }	
          }
       }
	stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }	
             }
        }
   }
}
