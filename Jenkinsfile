pipeline {
    agent any
    environment {
        PROD_SERVER_IP = "OVDJE_CES_STAVITI_IP_DRUGE_INSTANCE"
        PROD_USER = "ubuntu"
    }
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Build') { steps { sh 'docker-compose build' } }
        stage('Test') { steps { sh 'docker run --rm antonio-coric-web:latest nginx -t' } }
        stage('Deploy') {
            steps {
                script {
                 sh "scp -o StrictHostKeyChecking=no docker-compose.yml ubuntu@3.88.129.0:/home/ubuntu/"
sh "ssh -o StrictHostKeyChecking=no ubuntu@3.88.129.0 'docker-compose up -d'"
                }
            }
        }
    }
}
