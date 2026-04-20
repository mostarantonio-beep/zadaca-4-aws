pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Uzima najnoviji kod s GitHub-a
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Buildamo Docker sliku na Jenkins serveru
                sh 'docker build -t antonio-coric-web .'
            }
        }

        stage('Test') {
            steps {
                // Provjera je li Nginx konfiguracija unutar slike ispravna
                sh 'docker run --rm antonio-coric-web nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // 1. Šaljemo tvoj index.html na produkcijsku instancu
                    sh "scp -o StrictHostKeyChecking=no index.html ubuntu@34.203.194.188:/home/ubuntu/"
                    
                    // 2. Brišemo stari kontejner ako postoji i pokrećemo novi čisti Nginx
                    // Koristimo tvoj index.html koji smo upravo poslali
                    sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@34.203.194.188 '
                        docker rm -f web-server || true
                        docker run -d --name web-server -p 80:80 -v /home/ubuntu/index.html:/usr/share/nginx/html/index.html nginx:alpine
                        '
                    """
                }
            }
        }
    }
}
