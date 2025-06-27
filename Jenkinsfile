pipeline {
    agent {
        docker {
            image 'python:3.10' 
            args '-u root'      
        }
    }

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Setup Python Virtual Env') {
            steps {
                echo 'Setting up virtual environment...'
                sh 'apt-get update && apt-get install -y python3-venv'
                sh 'python -m venv $VENV_DIR'
                sh '. $VENV_DIR/bin/activate && pip install --upgrade pip'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing required packages...'
                sh '. $VENV_DIR/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Test Run') {
            steps {
                echo 'Running a dry test (loading model)...'
                sh '. $VENV_DIR/bin/activate && python test_model.py'
            }
        }

        stage('Lint Check (optional)') {
            steps {
                echo 'Checking Python code quality...'
                sh '. $VENV_DIR/bin/activate && pip install pylint && pylint *.py || true'
            }
        }
    }

    post {
        success {
            echo '✅ Build and test successful!'
        }
        failure {
            echo '❌ Build failed. Check logs.'
        }
    }
}
