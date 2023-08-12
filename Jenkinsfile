pipeline{
    agent any

    environment{
        DOCKERHUB_USER = "harshau04"
        APP_NAME = "test"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USER}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages{
        stage('Checkout Git') {
            steps{
                script{
                    git branch: 'main', url: 'https://github.com/harshau007/ArgoCD_Project.git'
                }
            }
        }
    }
}