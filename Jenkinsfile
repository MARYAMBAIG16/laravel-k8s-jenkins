pipeline {
    agent any

    stages {
        stage('Checkout GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Validate Kubernetes Files') {
            steps {
                sh 'kubectl apply --dry-run=client -f k8s/'
            }
        }

        stage('Deploy MySQL') {
            steps {
                sh 'kubectl apply -f k8s/mysql.yaml'
                sh 'kubectl rollout status deployment/mysql --timeout=180s'
            }
        }

        stage('Deploy Laravel') {
            steps {
                sh 'kubectl apply -f k8s/laravel.yaml'
                sh 'kubectl rollout status deployment/laravel-app --timeout=180s'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods,services'
            }
        }
    }
}
