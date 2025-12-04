pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo "Hello World from Jenkins!"
            }
        }
    }
    post {
    success {
      echo "Build & tests succeeded"
    }
    failure {
      echo "Build or tests failed"
    }
  }
}
