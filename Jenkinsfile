pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
        PYTHONPATH = "${env.WORKSPACE}"
        PYTHONIOENCODING = 'utf-8'  // Set UTF-8 encoding for all Python output
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pip freeze"  // List installed packages
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
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install pytest"  // Ensure pytest is available
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && coverage run -m pytest"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && coverage report"
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && coverage html"
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    // Adding --exit-zero to Bandit command to avoid pipeline failure
                    bat "${VIRTUAL_ENV}\\Scripts\\activate && bandit -r . > bandit_report.txt 2>&1"
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
            // Display the Bandit report in the logs for manual review
            script {
                if (fileExists('bandit_report.txt')) {
                    echo 'Bandit Report:'
                    bat "type bandit_report.txt"
                }
            }
            cleanWs()
        }
    }
}
