pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Preuzimanje koda s GitHub-a
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Izgradnja Docker image-a na Jenkins instanci
                sh 'docker build -t antonio-coric-web .'
            }
        }

        stage('Test') {
            steps {
                // Provjera ispravnosti Nginx konfiguracije
                sh 'docker run --rm antonio-coric-web nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def remoteIP = '98.94.100.200'
                    def remoteUser = 'ubuntu'

                    // 1. Kopiranje docker-compose i index datoteka na produkciju
                    sh "scp -o StrictHostKeyChecking=no docker-compose.yml index.html ${remoteUser}@${remoteIP}:/home/ubuntu/app/"

                    // 2. Pokretanje kontejnera na udaljenoj mašini
                    sh """
                        ssh -o StrictHostKeyChecking=no ${remoteUser}@${remoteIP} "
                            cd /home/ubuntu/app
                            docker-compose down || true
                            docker-compose up -d
                        "
                    """
                }
            }
        }
    }
}
