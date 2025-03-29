pipeline {
    agent any
    environment {
        APP_NAME = "reddit-clone-app"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/subashinibala-34/a-reddit-clone-gitops'
            }
        }
        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "subashinibala-34"
                    git config --global user.email "subabala1299@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
                    sh "git push https://${GIT_USER}:${GIT_TOKEN}@github.com/subashinibala-34/a-reddit-clone-gitops.git main"
                }
            }
        }
    } // <-- Missing closing bracket added here
} // <-- Closing bracket for the pipeline block
