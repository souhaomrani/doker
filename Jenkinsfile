pipeline {
    agent any    
    environment {
        PM_API_URL = "https://192.168.127.134:8006/api2/json"
        PM_USER = "root"
        PM_PASSWORD = "rootroot"
    }
    stages {
        stage('Test GitHub Connection') {
            steps {
                script {
                    def gitUrl = 'https://github.com/souhaomrani/doker.git'
                    // Checkout the GitHub repository using configured credentials
                    checkout([$class: 'GitSCM',
                              branches: [[name: '*/main']],
                              doGenerateSubmoduleConfigurations: false,
                              extensions: [[$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true]],
                              userRemoteConfigs: [[url: gitUrl]]])
                    echo "Connection to GitHub repository successful"
                }
            }
        }
        stage('Install Prometheus') {
            steps {
                script {
                    // Pull Prometheus image
                    docker.image('prom/prometheus').pull()
                    // Start Prometheus container
                    docker.container('prometheus')
                        .withRun("--name prometheus -p 9090:9090")
                        .run('prom/prometheus')
                }
            }
        }
        stage('Install Grafana') {
            steps {
                script {
                    // Pull Grafana image
                    docker.image('grafana/grafana').pull()
                    // Start Grafana container
                    docker.container('grafana')
                        .withRun("--name grafana -p 3000:3000")
                        .run('grafana/grafana')
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                // Ajoutez ici les étapes de construction de votre application
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // Ajoutez ici les étapes de test de votre application
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Ajoutez ici les étapes de déploiement de votre application
            }
        }
    }
}
