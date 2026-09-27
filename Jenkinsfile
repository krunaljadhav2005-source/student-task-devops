pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv-jenkins
                    ./venv-jenkins/bin/pip install --upgrade pip
                    ./venv-jenkins/bin/pip install -r requirements.txt
                    ./venv-jenkins/bin/pip install pytest
                '''
            }
        }

        stage('Test') {
            steps {
                sh './venv-jenkins/bin/pytest'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t krunal123/student-task-app:latest .'
          }
        }

        stage('Load Image to Minikube') {
            steps {
                sh 'minikube image load krunal123/student-task-app:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                    kubectl rollout restart deployment student-task-app
                '''
            }
        }

        stage('Check Deployment') {
            steps {
                sh '''
                    kubectl rollout status deployment/student-task-app
                    kubectl get pods
                    kubectl get services
                '''
            }
        }
    }
}
