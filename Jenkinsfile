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
        stage('Dockerize Application') {
            steps{
                script{
                    docker_img = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push to DockerHub') {
            steps{
                script{
                    docker.withRegistry('', REGISTRY_CREDS) {
                        docker_img.push("${BUILD_NUMBER}")
                        docker_img.push("latest")
                    }
                }
            }
        }
    }
}