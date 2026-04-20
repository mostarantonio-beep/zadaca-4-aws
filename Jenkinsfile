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
                // Provjera je li Nginx konfiguracija ispravna
                sh 'docker run --rm antonio-coric-web nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Kopiramo index.html lokalno (jer je Jenkins na istoj mašini)
                    sh "cp index.html /home/ubuntu/ || sudo cp index.html /home/ubuntu/"
                    
                    // Gasimo stari kontejner ako postoji i pokrećemo novi
                    sh """
                        docker rm -f web-server || true
                        docker run -d --name web-server -p 80:80 -v /home/ubuntu/index.html:/usr/share/nginx/html/index.html nginx:alpine
                    """
                }
            }
        }
    }
}
