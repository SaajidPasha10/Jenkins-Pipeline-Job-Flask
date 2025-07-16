pipeline {
  agent any

  tools {
    git 'DefaultGit'
  }

  environment {
    VENV_DIR = 'venv'
    RECIPIENTS = 'saajidpasha@gmail.com'
  }

  stages {
    stage('Prepare') {
      steps {
        echo "Running on ${isUnix() ? 'Unix-like' : 'Windows'} node"
      }
    }

    stage('Set Up Environment') {
      steps {
        script {
          if (isUnix()) {
            sh """
              python3 -m venv \$VENV_DIR
              . \$VENV_DIR/bin/activate
              pip install --upgrade pip
              pip install -r requirements.txt
            """
          } else {
            bat """
              python -m venv %VENV_DIR%
              call %VENV_DIR%\\Scripts\\activate
              pip install --upgrade pip
              pip install -r requirements.txt
            """
          }
        }
      }
    }

    stage('Run Tests') {
      steps {
        script {
          if (isUnix()) {
            sh """
              . \$VENV_DIR/bin/activate
              pytest test_app.py
            """
          } else {
            bat """
              call %VENV_DIR%\\Scripts\\activate
              pytest test_app.py
            """
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        script {
          if (isUnix()) {
            sh """
              echo "Deploying Flask app on Unix..."
              . \$VENV_DIR/bin/activate
              nohup python app.py > flask.log 2>&1 &
            """
          } else {
           
              bat """
              echo Deploying Flask app on Windows...
              call %VENV_DIR%\\Scripts\\activate
              powershell -Command "Start-Process python 'app.py'"
            """
              sleep(3000)
          
        }
      }
    }
  }

  post {
    success {
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
