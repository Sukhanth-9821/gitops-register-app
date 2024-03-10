pipeline {
    agent any
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'GITPAT', url: 'https://github.com/Sukhanth-9821/gitops-register-app.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's|sukhanth/registerapp:latest|sukhanth/registerapp:${IMAGE_TAG}|g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "Sukhanth-9821"
                   git config --global user.email "sukhanthsukh1234h@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GITPAT', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Sukhanth-9821/gitops-register-app.git main"
                }
            }
        }
      
    }
}
