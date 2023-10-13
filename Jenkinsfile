pipeline {

    agent any

    environment{
        DOCKERHUB_USERNAME = "Raslen"
        APP_NAME = "Gitops_CI_CD"
        IMAGE_TAGE = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" +"${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages{
        stage('Cleanup workspace'){
            steps{
                script {

                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/Ouarghii/Gitops_CI_CD.git',
                    branch: 'main'
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                   docker_image = docker.build "${IMAGE_TAGE}" 
                }
            }
        }
        stage('Docker Login') {
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'Raslen', passwordVariable: 'warghui110')]) {
                sh "echo \$DOCKERHUB_PASSWORD | docker login --username \$DOCKERHUB_USERNAME --password-stdin"
            }
        }
    }
}

        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push("latest")
                    }
                }
            }
        }
    }
}
