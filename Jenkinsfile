pipeline {
    agent none
    environment {
    PROJECT_DIR = '/var/www/elinka_random_numbers'
    }
    stages {
        stage("deps") {
            agent {
                docker {
                    image 'node:latest'
                    args '-u root'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage("build") {
            agent {
                docker {
                    image 'node:latest'
                    args '-u root'
                }
            }
            steps {
                sh 'npm run build_prod'
            }
        }
        stage("deploy") {
            agent any
            steps {
                withCredentials(
                    [
                        string(credentialsId: "production_ip", variable: 'SERVER_IP'),
                        sshUserPrivateKey(credentialsId: "production_key", keyFileVariable: 'SERVER_KEY', usernameVariable: 'SERVER_USERNAME')
                    ]
                ) {
                    sh 'ssh -i ${SERVER_KEY} ${SERVER_USERNAME}@${SERVER_IP} mkdir -p ${PROJECT_DIR}'
                    sh 'scp -i ${SERVER_KEY} -r dist/* ${SERVER_USERNAME}@${SERVER_IP}:${PROJECT_DIR}'
                }
            }
        }
    }
}