pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo "Hello World from Jenkins!!"
            }
        }
        stage('Fetch Code') {
            steps {
                git branch: 'main', url: 'https://github.com/keshariGaurav/jenkins-practice-project'
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
