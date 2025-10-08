pipeline {
  agent any

environment {
    DEPLOY_DIR = 'C:\\inetpub\\wwwroot'
}

  stages {
    stage('Checkout') {
      steps {
        echo 'Cloning project from GitHub...'
        // Use credentials ID created earlier
        git branch: 'main', url: 'https://github.com/pruthvirajpatil07/web-app'
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
        bat """
  if not exist "%DEPLOY_DIR%" (
    echo ERROR: Deploy directory %DEPLOY_DIR% not found
    exit /b 1
  )
  echo Copying files to %DEPLOY_DIR%
  xcopy * "%DEPLOY_DIR%\\" /E /Y
"""

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
