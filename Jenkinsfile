pipeline {
    agent any   // no labels
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
}
