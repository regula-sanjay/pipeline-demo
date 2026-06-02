pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '1', description: 'Image version')
    }

    environment {
        IMAGE = "regulasanjay/myapp"
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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
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
                docker stop $CONTAINER || true
                docker rm $CONTAINER || true
                docker run -d -p 8081:80 --name $CONTAINER $IMAGE:${params.VERSION}
                """
            }
        }
    }
}
