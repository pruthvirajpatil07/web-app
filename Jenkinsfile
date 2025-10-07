pipeline {
  agent any

  environment {
    // change DEPLOY_DIR depending on OS
    // Linux example: /var/www/html
    // Windows example: C:\\inetpub\\wwwroot
    DEPLOY_DIR = '/var/www/html'
  }

  stages {
    stage('Checkout') {
      steps {
        echo 'Cloning project from GitHub...'
        // Use credentials ID created earlier
        git branch: 'main', url: 'https://github.com/<your-username>/<repo-name>.git'
      }
    }

    stage('Build') {
      steps {
        echo 'Build step: verify workspace files'
        sh 'ls -la'
      }
    }

    stage('Test') {
      steps {
        echo 'Validating HTML with tidy (if installed)'
        // tidy command will fail if not installed; see notes for installation
        sh '''
          if command -v tidy >/dev/null 2>&1; then
            tidy -e index.html || true
          else
            echo "tidy not found; skipping HTML validation"
          fi
        '''
      }
    }

    stage('Deploy') {
      steps {
        echo "Deploying files to ${env.DEPLOY_DIR}"
        // Use platform-appropriate copy command
        sh '''
          if [ -d "${DEPLOY_DIR}" ]; then
            sudo cp -r * ${DEPLOY_DIR}/
          else
            echo "Deploy dir ${DEPLOY_DIR} not found — create or update DEPLOY_DIR"
            exit 1
          fi
        '''
      }
    }

    stage('Smoke Test (Optional)') {
      steps {
        echo 'Simple smoke test: curl the deployed index.html'
        sh 'sleep 1; curl -I http://localhost/index.html || true'
      }
    }
  }

  post {
    success {
      echo 'Pipeline finished successfully!'
    }
    failure {
      echo 'Pipeline failed — check console output'
    }
  }
}
