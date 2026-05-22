pipeline{
    agent { label "w1" }

    environment {
        IMAGE_NAME = 'pro-custom-image'
        IMAGE_TAG = "${BUILD_NUMBER}"
        GITHUB_REPO = 'https://github.com/whodeepaksoni/Production-Grade-Multi-Cloud-DevOps-Platform'
    }

    stages{
        stage('checkout scm'){
            steps{
                script{
                    echo "Checking out source code from SCM..."
                    git branch: 'main', url: "${GITHUB_REPO}"
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    echo "Building Docker image ${IMAGE_NAME}:${IMAGE_TAG}..."
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }
        stage('image tagging'){
            withcredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
            steps{
                script{
                    echo "Tagging Docker image ${IMAGE_NAME}:${IMAGE_TAG}..."
                    sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
            }
        }
        stage('Push Docker Image to Docker Hub'){
            withcredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
            steps{
                script{
                    echo "Pushing Docker image ${IMAGE_NAME}:${IMAGE_TAG} to Docker Hub..."
                    sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                    sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
            }
        }
        
        stage('deploy to production'){
            steps{
                script{
                    echo "removing old Docker containers from production..."
                    sh "docker rm -f ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} || true"
                    echo "Deploying Docker image ${IMAGE_NAME}:${IMAGE_TAG} to production..."
                    sh "docker run -d -p 3000:80 --name ${IMAGE_NAME} ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
        echo "Docker image ${IMAGE_NAME}:${IMAGE_TAG} deployed successfully to production!"; else { echo "Failed to deploy Docker image ${IMAGE_NAME}:${IMAGE_TAG} to production." }
    }            
}
