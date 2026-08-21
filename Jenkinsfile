pipeline {
    agent any

    environment {
        JFROG_URL = 'https://trialn4vk2g.jfrog.io'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate

                    pip install -r requirements.txt
                    pip install build twine
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    PYTHONPATH=. pytest
                '''
            }
        }

        stage('Build Package') {
            steps {
                sh '''
                    . venv/bin/activate
                    python -m build
                '''
            }
        }

        stage('Publish to JFrog') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'jfrog-user',
                        variable: 'JFROG_USER'
                    ),
                    string(
                        credentialsId: 'jfrog-token',
                        variable: 'JFROG_TOKEN'
                    )
                ]) {
                    sh '''
                        . venv/bin/activate

                        python -m twine upload \
                          --repository-url "${JFROG_URL}/artifactory/api/pypi/python-local" \
                          -u "$JFROG_USER" \
                          -p "$JFROG_TOKEN" \
                          dist/*
                    '''
                }
            }
        }
    }
}