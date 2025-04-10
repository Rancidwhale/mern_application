pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/Rancidwhale/mern_application.git'
            }
        }
        stage('Run Docker Compose') {
            steps {
                script{
                    sh 'docker-compose up -d'
                }
            }
        }
    }
}
