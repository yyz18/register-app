pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java21'
        maven 'Maven3'
    }
    environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "ashfaque9x"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/yyz18/register-app'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }

       
    }

    post {
       failure {
             emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                      subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                      mimeType: 'text/html',to: "ashfaque.s510@gmail.com"
      }
      success {
            emailext body: '''${SCRIPT, template="groovy-html.template"}''', 
                     subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                     mimeType: 'text/html',to: "ashfaque.s510@gmail.com"
      }      
   }
}
