pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ubuntu-apache:latest'
        GIT_REPO = 'https://github.com/Ratnesh10101/website.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'develop', url: "${GIT_REPO}"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").inside {
                        sh 'echo "Running tests..."'
                        // Add your test commands here
                    }
                }
            }
        }

        stage('Publish to Apache') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").inside {
                        sh 'echo "Publishing to Apache..."'
                        sh 'cp -r * /var/www/html/'
                        sh 'service apache2 start'
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

