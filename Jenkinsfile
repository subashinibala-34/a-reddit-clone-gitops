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
                git branch: 'main', credentialsId: 'github-ssh', url: 'git@github.com:subashinibala-34/a-reddit-clone-gitops.git'
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
                sshagent(['github-ssh']) {
                    sh """
                        git config --global user.name "subashinibala-34"
                        git config --global user.email "subabala1299@gmail.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest"
                        git push origin main
                    """
                }
            }
        }
    }
}
