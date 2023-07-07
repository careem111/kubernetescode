pipeline{
    agent any
   
    stages {
      stage('Checkout SCM') {
    	  steps{
                checkout scm
	        }
        }      
    stage('Build image'){
        steps{
            script{
            app = docker.build 'careem785/argocdtest'
            }
          }
      }
    stage('Push image') {
        steps{
            script {
            docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            app.push("${env.BUILD_NUMBER}")
            }
        }
        }
    }
    stage('Trigger ManifestUpdate') {
        steps{
            script{
                echo "triggering update manifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
            }
        }

          }
        }
 }

