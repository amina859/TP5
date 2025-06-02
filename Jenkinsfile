pipeline {
    agent { label 'windows' }

    environment {
        DOCKER_USERNAME = 'bamita'
        IMAGE_VERSION = "1.${BUILD_NUMBER}"
        DOCKER_IMAGE = "${DOCKER_USERNAME}/tp-app:${IMAGE_VERSION}"
        DOCKER_CONTAINER = 'ci-cd-html-css-app'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/amina859/TP5.git'
            }
        }

        stage('Test') {
            steps {
                echo 'Tests en cours'
                bat 'dir index.html || exit 1'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% . || exit 1"
            }
        }

        stage('Push image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '0000', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    bat '''
                    echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USER% --password-stdin
                    docker push %DOCKER_IMAGE%
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                docker container stop %DOCKER_CONTAINER% || exit 0
                docker container rm %DOCKER_CONTAINER% || exit 0
                docker container run -d --name %DOCKER_CONTAINER% -p 8080:80 %DOCKER_IMAGE%
                '''
            }
        }
    }

    post {
        success {
            echo 'Déploiement réussi 🎉'
        }
        failure {
            echo 'Échec du pipeline ❌'
        }
    }
}
