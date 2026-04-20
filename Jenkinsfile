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
            // 1. Kopiramo index.html direktno u home mapu (bez scp-a)
            sh "cp index.html /home/ubuntu/"

            // 2. Brišemo stari kontejner ako postoji i pokrećemo novi
            // Koristimo direktne docker naredbe jer smo već na localhostu
            sh """
                docker rm -f web-server || true
                docker run -d --name web-server -p 80:80 -v /home/ubuntu/index.html:/usr/share/nginx/html/index.html nginx:alpine
            """
        }
    }
}
