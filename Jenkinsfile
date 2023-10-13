pipeline {

    agent any

    environment{
        DOCKERHUB_USERNAME = "Raslen"
        APP_NAME = "Gitops_CI_CD"
        IMAGE_TAGE = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" +${APP_NAME}
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
    }
}
