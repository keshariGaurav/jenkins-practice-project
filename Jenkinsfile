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
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("kesharigaurav97/jenkinsapp:latest")
                }
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
