pipeline {
    agent { label "slave-1" }
    environment {
              APP_NAME = "ngs-app-pipeline"
    }
  stages {
         stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ganeshkutchu/NGS-cd-repo'
               }
        }
       stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yml
                   sed -i "s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g" deployment.yml
                   cat deployment.yml
                """
            }
        }
       stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "Ganeshkutchu"
                   git config --global user.email "ganeshkutchu777@gmail.com"
                   git add deployment.yml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Ganeshkutchu/NGS-cd-repo main"
                }
            }
        }
      
    }
}
