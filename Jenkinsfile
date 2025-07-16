pipeline{
    agent any
    environment{
        ENV_DIR = "env"
        email_recepient = "saajidpasha@gmail.com"
        PORT=5000
    }
    stages{
        
        stage('Build'){
            steps{
                
                    script{
                        if(fileExists('requirements.txt')){
                            if(isUnix()){
                            sh 'python3 -m venv \$VENV_DIR'
                            sh '. \$VENV_DIR/bin/activate'
                            sh 'pip install -r requirements.txt'
                        }
                        
                        else{
                             bat 'python3 -m venv \$VENV_DIR'
                            bat '. \$VENV_DIR/bin/activate'
                            bat 'pip install -r requirements.txt'
                        }
                    }
                }
            }
        }

        stage('Test'){
            steps{
                script{
                    if(isUnix()){
                        sh 'echo "Testing the Flask App........"'
                        sh '. \$VENV_DIR/bin/activate'
                        sh 'pytest test_app.py'
                    }
                    else{
                        bat 'echo "Testing the Flask App........"'
                        bat '. \$VENV_DIR/bin/activate'
                        bat 'pytest test_app.py'
                    }
                }
            }
        }

         stage('Deploy'){
            steps{
                script{
                    if(isUnix()){
                        sh 'echo "Deploying the Flask App on $PORT ........"'
                        sh '. \$VENV_DIR/bin/activate'
                        sh ' nohup python app.py > flask.log 2>&1 &'
                    }
                    else{
                        bat 'echo "Deploying the Flask App on $PORT ........"'
                        bat '. \$VENV_DIR/bin/activate'
                        bat 'start /B cmd /c "set FLASK_APP=%FLASK_APP% && %VENV_DIR%\\Scripts\\flask run   --host=0.0.0.0 --port=%FLASK_PORT%"'
                    }
                    sleep(3000)
                }
            }
        }
    }

post{
    success{
        echo '✅ Build & Deploy succeeded!'
        mail to: "${RECIPIENTS}",
        subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: "✅ Build succeeded.\nSee: ${env.BUILD_URL}"
    }
    failure {
      echo '❌ Build or Deploy failed.'
      mail to: "${RECIPIENTS}",
           subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "❌ Build failed.\nSee console: ${env.BUILD_URL}"
    }
}
}
