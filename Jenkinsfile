pipeline {
  agent {
    docker {
      image 'python:3.11'
      // run as root inside container to avoid permission problems creating venv/workspace files
      args '-u root:root'
    }
  }

  environment {
    VENV = "${WORKSPACE}/venv"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Install deps') {
      steps {
        // create venv and install requirements
        sh "python -m venv ${VENV}"
        sh ". ${VENV}/bin/activate && pip install -r requirements.txt"
      }
    }

    stage('Run tests') {
      steps {
        // produce a JUnit XML so Jenkins' junit step can pick up results
        sh ". ${VENV}/bin/activate && pytest -q --junitxml=report.xml"
      }
      post {
        always {
          junit allowEmptyResults: true, testResults: 'report.xml'
        }
      }
    }

    stage('Build artifact') {
      steps {
        // try to build a wheel; if there's no setup.py keep going
        sh ". ${VENV}/bin/activate && python -m pip install wheel && python setup.py bdist_wheel || echo 'skip build'"
        archiveArtifacts artifacts: 'dist/*.whl, requirements.txt', fingerprint: true, allowEmptyArchive: true
      }
    }
  }

  post {
    success { echo "Pipeline completed successfully" }
    failure { echo "Pipeline failed" }
  }
}
