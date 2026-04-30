
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Dohvaćanje koda s GitHub repozitorija
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Izgradnja Docker imagea na Jenkins instanci
                sh 'docker build -t antonio-coric-web .'
            }
        }

        stage('Test') {
            steps {
                // Testiranje ispravnosti konfiguracije
                sh 'docker run --rm antonio-coric-web nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def remoteIP = '98.94.100.200'
                    def remoteUser = 'ubuntu'

                    // 1. Slanje datoteka na produkcijsku EC2 instancu
                    sh "scp -o StrictHostKeyChecking=no docker-compose.yml index.html ${remoteUser}@${remoteIP}:/home/ubuntu/app/"

                    // 2. Pokretanje na produkciji uz korištenje sudo ovlasti
                    sh """
                        ssh -o StrictHostKeyChecking=no ${remoteUser}@${remoteIP} "
                            cd /home/ubuntu/app
                            sudo docker-compose down || true
                            sudo docker-compose up -d
                        "
                    """
                }
            }
        }
    }
}
