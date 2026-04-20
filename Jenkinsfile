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
                    sh "scp -o StrictHostKeyChecking=no docker-compose.yml ${PROD_USER}@${PROD_SERVER_IP}:/home/${PROD_USER}/"
                    sh "ssh -o StrictHostKeyChecking=no ${PROD_USER}@${PROD_SERVER_IP} 'sudo docker-compose up -d --build'"
                }
            }
        }
    }
}