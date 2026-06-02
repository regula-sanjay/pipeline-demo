pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '1.0', description: 'Enter build version')
        choice(name: 'ENV', choices: ['dev', 'test', 'prod'], description: 'Select environment')
        booleanParam(name: 'CLEAN', defaultValue: false, description: 'Clean workspace before build')
    }

    stages {

        stage('Show Input') {
            steps {
                echo "VERSION = ${params.VERSION}"
                echo "ENV = ${params.ENV}"
                echo "CLEAN = ${params.CLEAN}"
            }
        }

        stage('Clean Workspace') {
            when {
                expression { return params.CLEAN == true }
            }
            steps {
                echo "Cleaning workspace..."
                deleteDir()
            }
        }

        stage('Build') {
            steps {
                echo "Building version ${params.VERSION} for ${params.ENV}"
                sh 'echo Building application...'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to ${params.ENV}"
                sh 'echo Deploy step running...'
            }
        }
    }
}
