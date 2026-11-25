
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out Django project from GitHub'
                checkout scm
            }
        }

        stage('Python Setup') {
            steps {
                sh '''
                    python3 --version
                    python3 -m venv pipeline-venv
                    . pipeline-venv/bin/activate
                    pip install --upgrade pip
                    pip install -r project4/requirements.txt
                    pip install bandit pip-audit
                '''
            }
        }

        stage('Dependency Scan') {
            steps {
                sh '''
                    . pipeline-venv/bin/activate
                    pip-audit -r project4/requirements.txt
                '''
            }
        }

        stage('SAST - Bandit') {
            steps {
                sh '''
                    . pipeline-venv/bin/activate
                    bandit -r project4/project4 -ll
                '''
            }
        }

        stage('Django Security Check') {
            steps {
                sh '''
                    . pipeline-venv/bin/activate
                    cd project4
                    python manage.py check --deploy
                '''
            }
        }
    }

    post {
        success {
            echo 'DevSecOps pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}

