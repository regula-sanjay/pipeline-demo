pipeline {
    agent any
      parameters {
        string(name: 'PORT', defaultValue: '8081', description: 'Port Number')
    }
    
    stages {
        // your stages
    }

    environment {
        IMAGE_NAME = "sanjayregula/my-nginx-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {

                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE_NAME:latest
                    docker logout
                    '''
                }
            }
        }

        stage('Run Container') {
    steps {
        sh '''
        docker pull $IMAGE_NAME:latest

        docker stop nginx-demo || true
        docker rm nginx-demo || true

        docker run -d \
        --name nginx-demo \
        -p 9090:80 \
        $IMAGE_NAME:latest
        '''
            }
        }
    }
}
