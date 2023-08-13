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
        stage('Removing Images') {
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Updating Manifest Files') {
            steps{
                script{

                    sh"""
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    """
                }
            }
        }
        stage('Push Changes to Github') {
            steps{
                script{
                    sh """
                    git config --global user.name "harshau007"
                    git config --global user.email "amanupadhyay2004@gmail.com"
                    git add .
                    git commit -m "[Jenkins Updated] Manifest Files"
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh "git push https://github.com/harshau007/ArgoCD_Project.git main"
                    }
                }
            }
        }
    }
}