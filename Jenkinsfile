pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Code has been checked out'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t mini-cicd-app:v1 .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application'
            }
        }
    }
}