pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Povlačenje koda s GitHub repozitorija
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Izgradnja Docker slike
                sh 'docker build -t antonio-coric-web .'
            }
        }

        stage('Test') {
            steps {
                // Provjera ispravnosti Nginx konfiguracije unutar slike
                sh 'docker run --rm antonio-coric-web nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // 1. Kopiramo index.html u /tmp/ (tu Jenkins ima dozvolu pisanja)
                    sh "cp index.html /tmp/index.html"
                    
                    // 2. Čistimo stari kontejner i pokrećemo novi
                    // Montiramo datoteku direktno iz /tmp/ mape
                    sh """
                        docker rm -f web-server || true
                        docker run -d --name web-server -p 80:80 -v /tmp/index.html:/usr/share/nginx/html/index.html nginx:alpine
                    """
                }
            }
        }
    }
}
