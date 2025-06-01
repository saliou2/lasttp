// declaration du pipeline jenkins
pipeline{
    // Exécute le pipeline sur n'importe quel agent
    agent any
    // Déclarer les variables d'environnement globale
    environment {
        DOCKER_USERNAME = 'saliou'
        IMAGE_VERSION = '1.${BUILD_NUMBER}'
        DOCKER_IMAGE = "${DOCKER_USERNAME}/tp55:${IMAGE_VERSION}"
        DOCKER_CONTAINER = 'tp5-app'
        
    }
    // Définir les étapes du pipeline
    stages {
        // étape 1: récupérer le code source depuis github
        stage('Checkout') {
            steps {
                // Cloner le dépôt Git
                git branch: 'master', url: https://github.com/saliou2/E-com-app.git'
            }
        }
        // étape 2: exécution des tests
        stage("tests") {
            steps {
                // Exécuter les tests (si nécessaire)
                echo 'tests en cours...'
            }
        }
        // étape 3: construire l'image docker

        stage('Build Docker Image') {
            steps {
                // Construire l'image Docker
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        // étape 4: déployer l'image docker sur docker hub
        stage('Push Docker Image') {
            steps {
                // Se connecter à Docker Hub
                script {
                    withCredentials([usernamePassword(credentialsId: '251102', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    }
                    sh "docker login -u ${DOCKER_USERNAME}"
                    echo 'Docker login réussi'
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        // étape 5: déployer l'image docker
        stage('Deploy Docker Container') {
            steps {
                // Déployer le conteneur Docker
                script {
                    bat """
                    docker stop ${DOCKER_CONTAINER} || true
                    docker rm ${DOCKER_CONTAINER} || true
                    docker run -d --name ${DOCKER_CONTAINER} -p 80:80 ${DOCKER_IMAGE}
                    """ 
                   

                }
            }
        }
    }

    }