pipeline {
    agent any

    environment {
        BACKEND_IMAGE = 'myorg/backend:latest'
        DOCKER_REGISTRY = 'docker.io' // Changez si nécessaire
        DOCKER_CREDENTIALS = 'docker-credentials-id' // ID des identifiants Jenkins pour Docker
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Récupération du code depuis un dépôt Git
                git branch: 'main', url: 'https://github.com/nodejs.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Construire l'image Docker du backend
                    dir('backend') {
                        sh 'docker build -t ${BACKEND_IMAGE} .'
                    }
                }
            }
        }

      
        stage('Push Docker Images') {
            steps {
                script {
                    // Se connecter au registre Docker et pousser les images
                    withDockerRegistry([credentialsId: DOCKER_CREDENTIALS, url: "https://${DOCKER_REGISTRY}"]) {
                        sh "docker push ${BACKEND_IMAGE}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé.'
        }
        success {
            echo 'Pipeline exécuté avec succès.'
        }
        failure {
            echo 'Une erreur s\'est produite.'
        }
    }
}
