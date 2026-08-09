pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t mini-cicd-app:v1 .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag mini-cicd-app:v1 $DOCKER_USER/mini-cicd-app:v1
                        docker push $DOCKER_USER/mini-cicd-app:v1
                        docker logout
                    '''
                }
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