pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'kirahigashi/demo-web' 
        DOCKER_TAG = 'latest'
        REGISTRY_CREDENTIALS = 'dockerhub-id' 
        GIT_REPO = 'https://github.com/vtnHAN-SOLO/23127180_23127533.git'
        CONTAINER_NAME = 'website-do-an'
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('2. Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('3. Push to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: REGISTRY_CREDENTIALS, passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                        sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }

        stage('4. Deploy Container') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    
                    sh "docker run -d -p 8082:80 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
}
