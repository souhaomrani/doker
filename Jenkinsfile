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
                // Étape de build - Exemple de commande pour construire votre application
                sh 'mvn clean package'  // Exemple pour Maven
                // ou
                // sh 'npm install'       // Exemple pour Node.js
            }
        }

        stage('Test') {
            steps {
                // Étape de test - Exemple de commande pour exécuter vos tests
                sh 'mvn test'  // Exemple pour Maven
                // ou
                // sh 'npm test'  // Exemple pour Node.js
            }
        }

        stage('Deploy') {
            steps {
                // Étape de déploiement - Exemple de commande pour déployer votre application
                sh 'kubectl apply -f deployment.yaml'  // Exemple pour Kubernetes
                // ou
                // sh 'ansible-playbook deploy.yml'     // Exemple pour Ansible
            }
        }
    }

    post {
        success {
            // Actions à effectuer en cas de succès du pipeline
            echo 'Le pipeline s\'est exécuté avec succès!'
            // Par exemple, envoyer une notification
            // slackSend channel: '#builds', color: 'good', message: "Build réussi pour ${env.JOB_NAME}"
        }
        failure {
            // Actions à effectuer en cas d'échec du pipeline
            echo 'Le pipeline a échoué.'
            // Par exemple, envoyer une notification
            // slackSend channel: '#builds', color: 'danger', message: "Build échoué pour ${env.JOB_NAME}"
        }
    }
}

