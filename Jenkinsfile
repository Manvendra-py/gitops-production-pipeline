pipeline {
    agent any

    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Source Code checkout") {
            steps {
                git branch: "main", credentialsId: "github", url : "https://github.com/Manvendra-py/gitops-production-pipeline.git"
            }
        }

        stage("Update the Deployment Tag") {
            steps{
                sh """
                cat deployment.yaml
                sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                cat deployment.yaml
                """
            }
        }

        stage("Push Deployment file into git") {
            steps {
                sh """
                git config --global user.name "Manvendra"
                git config --global user.email "manav.singh.ms@gmail.com"
                git add deployment.yaml
                git commit -m "Updated Deployment Manifest"
                git push
                """
            }
        }
    }
}