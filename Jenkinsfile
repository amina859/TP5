pipeline {
     agent {
        label 'windows'
    }
    
    environment {
        DOCKER_USERNAME = "bamita" 
        IMAGE_VERSION = "1.${BUILD_NUMBER}"  
        DOCKER_IMAGE = "${DOCKER_USERNAME}/tp-app:${IMAGE_VERSION}" 
        DOCKER_CONTAINER = "ci-cd-html-css-app"  
    }
    
    stages {
        
        stage("Checkout") {
            steps {
                git branch: 'master', url: 'https://github.com/amina859/TP5.git'
            }
        }
        
        stage("Test") {
            steps {
                echo "Tests en cours"
            }
        }
        
        stage("Build Docker Image") {
            steps {
                script {
                    bat "docker build -t $DOCKER_IMAGE ."
                }
            }
        }
        
        stage("Push image to Docker Hub") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '0000', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]){
                    bat """
                    docker login -u ${DOCKER_USER} -p ${DOCKER_PASSWORD}
                    echo 'Docker login successful'
                    docker push $DOCKER_IMAGE
                    """
                    }
                }
            }
        }
        
        stage("Deploy") {
            steps {
                script {
                   bat """
                        REM Arrête le conteneur s'il existe
                        docker container stop $DOCKER_CONTAINER || exit 0
                        REM Supprime le conteneur s'il existe
                        docker container rm $DOCKER_CONTAINER || exit 0
                        REM Lance un nouveau conteneur en mode détaché
                        docker container run -d --name $DOCKER_CONTAINER -p 8081:80 $DOCKER_IMAGE
                        """
                }
            }
        }
    }
}





















