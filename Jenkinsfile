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
                // Provjera ispravnosti Nginx konfiguracije
                sh 'docker run --rm antonio-coric-web nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // 1. Kopiramo u /tmp/ jer tu svatko ima dozvolu pisanja (ne treba sudo)
                    sh "cp index.html /tmp/index.html"
                    
                    // 2. Gasimo stari kontejner ako postoji i pokrećemo novi
                    // Montiramo datoteku direktno iz /tmp/ mape u Docker
                    sh """
                        docker rm -f web-server || true
                        docker run -d --name web-server -p 80:80 -v /tmp/index.html:/usr/share/nginx/html/index.html nginx:alpine
                    """
                }
            }
        }
    }
}
