pipeline {
    agent any
	parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'manual-build', description: 'Enter the custom tag for the image')
    }
    environment {
        DOCKER_HUB_USER = 'bharathiio'
        IMAGE_NAME      = 'monkeygg2-site'
        DOCKER_CREDS    = 'docker-credential'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Building and Tagging the Docker Image') {
            steps {
                script {
                    appImage = docker.build("${DOCKER_HUB_USER}/${IMAGE_NAME}:${params.IMAGE_TAG}")
                }
            }
        }
        stage('Pushing to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDS) {
                        appImage.push()
                        appImage.push("latest")
                    }
                }
            }
        }
    }
    post {
        always {
            sh "docker rmi ${DOCKER_HUB_USER}/${IMAGE_NAME}:${params.IMAGE_TAG} || true"
            cleanWs()
        }
    }
}