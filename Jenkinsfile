pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHONPATH = "${env.WORKSPACE}"
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pytest"
                }
            }
        }
        stage('Code Coverage') {
            steps {
                script {
                    // Activate the environment, install pytest if necessary, and run coverage
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install pytest && coverage run -m pytest"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && coverage report"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && coverage html"
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && bandit -r ."
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
