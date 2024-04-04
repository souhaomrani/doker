pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'votre_identifiant_de_credentials_docker' // ID des informations d'identification Docker dans Jenkins
        DOCKER_IMAGE_NAME = 'mon_grafana' // Nom de votre image Docker
        DOCKER_REGISTRY_URL = 'votre_registry_docker_url' // URL de votre registre Docker
        GRAFANA_PORT = '3000' // Port sur lequel Grafana sera exposé
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
                    docker.build("${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}")
                    
                    // Authentification Docker pour accéder au registre Docker
                    docker.withRegistry("${DOCKER_REGISTRY_URL}", "${DOCKER_CREDENTIALS_ID}") {
                        // Pousser l'image vers le registre Docker
                        docker.image("${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}").push()
                    }
                }
            }
        }
        
        stage('Run Grafana') {
            steps {
                script {
                    // Exécuter l'image Docker de Grafana, en exposant le port spécifié
                    docker.image("${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}").run("-p ${GRAFANA_PORT}:3000 --name grafana -d")
                }
            }
        }
    }
}
