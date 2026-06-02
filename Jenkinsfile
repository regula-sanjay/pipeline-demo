pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '1', description: 'Image version')
    }

    environment {
        IMAGE = "yourdockerhubusername/myapp"
        CONTAINER = "myapp-container"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t $IMAGE:${params.VERSION} ."
            }
        }

        stage('Login DockerHub') {
            steps {
                sh "echo PASSWORD | docker login -u USER --password-stdin"
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push $IMAGE:${params.VERSION}"
            }
        }

        stage('Delete Local Image') {
            steps {
                sh "docker rmi -f $IMAGE:${params.VERSION} || true"
            }
        }

        stage('Pull Image Again') {
            steps {
                sh "docker pull $IMAGE:${params.VERSION}"
            }
        }

        stage('Run Container') {
            steps {
                sh """
                docker stop myapp-container || true
                docker rm myapp-container || true
                docker run -d -p 8081:80 --name myapp-container $IMAGE:${params.VERSION}
                """
            }
        }
    }
}
