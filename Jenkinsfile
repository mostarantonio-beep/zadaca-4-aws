pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t antonio-coric-web .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm antonio-coric-web nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def remoteIP = '98.94.100.200'
                    def remoteUser = 'ubuntu'

                    // Slanje datoteka na produkcijsku instancu
                    sh "scp -o StrictHostKeyChecking=no docker-compose.yml index.html ${remoteUser}@${remoteIP}:/home/ubuntu/app/"

                    // Pokretanje na produkciji putem Docker Compose
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
