pipeline {
    agent any
    environment {
        VENV_DIR = "env"
        RECIPIENTS = "YOUR EMAIL HERE"
        FLASK_PORT = "5000"
    }

    stages {

        stage('Build') {
            steps {
                script {
                    if (fileExists('requirements.txt')) {
                        if (isUnix()) {
                            sh "python3 -m venv $VENV_DIR"
                            sh "source $VENV_DIR/bin/activate && pip install -r requirements.txt"
                        } else {
                            bat "python -m venv %VENV_DIR%"
                            bat "call %VENV_DIR%\\Scripts\\activate && pip install -r requirements.txt"
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh "echo 'Testing the Flask App...'"
                        sh "source $VENV_DIR/bin/activate && pytest test_app.py"
                    } else {
                        bat "echo Testing the Flask App..."
                        bat "call %VENV_DIR%\\Scripts\\activate && pytest test_app.py"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (isUnix()) {
                        sh "echo 'Deploying the Flask App on port $FLASK_PORT...'"
                        sh "source $VENV_DIR/bin/activate && nohup python app.py > flask.log 2>&1 &"
                    } else {
                        bat "echo Deploying the Flask App on port %FLASK_PORT%..."
                        bat "call %VENV_DIR%\\Scripts\\activate && start /B python app.py"
                    }
                    sleep time: 1, unit: 'MINUTES'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deploy succeeded!'
            script {
                mail to: "${RECIPIENTS}",
                     subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                     body: "✅ Build succeeded.\nSee: ${env.BUILD_URL}"
            }
        }
        failure {
            echo '❌ Build or Deploy failed.'
            script {
                mail to: "${RECIPIENTS}",
                     subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                     body: "❌ Build failed.\nSee: ${env.BUILD_URL}"
            }
        }
    }
}
