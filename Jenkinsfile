
pipeline {
    agent any

    stages {
        stage('Validate Kubernetes') {
            steps {
                bat 'kubectl version --client'
            }
        }

        stage('Deploy Application') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl apply -f deployment.yaml'
                }
            }
        }

        stage('Wait for Readiness') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl rollout status deployment/readiness-deployment --timeout=180s'
                }
            }
        }

        stage('Verify Readiness Probe') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl get deployment readiness-deployment'
                    bat 'kubectl get pods -l app=readiness-app'
                    bat 'kubectl describe deployment readiness-deployment'
                }
            }
        }
    }
}
