pipeline {
  agent { label 'master' }   // or replace 'master' with the node name printed earlier
  stages {
    stage('Hello') { steps { echo "Hello World from Jenkins!!" } }
    stage('Fetch Code') { steps { git branch: 'main', url: 'https://github.com/keshariGaurav/jenkins-practice-project' } }
    stage('Docker Test') { steps { sh 'docker --version' } }
  }
}
