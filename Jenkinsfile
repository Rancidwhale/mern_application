pipeline{
    agent any
    stages{
        stage("git"){
            steps{
                git 'https://github.com/Rancidwhale/mern_application.git'
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
