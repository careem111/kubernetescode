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
            app = docker.build 'careem785/argo-cd-test'
          }
      }
    stage('Test image') {
        steps{
                app.inside {
                    sh 'echo "Tests passed"'
        }
                  }
             }
    stage('Push image') {
        steps{
        docker.withRegistry('https://registry.hub.docker.com', 'docker') {
            app.push('${env.BUILD_NUMBER}')
        }
        }
    }
    stage('Trigger ManifestUpdate') {
        steps{
                echo "triggering update manifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }

          }
        }
 }

