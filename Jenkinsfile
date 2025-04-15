pipeline{
    agent any
    environment {
        IMAGE_NAME1 = "samp-frontend" // Name of the image created in Jenkins
        IMAGE_NAME2 = "samp-backend" // Name of the image created in Jenkins
        CONTAINER_NAME1 = "samp-frontend-1" // Name of the container created in Jenkins
        CONTAINER_NAME2 = "samp-backend-1" // Name of the container created in Jenkins
    }
    stages{
        stage('git checkout'){
            steps{
                git 'https://github.com/Rancidwhale/mern_application.git'
            }
        }
        stage('Clean Up Docker Resources') {
            steps {
                script {
                    // Remove the specific container
                    sh '''
                    if docker ps -a --format '{{.Names}}' | grep -q $CONTAINER_NAME1; then
                        echo "Stopping and removing container: $CONTAINER_NAME1"
                        docker stop $CONTAINER_NAME1
                        docker rm $CONTAINER_NAME1
                    else
                        echo "Container $CONTAINER_NAME1 does not exist."
                    fi
                    '''
                    sh '''
                    if docker ps -a --format '{{.Names}}' | grep -q $CONTAINER_NAME2; then
                        echo "Stopping and removing container: $CONTAINER_NAME2"
                        docker stop $CONTAINER_NAME2
                        docker rm $CONTAINER_NAME2
                    else
                        echo "Container $CONTAINER_NAME2 does not exist."
                    fi
                    '''

                    // Remove the specific image
                    sh '''
                    if docker images -q $IMAGE_NAME1; then
                        echo "Removing image: $IMAGE_NAME1"
                        docker rmi -f $IMAGE_NAME1
                    else
                        echo "Image $IMAGE_NAME1 does not exist."
                    fi
                    '''
                    sh '''
                    if docker images -q $IMAGE_NAME2; then
                        echo "Removing image: $IMAGE_NAME2"
                        docker rmi -f $IMAGE_NAME2
                    else
                        echo "Image $IMAGE_NAME2 does not exist."
                    fi
                    '''
                }
            }
        }
        stage('docker-compose'){
            steps{
                script{
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
// pipeline {
//     agent any

//     stages {
//         stage('Git') {
//             steps {
//                 git 'https://github.com/Rancidwhale/mern_application.git'
//             }
//         }
//         stage('Run Docker Compose') {
//             steps {
//                 script{
//                     sh 'docker-compose up -d'
//                 }
//             }
//         }
//     }
// }
