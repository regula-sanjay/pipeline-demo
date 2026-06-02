pipeline {
    agent any

    parameters {
        string(name: 'VERSION', defaultValue: '1.0', description: 'Enter build version')

        choice(name: 'ENV', choices: ['dev', 'test', 'prod'], description: 'Select environment')

        booleanParam(name: 'CLEAN', defaultValue: false, description: 'Clean workspace before build')

        choice(name: 'BRANCH', choices: ['main', 'dev', 'test'], description: 'Select Git branch to build')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH}",
                    url: 'https://github.com/sanjayregula/demo-pipeline.git'
            }
        }

        stage('Show Input') {
            steps {
                echo "BRANCH = ${params.BRANCH}"
                echo "VERSION = ${params.VERSION}"
                echo "ENV = ${params.ENV}"
                echo "CLEAN = ${params.CLEAN}"
            }
        }

        stage('Clean Workspace') {
            when {
                expression { params.CLEAN == true }
            }
            steps {
                deleteDir()
            }
        }

        stage('Build') {
            steps {
                echo "Building branch ${params.BRANCH} version ${params.VERSION} for ${params.ENV}"
                sh 'echo Building application...'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${params.BRANCH} to ${params.ENV}"
                sh 'echo Deploy step running...'
            }
        }
    }
}
