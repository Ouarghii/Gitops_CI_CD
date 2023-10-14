pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "Raslen"
        APP_NAME = "Gitops_CI_CD"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/${APP_NAME}"
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
                    docker_image = docker.build "${IMAGE_TAG}"
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'Raslen', passwordVariable: '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')]) {
                        def registryCredentials = """{
                            "auths": {
                                "https://index.docker.io/v1/": {
                                    "auth": "${DOCKERHUB_USERNAME}:${DOCKERHUB_PASSWORD}"
                                }
                            }
                        }"""
                        sh "echo '$registryCredentials' | docker login --username $DOCKERHUB_USERNAME --password-stdin"
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', REGISTRY_CREDS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }
    }
}
