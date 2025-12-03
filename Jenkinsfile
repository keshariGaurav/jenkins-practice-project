pipeline {
  agent { docker 'python:3.11' }

  environment {
    VENV = "${WORKSPACE}/venv"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Install deps') {
      steps {
        sh 'python -m venv $VENV'
        sh '. $VENV/bin/activate && pip install -r requirements.txt'
      }
    }
    stage('Run tests') {
      steps { sh '. $VENV/bin/activate && pytest -q' }
      post {
        always { junit allowEmptyResults: true, testResults: '**/test-*.xml' }
      }
    }
    stage('Build artifact') {
      steps {
        sh '. $VENV/bin/activate && python -m pip install wheel && python setup.py bdist_wheel || echo "skip build"'
        archiveArtifacts artifacts: '**/*.py, **/requirements.txt', fingerprint: true
      }
    }
  }
  post {
    success { echo "Pipeline completed successfully" }
    failure { echo "Pipeline failed" }
  }
}
