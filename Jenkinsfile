pipeline {
    agent any

    environment {
        PM_API_URL = "https://192.168.127.134:8006/api2/json"
        PM_USER = "root"
        PM_PASSWORD = "rootroot"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'  // Exemple pour Maven
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'  // Exemple pour Maven
            }
        }

        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'  // Exemple pour Kubernetes
            }
        }
    }

    post {
        success {
            echo 'Le pipeline s\'est exécuté avec succès!'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
