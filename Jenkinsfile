pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t mini-cicd-app:${BUILD_NUMBER} .'
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
                        docker tag mini-cicd-app:${BUILD_NUMBER} $DOCKER_USER/mini-cicd-app:${BUILD_NUMBER}
                        docker push $DOCKER_USER/mini-cicd-app:${BUILD_NUMBER}
                        docker logout
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker rm -f mini-cicd-test 2>/dev/null || true
                    docker run -d --name mini-cicd-test -p 8082:80 mini-cicd-app:${BUILD_NUMBER}

                    sleep 3

                    curl -f http://localhost:8082

                    docker rm -f mini-cicd-test
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    kubectl --kubeconfig=/var/lib/jenkins/.kube/config apply -f k8s/
                '''
            }
        }
    }
}