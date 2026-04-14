pipeline {
    agent any
    
    environment {
        // Define virtualenv path to keep the workspace clean
        VENV = ".venv"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gglavanya/DEVOPS_CI.git'
            }
        }

        stage('Setup Environment') {
            steps {
                sh '''
                    python3 -m venv $VENV
                    $VENV/bin/pip install -r requirements.txt
                    $VENV/bin/pip install pytest flake8 ansible
                '''
            }
        }

        stage('Lint (flake8)') {
            steps {
                // flake8 checks for syntax and style errors
                sh '$VENV/bin/flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics'
            }
        }

        stage('Test (pytest)') {
            steps {
                sh '$VENV/bin/pytest'
            }
        }

        stage('Configuration (Ansible)') {
            steps {
                // Use Ansible to prepare the target server
                sh '$VENV/bin/ansible-playbook -i inventory.ini deploy.yml'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Add your final deployment commands here
            }
        }
    }
}
