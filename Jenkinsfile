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
                    # Arrête le conteneur s'il existe
                    docker container stop $DOCKER_CONTAINER || true
                    # supprime le conteneur s'il existe
                    docker container rm $DOCKER_CONTAINER || true
                    # Lance un nouveau conteneur en mode détaché(en arrière-plan )
                    docker container run -d --name $DOCKER_CONTAINER -p 8080:80 $DOCKER_IMAGE
                    """
                }
            }
        }
    }
}





















