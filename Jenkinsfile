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
                    sh "cp index.html /tmp/index.html"
                    sh """
                        docker rm -f web-server || true
                        docker run -d --name web-server -p 80:80 -v /tmp/index.html:/usr/share/nginx/html/index.html nginx:alpine
                    """
                }
            }
        }
    }
}
