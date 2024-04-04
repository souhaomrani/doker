pipeline {
    agent any
    
    environment {
        DOCKER_REGISTRY_URL = 'https://192.168.127.134:8006/api2/json'
        DOCKER_IMAGE_NAME = 'mon_grafana'
        DOCKER_CREDENTIALS_ID = 'votre_identifiant_de_credentials_docker'
    }
    
    stages {
        stage('Checkout SCM') {
            steps {
                // Vérifier le code source à partir du SCM (Système de Contrôle de Versions)
                checkout scm
            }
        }
        
        stage('Build and Push Grafana Docker Image') {
            steps {
                script {
                    // Construire l'image Docker avec le nom spécifié
                    def dockerImage = docker.build("${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}")

                    // Authentification Docker pour accéder au registre Docker
                    docker.withRegistry("${DOCKER_REGISTRY_URL}", "${DOCKER_CREDENTIALS_ID}") {
                        // Pousser l'image vers le registre Docker
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Build and Run Grafana') {
            steps {
                script {
                    // Run Grafana container
                    docker.image("${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}").run('-p 3000:3000 --name grafana -d')
                }
            }
        }

        stage('Build and Run Prometheus') {
            steps {
                script {
                    // Pull Prometheus Docker image
                    docker.image('prom/prometheus:latest').pull()

                    // Run Prometheus container
                    docker.image('prom/prometheus:latest').run('-p 9090:9090 --name prometheus -d')
                }
            }
        }
    }

    post {
        always {
            script {
                // Stop and remove containers
                docker.container('grafana').stop()
                docker.container('grafana').remove()

                docker.container('prometheus').stop()
                docker.container('prometheus').remove()
            }
        }
    }
}
