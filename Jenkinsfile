pipeline {
    agent any

    environment {
        IMAGE_NAME = "webapp:${BUILD_NUMBER}"
        CONTAINER_NAME = "webapp_ctr"
    }

    stages {
        stage('Cloning Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/f1Costa/jk-public-gh.git'
            }
        }
        
        stage('Build Image') {
            steps {
                script {
                    def buildStatus = sh(script: 'docker build -t ${IMAGE_NAME} .', returnStatus: true)
                    if (buildStatus != 0) {
                        error("Docker build failed.")
                    }
                }
            }
        }
       
        stage('Deploying Application') {
            steps {
                script {
                    sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d -p 3000:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                    '''
                }
            }
        }
    }
}
