def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger'
    ]
pipeline{
    agent any
    environment {
        IMAGE_NAME1 = "mern-pipeline-frontend " // Name of the image created in Jenkins
        IMAGE_NAME2 = "mern-pipeline-backend " // Name of the image created in Jenkins
        CONTAINER_NAME1 = "mern-pipeline-frontend-1" // Name of the container created in Jenkins
        CONTAINER_NAME2 = "mern-pipeline-backend-1" // Name of the container created in Jenkins
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages{
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('git '){
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
        stage('Code Analysis'){
            steps{
                withSonarQubeEnv('sonar-slave'){
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Chat_Room \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Chat_Room'''
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
         stage('docker push'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker-cred'){
                        sh 'docker tag $IMAGE_NAME1 abdullah77044/$IMAGE_NAME1'
                        sh 'docker tag $IMAGE_NAME2 abdullah77044/$IMAGE_NAME2'
                        sh 'docker push abdullah77044/$IMAGE_NAME1'
                        sh 'docker push abdullah77044/$IMAGE_NAME2'
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'slack Notification.'
            slackSend channel: '#sample',
            color: COLOR_MAP [currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URl}"
            
        }
    }
}
//comment
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
//only 