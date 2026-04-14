pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/gglavanya/DEVOPS_CI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'pytest'
            }
        }

        stage('Code Quality Check (Lint)') {
            steps {
                sh 'flake8 app/'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Build completed (no compile needed for Python)"'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'ansible-playbook -i inventory.ini deploy.yml -e env=staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                input "Approve Production Deployment?"
                sh 'ansible-playbook -i inventory.ini deploy.yml -e env=production'
            }
        }
    }

    post {
        success {
            echo "SUCCESS"
        }
        failure {
            echo "FAILED"
        }
    }
}
