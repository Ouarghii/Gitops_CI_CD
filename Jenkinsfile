pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = credentials('dockerhub').username
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/Gitops_CI_CD"
        IMAGE_TAG = "${BUILD_NUMBER}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages {
        stage('Cleanup workspace') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM') {
            steps {
                script {
                    git credentialsId: 'github',
                    url: 'https://github.com/Ouarghii/Gitops_CI_CD.git',
                    branch: 'main'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'Raslen', passwordVariable: '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')]) {
                        sh """echo '\${DOCKERHUB_PASSWORD}' | docker login --username '\${DOCKERHUB_USERNAME}' --password-stdin"""
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', REGISTRY_CREDS) {
                        dockerImage.push("${IMAGE_TAG}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}
