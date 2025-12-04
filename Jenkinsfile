pipeline {
  agent any
  options { timeout(time: 5, unit: 'MINUTES') } // safety timeout for whole build
  stages {
    stage('Node & env') {
      steps {
        echo "NODE_NAME: ${env.NODE_NAME}"
        echo "NODE_LABELS: ${env.NODE_LABELS}"
        sh '''
          echo "===== Shell location & PATH ====="
          echo "which sh: $(which sh || echo not-found)"
          echo "which docker: $(which docker || echo not-found)"
          echo "docker binary ls:"
          ls -la $(which docker 2>/dev/null) || true
          echo "PATH: $PATH"
          echo "User: $(whoami)"
          echo "ps aux | grep -i Docker (host):"
          ps aux | grep -i Docker || true
        '''
      }
    }

    stage('Docker quick check') {
      steps {
        script {
          // small guarded block so docker commands don't hang forever
          try {
            timeout(time: 30, unit: 'SECONDS') {
              sh '''
                echo "Trying docker --version (30s timeout)"
                docker --version || echo "docker --version failed (exit=$?)"
                echo "Trying docker info (30s timeout) --- printing limited output"
                docker info --format '{{json .}}' | head -c 200 || echo "docker info failed"
              '''
            }
          } catch (err) {
            echo "docker CLI timed out or failed: ${err}"
          }
        }
      }
    }

    stage('Try start Docker Desktop (if not running)') {
      steps {
        script {
          // Only attempt if docker wasn't found / not working
          def dockerFound = sh(script: "which docker >/dev/null 2>&1 && echo yes || echo no", returnStdout: true).trim()
          if (dockerFound == 'no') {
            echo "docker binary not found in PATH available to Jenkins — add /opt/homebrew/bin or the docker path to Jenkins Global properties."
          } else {
            // If docker exists but daemon not responding we try to open Docker.app (best-effort)
            def ok = sh(script: "docker info >/dev/null 2>&1 && echo ok || echo no", returnStdout: true).trim()
            if (ok == 'ok') {
              echo "Docker daemon reachable already."
            } else {
              echo "Docker CLI present but daemon not reachable. Attempting to start Docker Desktop app (open -a Docker) and waiting up to 60s..."
              sh '''
                open -a Docker || echo "open command failed (maybe Docker.app not installed)"
                SECONDS=0
                while [ $SECONDS -lt 60 ]; do
                  docker info >/dev/null 2>&1 && { echo "docker daemon ready after ${SECONDS}s"; exit 0; }
                  sleep 2
                done
                echo "Timed out waiting for docker daemon (60s)."
              '''
            }
          }
        }
      }
    }
  }
  post {
    always { echo "Diagnostic pipeline finished" }
  }
}
