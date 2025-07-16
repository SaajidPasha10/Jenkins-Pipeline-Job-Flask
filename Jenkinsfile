pipeline {
  agent any

  environment {
    VENV_DIR = 'venv'
  }
  tools {
  git 'DefaultGit'
}

  stages {
   

    stage('Set Up Environment') {
      steps {
        sh '''
          python3 -m venv $VENV_DIR
          source $VENV_DIR/bin/activate
          pip install --upgrade pip
          pip install -r requirements.txt
        '''
      }
    }

    stage('Run Tests') {
      steps {
        sh '''
          source $VENV_DIR/bin/activate
          pytest test_app.py
        '''
      }
    }

    stage('Deploy') {
      steps {
        sh '''
          echo "Simulating deployment..."
          echo "You could use gunicorn or upload to server here"
        '''
      }
    }
  }

  post {
    success {
      echo '✅ Build & Deploy succeeded!'
    }
    failure {
      echo '❌ Build or Deploy failed.'
    }
  }
}
