pipeline {
    agent none
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
    }
}